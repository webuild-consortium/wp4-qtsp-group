# 1\. Executive summary

The European Digital Identity (EUDI) Wallet introduces a common framework for the provision and use of electronic identity and trust services across the European Union. Within this framework, electronic signatures and electronic seals remain governed by the eIDAS Regulation and the associated ETSI standards that define trust service requirements and assurance levels.

This report provides a technical overview of signing and sealing models relevant to the EUDI Wallet ecosystem, with a focus on the interaction between wallets, relying parties, and Qualified Trust Service Providers (QTSPs). Particular attention is given to the use of remote signing and sealing services and to the role of the Cloud Signature Consortium (CSC) API as a standardised interface for accessing such services.

Two architectural approaches are analysed: a wallet-centric signing model, where the wallet orchestrates the authorization of the user without relying on an identity provider supporting the QTSP, and a QTSP-centric model, where the trust service provider retains primary control over identity management, key custody, and signature or seal activation although ensuring that it only happens under the sole control of the user. The report clarifies that these models are not mutually exclusive and may coexist within the same ecosystem, serving different functional and regulatory purposes.

In this context, the report describes how CSC can be used as an interoperability layer to expose remote signing or sealing services without altering the underlying trust model or regulatory responsibilities defined by eIDAS.

Throughout the document, relevant regulatory and standardisation references are identified, including eIDAS regulation, ETSI standards, and the EUDI Wallet Architecture and Reference Framework. The report does not aim to redefine these frameworks, but rather to position existing, compliant trust service architectures within the emerging EUDI Wallet landscape.

By providing a clear separation between trust, control, and technical integration layers, this report aims to support informed architectural decisions and facilitate interoperable, standards-based deployment of signing and sealing services in the EUDI Wallet ecosystem.

# 2\. Introduction

## 2.1 Scope and goal

The scope of this document is to provide an analysis of electronic signature and electronic seal methods in the context of the European Digital Identity (EUDI) Wallet and European Business Wallet ecosystems, with particular focus on their technical realisation and regulatory alignment under the eIDAS framework. The document will also provide the rationale for choosing signing and sealing scenarios for WE BUILD.

It is assumed that the process of identification and issuance of the corresponding certificates has already been completed, and therefore, this document will focus on the analysis of the chosen signing and sealing methods for their technical realization and regulatory alignment.

_NOTE: The statement above to be rechecked. Enrolment to the QTSP with certificate issuance can be brought into scope later for both long-lived and one-shot, short-lived certificates._

The document examines signing and sealing models applicable to the EUDI Wallet, including both Wallet-centric and QTSP-centric approaches, and analyses how these models interact with existing trust service infrastructures. Special attention is given to remote signing and sealing services and to the use of the Cloud Signature Consortium (CSC) API as a standardised interface for accessing such services.

The goal of this report is to:

- Clarify the roles and responsibilities of wallets, relying parties, and Qualified Trust Service Providers (QTSPs);
- Describe applicable architectural patterns for advanced and qualified electronic signatures and seals;
- Assess regulatory considerations and standard compliance without redefining existing legal or technical frameworks.
- Choose signing and sealing scenarios to be used in the use cases piloted in WE BUILD.

This document does not aim to specify wallet implementations or user experience flows.

## 2.2 Target audience

This document is intended for a technical and regulatory audience involved in the design, implementation, assessment, or integration of signing and sealing services within the EUDI Wallet ecosystem, including:

- Wallet architects and implementers;
- Qualified Trust Service Providers (QTSPs) and trust service operators;
- Public and private relying parties consuming signature or seal services;
- Public and private EAA/QEAA issuers consuming seal services from an external QTSP;
- Regulatory, conformity assessment, and standardisation stakeholders.

The reader is expected to be familiar with public key infrastructures, electronic signatures and seals, and the general principles of the eIDAS Regulation and ETSI trust service standards.

# 3\. Overview of how to sign using the EUDI Wallet

NOTE: Consider explanation and use of terminology such as driving application, signature creation application etc. (from standards) in case we will use that later for diagrams and such.

In the We Build context, signing with the EU Digital Identity Wallet (EUDI Wallet / EUDIW) is best described as a transaction-orchestrated signing process initiated by a relying party (e.g., a contracting platform, procurement system, bank onboarding flow, or public-sector portal), with the wallet acting primarily as a user-controlled authentication and authorization component that can also convey attributes needed for the transaction (e.g., identity data and, where applicable, representation/mandate evidence).

From a functional architecture standpoint, We Build can treat “signing using the EUDI Wallet” as a composition of three building blocks:

First, a **signature creation application / signing service** (which may be operated by the relying party, a neutral signing service provider, or a trust service provider) prepares the signing transaction: it gathers the document(s), determines the required signature level (QES vs AES), selects required formats (e.g. PAdES for PDFs), and defines the process constraints (signature policy, ordering of signers, required timestamps, etc.). This matches observations of previous LSPs (such as EWC) that a signing use case must specify “what/why/who/how” and that real processes may involve multiple documents, multiple signers, synchronous or asynchronous steps, and even additional non-signing roles (reviewers/approvers).

Second, the EUDI Wallet contributes high-assurance user binding and user consent. Under the European Digital Identity framework (Regulation (EU) 2024/1183), EUDI Wallets are to be provided under an electronic identification scheme with assurance level high, and must enable users to sign with qualified electronic signatures (and seal with qualified electronic seals). In We Build business transactions, this means the wallet can be used to authenticate the natural person signer and, where relevant, to help prove that the signer is an authorized representative of an economic operator (a need that is explicitly central for Business Wallets).

Third, the actual cryptographic QES operation is typically executed by either (a) a local/external QSCD or (b) a remote qualified signature creation device (remote QSCD) operated under a qualified trust service. The remote option is the most mature and scalable for cross-border business transactions and is consistent with the EWC “first solutions” approach that uses the wallet for authentication/authorization while calling a remote signing service via standardized interfaces. For remote signing interoperability, We Build should explicitly align with CSC API v2.2: the remote signing service exposes endpoints under a base URI ending in /csc/v2 (with OAuth endpoints allowed to be separate), and supports remote signature creation via methods such as credentials/authorize (to obtain Signature Activation Data) and signatures/signHash or signatures/signDoc.

Two certificate lifecycle patterns are particularly relevant for We Build:

A long-term credential pattern, where the signer already has an existing qualified certificate and signing key managed (often remotely) and the signing transaction mainly performs signature authorization and creation. This is one of the two approaches for near-term deployment.

A “one-shot / on-the-fly” credential pattern, where a key pair and certificate (often short-lived) are issued as part of the signing session. This could be the second prioritized approach and notes it is conceptually aligned with targeted, context-dependent identity usage. In We Build, this pattern can be particularly attractive for business workflows where minimizing long-lived credentials reduces operational friction, provided the QTSP’s identity proofing and linking requirements are met (see “Signing processes and requirements” below).

For the Wallet-centric approach, the Wallet must expose a signing API that can be used by the signing service provider. This API can also be used by another Wallet as client to set up signing flows between Wallets and their users without any signing service. While such flows are expected to be rare, there are use cases where persons sign documents between them, not involving any service provider.

Two alternatives are envisaged for Wallet-centric signing. In the first alternative, the signing service sends the entire document to sign to the Wallet, which in this case must have an embedded signature creation application. The signing is carried out within the Wallet using a remote QSCD service known to the Wallet. Alternatively, the Wallet can interact on the device with an external QSCD such as an NFC enabled smart card held against the device. If in the future devices come with embedded QSCD hardware, this can be used to sign entirely locally; currently hardware on devices can only support AES. The signed document is returned to the signing service, which may add timestamps and such if not done by the Wallet.

In the second alternative, which is currently not foreseen by the ARF, the signing service carries out all operations including hash value computation, and only the hash value is sent to the Wallet. The Wallet ensures that the hash value is signed using a remote QSCD service (or external QSCD) and returns signed hash to the signature service for packaging with the document to sign.

## 3.1 Signing processes and requirements

A We Build signing process should be described as a business transaction flow that results in one or more legally effective signed (and possibly sealed) electronic documents. Each use case should capture at least: what documents are signed, why signatures are required (legal effect), who signs (identities, roles, authorizations), and how signing is performed (signature level and format constraints).

Because We Build focuses on business transactions, the signing requirements go beyond “single user signs a single file.” Considering that signing processes can involve multiple documents, multiple signers, mixed signing means (wallet and non-wallet users), and ordering constraints; importantly, some signature formats (notably signed PDFs in PAdES) require sequential packaging of multiple signatures. This matters directly for We Build B2B/B2G scenarios such as: multi-party contracts, framework agreements with annexes, procurement submissions with declarations, and workflows where the organization counter-signs after an employee signs.

From a process perspective, We Build should support both synchronous and asynchronous signing. Previous LSPs’s findings note that signing may be synchronous (embedded in a continuous online flow) or asynchronous (signers return later), and that in multi-signer setups only one party can practically sign “synchronously” at a time. In the We Build context it should therefore be assumed that the signature creation application maintains a server-side transaction state (documents, signing order, which signatures are present, remaining signers, expiry times) and can resume the flow without losing integrity.

From this process perspective, Wallet-centric signing, at least when the entire document is sent to the Wallet, should be treated as asynchronous signing. When the document is in the Wallet, it is unpredictable how much time the user will need to sign.

For We Build, representation and mandates are core. The EUBW proposal explicitly foresees that Business Wallets support delegation and role-based authorization: the wallet should incorporate a mandate/role-based authorization system for professional contexts and defines “mandate” as an authorization granted by a business wallet owner to an authorized representative to act on its behalf. It also anticipates that people acting on behalf of an entity should be able to execute attestations/declarations/documents through a legally valid electronic signature (per eIDAS definitions and effects). In practical We Build QES flows, this means the signature ceremony should bind not only (i) the signer identity, but also (ii) the signer’s capacity/authority to sign for the economic operator, where the transaction requires it. (This can be achieved by combining QES identity with attributes/mandate evidence and by including appropriate references in the transaction records and/or signed statements, depending on legal and business policy.)

On security and legal assurance, it should be explicitly distinguished: (a) wallet authentication (who is approving), (b) signature activation (how sole control/control is enforced), and (c) signature packaging/validation (how the relying party can validate over time).

For remote QES, “sole control” (for natural persons) is typically implemented through a signature activation mechanism where the user provides signature activation data (SAD) that is cryptographically and procedurally bound to the signing session. ETSI TS 119 431-1 (policy/security requirements for TSPs operating remote QSCDs) makes this concrete by referencing EN 419 241-1 clauses for signature activation data submission under sole control (natural persons) and by describing constraints such as linking a session to SAD(s), including multi-signature constraints (e.g., in PAdES, each new signature covers previous ones).

For interoperability, We Build should specify a CSC-based remote signing profile. CSC API v2.2 defines the remote service base URI pattern ending in /csc/v2 and supports credential authorization and signing through standard endpoints. In particular, CSC v2.2 allows QES intent to be expressed via a signature qualifier, and documents that signatureQualifier may be used (e.g., "eu_eidas_qes") and that at least one of credentialID or signatureQualifier must be present. This is helpful for We Build cross-border scenarios because it provides a standardized way for the signature creation application to request a QES outcome even when credential selection is abstracted.

Cryptographic requirements should be explicitly stated. CSC v2.2 requires hash algorithms “as strong or stronger than SHA-256” for relevant operations and recommends following ETSI algorithm guidance. At the signature creation application level, ETSI TS 119 101 requires correct preparation of DTBS/DTBSR (data-to-be-signed and its representation) and includes controls for bulk signing that are directly relevant to business workflows: the SCA should allow the signer to individually display documents in a bulk signing process and must ensure no unselected document is included.

Business Wallet–specific operational requirements should be integrated into the We Build signing flow description. Previous LSP’s requirements include Business Wallet providers to implement transaction logging policies that include, at minimum, electronic signing and sealing transactions (including unsuccessful ones) and specifies minimum logged data items (time/date, relying party identification, types of data requested/presented, and reasons for non-completion). For We Build, this implies that any QES integration with an EUBW front-end/back-end must define: what is logged by the wallet, what is logged by the signing service, how identifiers are correlated, and how logs are protected for integrity/confidentiality.

Finally, We Build should treat signature creation applications as composable and potentially multi-provider. The EUBW proposal contemplates that signature creation applications used by Business Wallet units may be provided by Business Wallet providers, trust service providers, or relying parties, and must support signing/sealing of user data and relying-party data, creation of mandatory/optional signature formats, and informing users of the result. This supports We Build deployment flexibility: an enterprise platform can embed its own signing application, rely on a QTSP’s signing front-end, or integrate a neutral signing portal, while still using CSC-based remote signing for the cryptographic operation.

## 3.2 Actors and roles

We Build business transactions involve both the “classic” e-signature roles and additional wallet-era roles related to mandates, identity, and transaction orchestration.

At minimum, the following mandatory roles are identified in a signing process: signer, relying party, and (qualified) certificate authority, and emphasizes that QES requires a qualified signature creation device (QSCD) that ensures only the signer can activate the signing key. What can be further distinguished is remote QES service (holding keys in remote QSCD/SCD) and a signing service provider / signature creation application that manages document presentation, consent, calls the signing means, packages signatures, and returns signed documents.

In We Build, these roles should be refined for business transactions as follows (the same entity may perform multiple roles, but the separation is useful for requirements and interfaces):

The signer is typically a natural person using an EUDI Wallet who applies a QES to a business document, often in a professional capacity (e.g., employee or legal representative). This corresponds to the “signer/signatory” role. Where the signer acts on behalf of an economic operator, the EUBW framework adds the concept of an authorized representative operating under a mandate/role-based authorization system. The economic operator/business wallet owner is the organization (or self-employed/sole trader) whose business processes and obligations are addressed by the transaction. The EUBW proposal defines a business wallet ecosystem oriented to economic operators and public sector bodies, with actions through the wallet intended to have equivalent legal effect to in-person or paper processes when using core functionalities.

The Business Wallet provider operates the European Business Wallet unit (front-end/back-end plus secure cryptographic components) and must meet technical and operational requirements including authentication gating and transaction logging. This provider may also provide (or integrate) a signature creation application, but the EUBW proposal explicitly allows signature creation applications to be provided by wallet providers, trust service providers, or relying parties.

The relying party is the actor relying on the resulting signatures for legal or business effect. This is often the service provider but can include third parties that receive and act on signed documents. In the EUBW proposal, a “European Business Wallet–relying party” is defined broadly as a natural person, economic operator, or public sector body that relies upon Business Wallets.

The signature creation application (signing service/orchestrator) is the component that runs the signing ceremony: it prepares the data-to-be-signed, presents documents, captures user consent, requests credential authorization, calls the remote signing endpoints, and packages the final signed documents. This maps directly to the “signing service provider” role and to the EUBW Annex’s functional expectations for signature creation applications.

If the document is sent to the Wallet in a Wallet-centric signing setup, the Wallet must implement at least a rudimentary signature creation application to present document, obtain consent, authorize signing, and package into final signed document.

The qualified trust service provider (QTSP) provides qualified certificates and (in remote signing) operates the qualified service for management of remote QSCDs. Certificates and remote QSCD can be provided by two different QTSPs but the two services go together. Under Regulation (EU) 2024/1183, remote QSCD management is explicitly treated as a qualified trust service and is recognized cross-border when provided in one Member State. ETSI TS 119 431-1 further elaborates security/policy requirements for TSP services operating remote QSCD/SCDev, including eID linking requirements (assurance level substantial or higher when linked to an eID means) and constraints on signature activation data and sessions.

The remote signing service interface provider exposes CSC API endpoints to enable interoperable integration. CSC v2.2 specifies the base URI structure ending in /csc/v2 and defines endpoint-based interactions for credential authorization and signature creation (e.g., credentials/authorize, signatures/signHash).

Optional but often required in business use cases are identity/attribute providers and brokers. Previous LSPs (EWC) describe an **identity provider (IDP)** as an intermediary that can authenticate users with the EUDI Wallet and relieve relying parties from direct wallet integration. In We Build, analogous intermediaries may exist for Business Wallet authentication, mandate verification, and/or attribute issuance needed for representation rights.

## 3.3 CSC API v2.2 implications for We Build

Because We Build QES integration is expected to lean on CSC-based remote signing, CSC API v2.2 terminology and endpoint structure should be considered rather than older CSC versions.

The CSC v2.2 specification states that a conforming remote signing service’s base URI must end with /csc/v2, and API method endpoints are concatenated to that base URI (with OAuth 2.0 endpoints optionally outside the base URI). Therefore, We Build text should consistently refer to endpoints in the form …/csc/v2/&lt;method&gt;, such as …/csc/v2/credentials/authorize and …/csc/v2/signatures/signHash.

For QES intent signaling, CSC v2.2 documents use of a signatureQualifier and gives "eu_eidas_qes" as an example denoting an eIDAS qualified electronic signature; it also requires that at least one of credentialID or signatureQualifier is present in relevant requests. This is directly useful for We Build cross-border business flows where the calling application may not pre-select a specific credential but still needs a QES-grade signature outcome.

For authorization and user consent binding, CSC v2.2 explicitly supports both OAuth-based and API-based credential authorization flows. The spec includes OAuth authorization request examples that carry parameters like credentialID or signatureQualifier, number of signatures, hashes, and the hash algorithm OID. It also specifies direct authorization via credentials/authorize, which can return a SAD (or a handle for asynchronous/out-of-band flows), and then SAD is used for actual signature creation.

On cryptographic hygiene, CSC v2.2 states that only hashing algorithms as strong or stronger than SHA-256 shall be used for relevant operations, and references ETSI recommendations. We Build should mirror this requirement in its “process and requirements” section to avoid under-specification. Cryptographic agility, the possibility to replace algorithms by new ones, is crucial to ensure that quantum-safe algorithms can be used in the future.

_3.4 Regulatory alignment points for EUDI Wallets and the EUBW proposal_

We Build’s QES section should explicitly claim alignment with two parallel but connected legal tracks: the European Digital Identity framework (eIDAS as amended) and the proposed European Business Wallets Regulation.

Under Regulation (EU) 2024/1183, European Digital Identity Wallets are required to be provided under an electronic identification scheme with assurance level high and must enable users to sign with qualified electronic signatures (and seal with qualified electronic seals). The same Regulation also introduces and structures qualified services for remote QSCD management and their cross-border recognition, which is directly relevant when We Build uses remote QSCD-based QES.

Under COM(2025) 838 final (EUBW proposal), Business Wallets are intended to support business-to-government and business-to-business interactions and include core functionalities explicitly covering qualified electronic signatures, qualified electronic seals, qualified timestamps, and authorization of multiple users to access and operate the Business Wallet (with management and revocation of those authorizations). The proposal’s Annex further requires that Business Wallet solutions can securely interface with local, external, or remotely managed qualified signature/seal creation devices and sets functional expectations for signature creation applications (including mandatory/optional formats and user notification). It also imposes transaction logging requirements that explicitly include electronic signing/sealing and require logging of unsuccessful transactions and their reasons.

For We Build business transactions, the strongest design implication is that “QES with EUDIW” should not be described as only an identity step; it must be described as a coordinated process where wallet authentication, mandate/representation management (when applicable), CSC-based remote credential authorization, signature creation, signature packaging, and wallet-level logging come together to achieve legally effective, auditable outcomes across borders.

# 4\. Wallet-centric signing model

This section considers wallet-centric signing models, in which the EUDI Wallet is the central component of the qualified electronic signature (QES) process. In particular, it analyses key aspects of the signing workflow, including the location of the private key and the signature creation device, as well as document handling and processing within the signature creation application. Based on these aspects, three distinct signing processes are identified within the wallet-centric model, which are shown in the section Detailed signing flows.

## 4.1 Actors

- Signer: A natural person that holds and utilizes an EUDI Wallet that wants to sign one or more documents in a signing process.
- Signer Creation Application (SCA): Signing service that is responsible for displaying and preparing and embedding the signature in the signed document.
- Remote QES service: Service for creating a signature using an (Q)SCD (Qualified signature creation device) and provided by a trust service provider.

## 4.2 Signing Flows High Level Overview

The scenarios below describe only the basic signing carried out by the wallet. In addition, collection of revocation status information, timestamping etc. May be needed. These functions can also be done by the wallet, but they may also be done by other actors later in the business process.

Remote signing with external SCA:  
The signing process is initiated by the user using wallet. The document is transmitted to an external Signature Creation Application (SCA), where it is displayed to the user for review and consent. Upon user approval, the document (or signature request) is forwarded to the remote Signature Creation Device (SCD) for signature creation.

Remote signing with local SCA:  
The signing process is initiated by the user using wallet. The document is displayed to the user directly within the wallet, which acts as the Signature Creation Application (SCA). After user consent, the document (or signature request) is sent to a remote Signature Creation Device (SCD) for signature creation.

Local signing:  
The signing process is initiated by the wallet. The document is displayed to the user within the wallet, which acts as the Signature Creation Application (SCA). Signature creation is performed locally using a Signature Creation Device (SCD) integrated into the user’s smartphone. Additionally, the QSCD could also be an external device such as a smart card.

Transfer of hash value only (out of scope of We Build):

The signing process is initiated at an external SCA. When the user has consented to signing, the document hash is computed by the SCA, and the hash value is transferred to the wallet. The hash value is signed initiated by the wallet by remote or local signing as described above, and the signed hash value is returned to the SCA for packaging.

## 4.3 Detailed Signing Flows

In this section, the three signing models are discussed in more detail. The description still remains on a higher level without going into the technical details. Additionally, the signing model could work with different types of certificates such as short term / one shot certificates which are generated on-the-fly, as well as long term certificates. However, the described models assume an already existing certificate.

The described models focus within their description on the natural person wallet. However, all models could work on the EWB as well. There might be some adoptions necessary depending on the actual architecture and implementation of the respective EBW. 4.3.1 Remote signing with external SCA

In this signing model are three actors involved namely the EUDI wallet including its holder, the signing services provider as well as the remote QES service. The Signing service as well as the remote QES service could be provided by the same party, naturally by an QTSP. They can also be provided by different providers.

The protocol used between the signing service and the remote QES service should be the CSC API version 2.

The remote signing process using an external SCA is illustrated in the Figure above with the following steps:

1.  As a first step, a user selects the document that they want to be signed. This document could be provided by a service provider such as an online service, or an email attachment, or stored in local or cloud storage of the user. In this step, the user selects the document and shares it with the EUDI wallet for signing.
2.  Next, the selected document is sent to the signing service that is related to the user EUDI wallet instance as well as the holder. In this step, also identification and authentication is performed but not discussed in detail here. The user is also redirected to the signing service which could be displayed in a browser either on the smartphone or via computer.
3.  The signing service receives provider receives the document in the signing request and prepares the document according to the correspondent signature type (e.g., PAdES, etc.). The document is displayed to the user which consents to proceeding with the signature process.
4.  The hash value to be signed is calculated by the signing service provider and send in a signing reuqest to the remote QES service.
5.  The remote QES service also contains an QSCD that is used to create the signature. In order to perform the signature, an authorization request is sent to the user.
6.  After approvement, the signed hash is returned from the remote QES service to the signing service.
7.  The signing service constructs the final signed docuement and returns it to the wallet.
8.  The wallet notifies the user about the successful signing process and asks for the storage location.

## 4.3.2 Remote signing with local SCA

In this signing model, the signing service including the signature creation device is part of the wallet solution. This implies that the communication to the remote QES service must be part of the wallet solution as well.

The remote signing model with local SCA is illustrated in the Figure above and the steps are described as follows:

1.  As a first step, a user selects the document that they want to be signed. This document could be provided by a service provider such as an online service, or an email attachment, or stored in local or cloud storage of the user. In this step, the user selects the document and shares it with the EUDI wallet for signing.
2.  The document is now shared with and handled by the internal SCA starting with preparing the document for signing according to the signature type (e.g., PAdES, etc.).
3.  The document is displayed to the user directly in the wallet which provides consent to continuing the signature process.
4.  Next, the hash to be signed is being calculated.
5.  The signature request is sent to the remote signing service including the hash.
6.  In order to access the remote QES service, commonly provided by a QTSP, the wallet must perform identification and authentication towards this service. Additionally, the wallet user must approve (authorize) the signature creation which is also covered within this step.
7.  The signed hash value is returned to the wallet.
8.  The SCA within the wallet solution embeds the signature value in the document and stores it wherever the user wishes.

## 4.3.3 Local signing

In this signing model, the so-called local signing model, all involved parties are now part of the wallet solution itself such as the SCA as well as the QSCD. The QSCD is responsible for creating the actual signature value as well as to manage and protect the related key material.

As mentioned earlier, the local signing model could also be implemented using an external QSCD such as by using a smart card.

The main benefit of this model is that the document never has to leave the device and no third party is involved. However, including all parts in the wallet solution comes with additional challenges when considering security. Furthermore, a QSCD is certified hardware which is currently not available on any mobile device and therefore, qualified signing is not possible using this model but advanced signing might.

The Figure above depicts the local signing model, which includes the following steps:

- Steps 1 – 4 stay the same as in the remote signing with local signing with local SCA model.
- Step 5: The hash is sent for signing to the internal QSCD.
- Step 6: In order to authorize the signature, a request for performing biometric authentication can be made.
- Step 7 – 8: The signature value is returned and embedded in the final document. This document is stored upon the users storage preference.

# 5\. QTSP-centric signing model

In a QTSP-centric signing model, the trust service provider remains responsible for the execution and governance of the signing or sealing process. The cryptographic key material used for signature or seal creation is generated, stored, and operated within infrastructure controlled bythe Qualified Trust Service Provider (QTSP), typically within secure hardware environments.

The signing process is therefore remote from the perspective of the user or calling application. External components interact with the trust service through defined service interfaces, while the actual cryptographic operation is performed within the QTSP’s controlled environment.

A typical deployment separates responsibilities across distinct logical components:

- A client-side application that interacts with the signer and prepares the data to be signed;
- An authorization component responsible for authenticating the signer and authorizing access to the cryptographic keys.;
- The remote signing service, which performs the cryptographic operation and manages credential lifecycle;
- The relying party that validates and consumes the resulting signature or seal.

These components may be operated by the same organisation or by different entities, but their responsibilities remain logically distinct. The separation of concerns ensures that user interaction, authorization, cryptographic execution, and trust validation can evolve independently while preserving regulatory compliance.

Within this architecture, the EUDI Wallet may act as the client-side orchestration component, at least for user authentication, conveying user intent and triggering a signing operation. However, it does not operate the signature creation environment, does not manage signing credentials, and does not assume the regulatory obligations of a trust service provider.

From a regulatory perspective, this model preserves established trust boundaries:

- The QTSP remains responsible for identity binding, credential issuance, and compliance with applicable ETSI standards.
- Signature or seal creation data remains under controlled conditions consistent with the required assurance level.
- Activation mechanisms enforce the conditions required for advanced or qualified signatures, including sole control where applicable.

The QTSP-centric model is therefore particularly suited for remote qualified electronic signatures, qualified electronic seals, and organizational signing processes, where governance, supervision, and conformity assessment must remain anchored in the trust service domain.

The following subsections present the flows for Qualified Electronic Signatures and Qualified Electronic Seals signing respectively.

## 5.1 Remote Qualified Electronic Signatures flow

Under the CSC specification for remote signing the main flows are authorization to access the API resources and the one to perform the signature itself.

## 5.1.1 Authorization to access API resources

The authorization flow to access the API resources other than signing related ones is the following:

1.  Internal authorization within the legal entity  
    <br/>A Legal Entity User (e.g. an employee) is authenticated and authorized within the organization’s Identity and Access Management (IAM) system to perform seal-related operations.  
    <br/>This authorization is internal to the legal entity and remains entirely outside the QTSP domain. The QTSP does not participate in, nor rely upon, this internal identity validation.

1.  Machine-to-machine token request

Acting on behalf of the legal entity, the Legal Entity Machine (client application) requests an access token from the Authorization Server of the QTSP offering the remote signature service by using the OAuth2 flow established for this cases which is the credentials flow with “service” scope.

## 5.1.2 Authorization to sign with external SCA

In this model, the Signature Creation Application (SCA) is operated by the QTSP as part of its remote signing infrastructure. The calling system does not construct the final signature container. Instead, it delegates document-based signing to the QTSP.

This differs from the digest-based model where an external SCA prepares the document and embeds the signature value. Here, the QTSP performs both the cryptographic signature operation and the construction of the final signature container (e.g., PAdES, CAdES, XAdES, JAdES).

5.1.2.1 Explicit authorization

Once the Legal Entity Machine has obtained a valid service access token, the signing process requires explicit authorization to use a specific signing credential. This step governs the activation of the credential and is distinct from both service-level API authorization and the final signature creation call.

The following diagram illustrates a credential authorization flow preceding signature execution:

1.  Initiation by the legal entity user

A Legal Entity User initiates a signing operation within the organization’s system, indicating the intention to use a company seal certificate to sign a document.

At this stage, the user interaction occurs within the legal entity domain (e.g. internal application, portal, or workflow system).

1.  Retrieval of available credentials

The Legal Entity Machine, acting as the client application and already holding a valid service access token, invoques /credentials/list including the access token with the appropriate service scope.

1.  Response with credential list

The Remote Signing Service Provider (RSSP) validates the token and returns the list of credentials available to the legal entity within the authorized context.

1.  Presentation of credentials to the user

The Legal Entity Machine presents the list of available signing credentials to the Legal Entity User.

The user is able to review and select the credential intended for the signing operation.

1.  Credential selection and document confirmation

The Legal Entity User selects the credential to be used (a qualified electronic seal certificate) and the document to be signed.

1.  Credential authorization request

The Legal Entity Machine invokes /credentials/authorize with a request body constructed in accordance with the CSC data model and includes the required authorization structure (authData) for explicit authorization. Is important to note that among other requisites the Qualified Seal is to be authorized using atleas two different factor authentication methods while specifying them inside authData to ensure SCAL2.

This request asks the RSSP to authorize the use of the specified credential for a forthcoming signature operation for a given document.

1.  Issuance of Signature Activation Data (SAD)

Upon successful authorization, the RSSP returns a Signature Activation Data (SAD. The Signature Activation Data (SAD) represents authorization to use the selected credential under the defined conditions and within a limited transaction scope.

The SAD will be required in the subsequent signature creation call.

5.1.2.2 OAuth2 authorization

\[to be completed\]

## 5.1.3 Signing a document with external SCA

In this model, the Signature Creation Application (SCA) is external from the user’s perspective and is operated by the QTSP as part of its remote signing infrastructure. The user or calling system submits the document and required authorization data to the QTSP, which performs the signing operation and returns the completed signed document.

The following diagram illustrates the interaction sequence:

The Legal Entity Machine, acting as the Driving Application, invokes /signatures/signDoc

The request includes:

- The Signature Activation Data (SAD) obtained in the previous authorization flow,
- The document to be signed,
- The request body structured in accordance with the CSC specification.

Then the Remote Signing Service Provider (QTSP-hosted SCA) validates the request, performs the signing operation, and returns the signed document.

Last but not least the Legal Entity Machine delivers the signed document to the Legal Entity User.

# 6\. Complex signing scenarios

\[to be completed\]

# 7\. Regulatory requirements

## 7.1 eIDAS Regulation and Implementing Acts

eIDAS (Regulation (EU) No 910/2014) establishes Qualified Electronic Signatures (QES) and Qualified Electronic Seals (QSeal) as the EU’s highest-assurance signature/seal instruments: QES has the same legal effect as a handwritten signature, while QSeal benefits from a presumption of integrity and correctness of origin for the sealed data.

Regulation (EU) 2024/1183 (often referred to as “eIDAS 2.0”) shifts the ecosystem from “trust services + optional national eID” toward a wallet-centered architecture, defining the European Digital Identity Wallet as an eID means that can sign with QES and seal with QSeal, and explicitly introducing remote qualified signature/seal creation device concepts and qualified trust services for the management of remote qualified devices, with explicit cross-border recognition rules.

Within that framework, the distinction between ordinary, advanced and qualified electronic signatures and seals is preserved. A qualified electronic signature (“QES”) remains, in substance, an advanced electronic signature created by a qualified electronic signature creation device and based on a qualified certificate for electronic signatures. A qualified electronic seal (“QESeal”) likewise remains the qualified version of the electronic seal used by legal persons to guarantee origin and integrity. The amended regime therefore does not replace the classical eIDAS trust-services model; rather, it embeds that model into a broader digital identity architecture.

The legal effects attached to QES and QESeal remain central. Under eIDAS, a QES has the equivalent legal effect of a handwritten signature, while a QESeal benefits from a presumption of integrity of the data and of correctness of origin. eIDAS also preserves the cross-border rule according to which qualified signatures and qualified seals based on qualified certificates issued in one Member State must be recognised as such in all other Member States. That combination of strong evidential effect and mandatory cross-border recognition is what continues to make QES and QESeal the highest-assurance trust-service tools under Union law.

The qualified status itself remains tied to the eIDAS supervision model. Trust service providers must obtain qualified status from the competent supervisory body and appear on the national trusted list; the qualified effects of the relevant service depend on that status. The amended framework also continues to require robust identity verification when qualified certificates are issued. The practical novelty introduced by the 2024 amendment lies not in changing the legal effect of QES or QESeal, but in mainstreaming their use through the EUDI Wallet. The Commission’s official guidance is explicit that the legal effect of electronic signatures is unchanged compared with the original eIDAS regime; what changes is the availability and usability of qualified signatures and seals through the wallet ecosystem. In other words, the EUDI Wallet is best understood as a delivery and usability layer for existing eIDAS trust services, not as a substitute legal regime for them.

As regards implementation, the amended framework is designed so that wallet users can receive and use qualified certificates within the wallet environment. The Commission has stated that certificates, including those for electronic signatures and seals, can be requested and stored within the EUDI Wallet. More specifically, Implementing Regulation (EU) 2024/2979 requires wallet providers to ensure that users are able to receive qualified certificates for QES or QESeal linked to qualified signature or seal creation devices that may be local, external or remote in relation to the wallet. That same implementing framework also requires access to signature-creation applications and accommodates different device-management models, including remote qualified devices managed as a service by qualified trust service providers.

For natural persons, the amended eIDAS framework adds an important policy choice: qualified electronic signatures created through the EUDI Wallet must be available free of charge for non-professional purposes. That is one of the clearest signs that the Regulation aims to normalise everyday use of QES across the Union. By contrast, electronic seals remain principally business-facing tools linked to legal persons. The Commission has confirmed that legal entities will be able to access electronic sealing functionality through the wallet environment as well, but Member States and wallet providers are not required to provide such functionality free of charge for legal entities or for professional use cases.

The broader implementation package reinforces that architecture.

After Regulation (EU) 2024/1183 amended eIDAS, the implementation layer became much denser and more technical for QES/QESeal. The amendment inserted new rules on remote qualified electronic signature creation devices and remote qualified electronic seal creation devices (Articles 29a and 39a), and also required the Commission to adopt implementing acts on reference standards for qualified certificates for signatures and seals. In other words, the amended framework moves from a relatively limited implementation layer to a much more complete technical rulebook for remote qualified signing and sealing.

In late 2024 the Commission adopted the first set of wallet implementing regulations covering person identification data and electronic attestations of attributes, wallet core functionalities, wallet certification, wallet protocols and interfaces, and notifications concerning the wallet ecosystem. In 2025 and early 2026, further implementing acts followed on wallet-relying party registration, identity verification for qualified certificates, remote qualified signature and seal device management, qualified validation services, and recognised signature and seal formats. Taken together, those measures show that the EUDI Wallet is being built as an interoperable technical and supervisory environment in which QES and QESeal remain eIDAS trust services, but become far easier to deploy and use at scale.

Implementation has accelerated through post-2024 implementing regulations that operationalize wallet integrity/core functionality (including QES/QSeal enablement and signature creation applications), wallet certification, qualified certificate reference standards, validation reference standards, and the management of remote qualified devices (rQSCD/rQSCD for seals) as qualified trust services—anchoring conformance assessment in ETSI standards.

The eIDAS implementation layer for QES/QSeal (original and amended) is best understood as a chain that operationalizes (a) trust anchors, (b) formats, (c) wallet enablement, (d) certificate issuance profiles, (e) validation, and (f) remote qualified device management.

Trusted Lists (TL) are the cross-border trust anchor mechanism used for qualified trust services: Commission Implementing Decision (EU) 2015/1505 sets technical specifications and formats for Trusted Lists under Article 22(5) eIDAS, supporting consistent publication/consumption of QTSP qualified status across the EU. In practice, this is what makes the qualified status of a trust service provider and its services visible and machine-readable across borders. For QES and QESeal, that matters because validation engines, relying parties and supervisory ecosystems use the trusted lists to determine whether a certificate/service was qualified at the relevant time. So even though 2015/1505 is not “about signing formats” as such, it is one of the central implementation instruments for the legal operability of QES/QESeal.

The original framework included Commission Implementing Decision (EU) 2016/650 on the security assessment standards for qualified signature and seal creation devices. This is the act that connects the legal concept of the QSCD/QSealCD in eIDAS with concrete assessment standards. It relied on standards such as EN 419 211 for products in a user-managed environment, and it expressly acknowledged that the market for remote qualified devices still needed further standardisation at the time. That is why 2016/650 is important historically: it is the bridge from Annex II of eIDAS to conformity assessment, but it was still largely oriented to the pre-remote-QSCD era.

For public-sector interoperability on accepted signature/seal formats, the older Commission Implementing Decision (EU) 2015/1506 has been repealed and replaced by Commission Implementing Regulation (EU) 2026/248, which lays down rules for formats of advanced signatures and advanced seals to be recognized by public sector bodies and explicitly aims to improve cross-border validation/interoperability (noting continuing technical evolution). This matters for B2G even when We Build uses QES/QSeal, because public-sector format recognition rules typically cover qualified signatures/seals “in specific formats” as part of the recognition obligation.

For EUDI Wallet enablement of QES/QSeal, Commission Implementing Regulation (EU) 2024/2979 sets integrity/core functionality obligations, including ensuring wallets can receive qualified certificates linked to local/external/remote qualified devices, securely interface to those devices, and provide signature creation applications; it also defines required functions for signature creation applications (sign/seal user-provided and relying-party-provided data; create signatures/seals in mandatory formats; inform users of results) and anticipates an API where integrated apps rely on remote qualified devices.

For wallet trustworthiness, Commission Implementing Regulation (EU) 2024/2981 establishes rules for the certification of European Digital Identity Wallets under Article 5c eIDAS, anchoring the certification framework in functional, cybersecurity, and data protection requirements to ensure a high level of security and trust in wallets.

For QTSP certificate issuance conformity, Commission Implementing Regulation (EU) 2025/1943 establishes reference standards for qualified certificates for electronic signatures and qualified certificates for electronic seals, supporting the “presumption of compliance” mechanism (i.e., conformance with referenced ETSI EN standards leads supervisory bodies to presume compliance with relevant eIDAS certificate requirements). This is central because QES and QESeal depend not only on a qualified device/service but also on the certificate layer. The Regulation ties compliance to updated ETSI certificate-policy and certificate-profile standards, including ETSI EN 319 411-2, ETSI EN 319 412-1, EN 319 412-2, EN 319 412-3 and EN 319 412-5, with adaptations for the EU regime.

For relying-party validation conformity, Commission Implementing Regulation (EU) 2025/1945 sets reference standards/specifications for validation of qualified electronic signatures and seals (and validation of advanced signatures/seals based on qualified certs), explicitly tying presumption of compliance to the use of ETSI standards and adapting references (including to ETSI TS 119 101 on signature creation/validation applications and ETSI TS 119 612 on Trusted Lists).

For remote qualified device management as a qualified trust service, Commission Implementing Regulation (EU) 2025/1567 establishes reference standards/specifications for management of remote qualified electronic signature creation devices and remote qualified electronic seal creation devices as qualified trust services, explicitly referencing ETSI TS 119 431-1 and setting an applicability date (notably, it applies from 19 August 2027).

In summary, the eIDAS implementation regime for QES/QESeal now has a clearer structure: trusted lists (2015/1505) tell you who is qualified; device assessment (2016/650, now complemented by 2025/1567 for remote devices) tells you how qualified signing/sealing environments are assessed; qualified certificate standards (2025/1943) govern the certificate layer; validation (2025/1945) and preservation (2025/1946) govern post-signature legal/technical reliability; and public-sector format recognition is now handled by 2026/248, replacing 2015/1506. That is the implementation-level map underneath QES and QESeal under eIDAS as it stands today.

## 7.2 The European Business Wallet proposal

The European Business Wallets (“EUBW”) draft regulation is, at this stage, a Commission proposal rather than adopted law. The proposal, COM(2025) 838 final of 19 November 2025, is presented as a measure complementing the European Digital Identity Wallet ecosystem by introducing a market-oriented digital tool tailored to business transactions. Its stated purpose is to provide a harmonised and trusted framework through which economic operators and public sector bodies can identify themselves, authenticate, and exchange data with full legal effect across borders.

From a QES/QESeal perspective, the proposal does not appear to create a parallel signature or seal regime outside eIDAS. Rather, it builds on the trust-services framework already established under eIDAS, as amended, and seeks to adapt that framework to business workflows. This is important analytically: the legal effect of qualified signatures and seals would still derive from eIDAS, while the EUBW would function as a business-facing orchestration layer for their use in regulatory, transactional and cross-border operational contexts.

The proposal is especially significant because it expressly links the business-wallet environment to qualified signatures and seals. According to the official proposal material, providers of European Business Wallets must ensure that wallet users are able to receive qualified certificates for qualified electronic signatures or seals linked to qualified signature or seal creation devices that may be local, external or remote in relation to the wallet unit. That is conceptually very close to the EUDI Wallet model and confirms that the EUBW is intended to operationalise, rather than replace, the existing qualified trust-services architecture.

The business use case is also more explicit than in the consumer-focused EUDI context. The proposal material states that Business Wallets should be able to prove legal identity and access rights, enable conformity declarations to be signed and sealed, and support secure and verifiable cross-border exchange of product data. In practical terms, that points to a regime in which QES and QESeal are not merely optional add-ons, but core compliance tools for representation, attestations, filings and regulated business communications.

Another important feature is interoperability with the EUDI ecosystem. The Commission has indicated that the EUBW should be integrated with existing frameworks, including the EU Digital Identity Wallet, and that such integration should allow relevant persons to authenticate via the EUDI Wallet and access trust services offered for the EUBW without creating a separate business identity. On that reading, the EUBW proposal points toward a layered model: EUDI supplies the foundational identity and trust-services backbone, while the EUBW adds business representation, permissions and sector-specific operational use cases on top.

Accordingly, the best reading of the EUBW draft regulation is that it extends the reach of QES and QESeal into a structured business-wallet environment without altering their underlying legal nature. QES and QESeal would continue to derive their legal validity, evidential force and cross-border recognition from eIDAS; the EUBW would provide a business-specific framework for implementing those tools in day-to-day compliance, product, representation and transaction flows. Because the instrument is still only a proposal, that assessment remains provisional and subject to change during the legislative process.

## 7.3 Technical standards and protocols for compliant interoperability

For remote qualified signing/sealing at scale (a central need for B2B/B2G), ETSI TS 119 431-1 is the key technical baseline for policy and security requirements for TSPs operating a remote signature creation device; it is explicitly aligned with CEN EN 419 241-1 and is directly referenced by the EU implementing regulation for remote qualified device management.

ETSI TS 119 432 specifies protocols/interfaces for remote digital signature creation and is commonly treated as the “protocol-level” companion to the policy/security focus of ETSI TS 119 431; together these help implement the eIDAS Annex II “sole control/control” outcomes in remote architectures.

ETSI TS 119 101 specifies policy and security requirements for applications for signature creation and signature validation (“signature creation applications” in wallet terms). This becomes operationally important because both EUDI Wallet and proposed EUBW frameworks explicitly foresee signature creation applications being provided by wallet providers, trust service providers, or relying parties creating a compliance need for consistent app-level controls irrespective of deployment model.

CEN EN 419 241-1 is explicitly positioned by ETSI as the general requirements baseline for trustworthy systems supporting server signing, and ETSI materials also point to EN 419 241-2 as a protection profile for QSCD for server signing together forming an evaluation backbone for remote QSCD assurance aligned with eIDAS.

For API-level market interoperability, the Cloud Signature Consortium reports that CSC API v2.2 was released on 6 November 2025 and is intended for interoperable cloud signature/seal services, designed to fit modern authorization frameworks (e.g., OAuth 2.0). However, direct access to the v2.2 PDF was not reliably retrievable via automated fetch in this research session; therefore, We Build should treat CSC v2.2 as the target interface while ensuring that the wallet-side signature creation application requirements and the QTSP remote QSCD requirements are demonstrably met through the ETSI/EN standards that are explicitly referenced by EU implementing acts.

## 7.4 Practical implications for We Build business transactions

For We Build B2B/B2G transaction flows, the primary compliance posture should start from the legal “end state” required: when a transaction calls for a handwritten-signature-equivalent outcome, We Build should specify QES (natural person) and/or QSeal (legal person) and then build the architecture so that (i) qualified certificates follow recognized profiles, (ii) devices meet QSCD/rQSCD requirements, and (iii) relying parties can validate/retain legal effect cross-border. This maps directly to eIDAS legal effects for QES/QSeal and to the implementing regulations establishing reference standards for certificates and validation.

A key We Build risk point is representation and capacity: QES binds signature to a natural person, while many business transactions require proving that person’s authority to act for an economic operator. The proposed EUBW introduces “mandate” definitions and calls for interoperability mechanisms for mandates/delegations across wallets (including formats for representing roles/attributes and auditability of authorization events), signaling that business-grade wallet signing will be evaluated on authorization evidence, not only identity assurance. We Build should therefore plan to bind mandate evidence to the transaction record (and, where appropriate, embed or reference it in signed statements/structured data), while monitoring forthcoming EUBW implementing acts.

Operationally, We Build implementations should align transaction transparency and auditability with wallet logging requirements. EUDI Wallet rules emphasize transaction logging for transparency and user oversight, while the EUBW proposal’s Annex is more explicit: it requires logging of signing/sealing transactions and non-completed transactions with reasons, and sets integrity/authenticity/confidentiality expectations for logged information. This makes logging design, correlation identifiers across relying party, wallet, signing service, and QTSP - an implementation-critical compliance deliverable, not an optional monitoring feature.

A timing-driven gap for We Build is that some remote QSCD/QSeal device management implementing rules apply later: the EU implementing regulation on remote qualified device management applies from 19 August 2027, meaning We Build should (a) implement to the referenced ETSI standards now, (b) choose QTSP partners that can evidence readiness for qualified status under the new “remote qualified device management” trust service category, and (c) avoid vendor lock-in by keeping the signature creation application interface cleanly separated (a design that aligns with wallet implementing regulations allowing signature creation applications to be provided by multiple actor types).

Certificate lifecycle is a strategic design choice for business onboarding. eIDAS allows qualified certificates with defined validity periods (Annex I/III content requirements include validity period fields) and eIDAS 2.0 expands acceptable identity verification methods (including via EUDI Wallet at assurance level high) for issuing qualified certificates, which can support both “long-term credential” models and “just-in-time/short-lived” issuance patterns. The main We Build risk point is not legality of shorter lifetimes per se, but long-term verifiability (validation material, timestamps, preservation) and consistent reliance on trusted lists/validation standards as mandated by implementing regulations.

## 7.5 Standardization references

The base standards are listed in the table below.

|     |     |
| --- | --- |
| **Index** | **Standard** |
| STD-01 | CEN EN 419 241-1, Trustworthy Systems Supporting Server Signing - Part 1: General System Security Requirements |
| STD-02 | CEN EN 419 241-2, Trustworthy Systems Supporting Server Signing Part 2: Protection Profile for QSCD for Server Signing |
| STD-03 | CEN EN 419 221-5, Protection profiles for TSP Cryptographic modules - Part 5: Cryptographic Module for Trust Services |
| STD-04 | Cloud Signature Consortium (CSC) API v2 |
| STD-05 | ETSI TS 119 431-1: Policy and security requirements for trust service providers; Part 1: TSP services operating a remote QSCD / SCDev |
| STD-06 | ETSI TS 119 431-2: Policy and security requirements for trust service providers; Part 2: TSP service components supporting AdES digital signature creation |
| STD-07 | ETSI TS 119 432: Electronic Signatures and Infrastructures (ESI); Protocols for remote digital signature creation |

|     |     |
| --- | --- |
| STD-08 | CEN EN 419 211 parts 1-4: Protection profiles for secure signature creation device |

|     |     |
| --- | --- |
| STD-09 | ETSI EN 319 102 parts 1-2: Electronic Signatures and Infrastructures (ESI); Procedures for Creation and Validation of AdES Digital Signatures |
| STD-10 | ETSI EN 319 122 parts 1-2: Electronic Signatures and Infrastructures (ESI); CAdES digital signatures |
| STD-11 | ETSI EN 319 132-1 parts 1-3: Electronic Signatures and Trust Infrastructures (ESI); XAdES digital signatures |
| STD-12 | ETSI EN 319 142 parts 1-3: Electronic Signatures and Infrastructures (ESI); PAdES digital signatures |
| STD-13 | ETSI TS 119 182 parts 1-2: Electronic Signatures and Infrastructures (ESI); JAdES digital signatures |

## 7.6 Requirements within the ARF

**Requirements for Wallet providers**

|     |     |     |
| --- | --- | --- |
| **Index** | **Requirement specification** | **Mandatory or Not/ Official legal source** |
| QES_01 | Wallet Providers SHALL ensure that each User has the possibility to receive a qualified certificate for Qualified Electronic Signatures, bound to a QSCD, that is either local, external, or remotely managed in relation to the Wallet Instance. | Mandatory (EU law): YES - Commission Implementing Regulation (EU) 2024/2979, Art. 11(1)-(2) (qualified certificate bound to QSCD: local / external / remote). |
| QES_02 | Wallet Providers SHALL ensure that each User who is a natural person has, at least for non-professional purposes, free-of-charge access to a Signature Creation Application which allows the creation of free-of-charge Qualified Electronic Signatures using the certificates referred to in QES_01. Wallet Providers SHALL ensure that: - The Signature Creation Application SHALL, as a minimum, be capable of signing or sealing User-provided data and Relying Party-provided data. - The Signature Creation Application SHALL be implemented as part of a Wallet Solution or external to it (by providers of trust services or by Relying Parties). - The Signature Creation Application SHALL be able to generate signatures or seals in formats compliant with at least the mandatory formats referred to in QES_08. _Note: a) Signature Creation Application (SCA): see definition in the ETSI TS 119 432 standard. 2) If the SCA is external to the Wallet Solution, it may be for example a separate mobile application, or be hosted remotely, for instance by the QTSP or by a Relying Party._ | Mandatory (EU law): YES - Reg. (EU) 2024/2979, Art. 11(3) + Art. 12 (free access to SCA; minimum SCA capabilities; signature formats). |
| QES_03 | For the use of the qualified certificate referred to in QES_01, Wallet Providers SHALL ensure that a Wallet Unit implements secure authentication of the User, as well as signature or seal invocation capabilities, as a part of a local, external or remote QSCD. | Mandatory (EU law): MOSTLY - derived from Reg. (EU) 2024/2979, Art. 11–12 (secure user authentication + signature invocation to use the QES certificate/QSCD). |
| QES_04 | Wallet Providers SHALL enable their Wallet Units to interface with QSCDs using protocols and interfaces necessary for the implementation of secure User authentication and signature or seal functionality. _Note: In a Relying Party-centric flow, the remote QTSP will likely be selected by the Relying Party, which implies the QSCD is managed by the remote QTSP. In a Wallet Unit-driven flow, the User should be able to choose the QSCD._ | Mandatory (EU law): PARTLY - Reg. (EU) 2024/2979, Art. 11(2) & Art. 12(3) (wallet solutions interact securely with local/external/remote QSCD; API required for remote QSCD when SCA is in-wallet). |
| QES_05 | Wallet Providers SHALL enable their Wallet Units to be used for User enrolment to a remote QES Provider (i.e., a QTSP offering remote QES), except where the Wallet Unit interfaces with local or external QSCDs. | Mandatory (EU law): IMPLIED ONLY - Art. 11 requires ability to receive/ use a qualified certificate bound to a remote QSCD; “enrolment” mechanics are not explicitly prescribed in law. |
| QES_06 | Wallet Providers SHALL ensure that their Wallet Solution supports at least one of the following options for remote QES signature creation: - remote QES creation through secure authentication to a QTSP signature web portal, - remote QES creation channelled by the Wallet Unit, - remote QES creation channelled by a Relying Party. | Mandatory (EU law): NO (ARF design choice) - the regulation allows SCA to be in the wallet, QTSP, or relying party; it does not mandate specific user flows. |
| QES_07 | Wallet Providers SHALL ensure that, where a Signature Creation Application relies on a remote Qualified Signature Creation Device and where it is integrated into a Wallet Instance, it supports the Cloud Signature Consortium API Specification 2.0 \[CSC API\]. | Mandatory (EU law): YES, CONDITIONALLY - Reg. (EU) 2024/2979, Art. 12(3) + Annex IV (if SCA is in-wallet and relies on remote QSCD, it must support the API specified in Annex IV (CSC API v2.0)). |
| QES_08 | Wallet Providers SHALL ensure that their Wallet Units are able to create signatures or seals in accordance with the mandatory PAdES format as specified in ETSI EN 319 142-1 V1.1.1 (2016-04). In addition, Wallet Providers SHOULD ensure that their Wallet Units are able to create signatures or seals in accordance with the following formats: - XAdES as specified in ETSI EN 319 132-1 V1.2.1 (2022-02), - JAdES as specified in ETSI TS 119 182-1 V1.2.1 (2024-07), - CAdES as specified in ETSI EN 3191 22-1 V1.3.1 (2023-06), and - ASiC as specified in ETSI EN 319 162-1 V1.1.1 (2016-04) and ETSI EN 319 162-2 V1.1.1 (2016-04). | Mandatory (EU law): YES - Reg. (EU) 2024/2979, Annex IV (mandatory PAdES format for signing/sealing data). |
| QES_09 | Empty | N/A (Empty requirement). |
| QES_10 | Wallet Providers SHALL ensure that, where the Signature Creation Application is implemented as part of the Wallet Unit and is used to generate signatures or seals of the representation of the document or data to be signed or sealed, the Wallet Unit presents the representation of the document or data to be signed or sealed to the User. | Mandatory (EU law): YES (principle) - eIDAS Reg. (EU) 910/2014, Annex II(2) (QSCD must not prevent data-to-be-signed being displayed to the signatory). |
| QES_11 | Wallet Providers SHALL ensure that, where the Signature Creation Application is implemented as part of the Wallet Unit, a Wallet Unit computes the hash or digest of the document or data to be signed through a Signature Create Application component. | Mandatory (EU law): NO (technical implementation detail) - hashing/digest is implied by signature schemes but not explicitly required by the regulation text. |
| QES_12 | Wallet Providers SHALL ensure that a Wallet Unit is able to create the signature value of the document or data to be signed either using a local or a remote signing application. _Note: a local signing application is on-device. It may either be embedded in the Wallet Unit or be an external application._ | Mandatory (EU law): PARTLY - Reg. (EU) 2024/2979, Art. 11 (local/external/remote QSCD) + Art. 12 (SCA can be in-wallet or external). “Local signing application” detail is not prescribed. |
| QES_13 | Wallet Providers SHALL ensure that a Wallet Unit provides a log of transactions related to qualified electronic signatures or seals generated by or through the Wallet Unit, allowing the User to view the history of previously signed data or documents, according to requirement DASH_04 in [Topic 19](https://eudi.dev/latest/annexes/annex-2/annex-2.02-high-level-requirements-by-topic/#a2312-topic-19---user-navigation-requirements-dashboard-logs-for-transparency). _Note: If the signature is generated by a remote Signature Creation Application, the Wallet is at minimum used to authenticate the User to the remote QTSP and to obtain the User's consent for the usage of the private signing key. The logs then record information about these processes._ | Mandatory (EU law): NO (ARF / product requirement) - transaction logs are not explicitly mandated in the cited eIDAS provisions for wallets/SCAs. |
| QES_14 | Wallet Providers SHALL ensure that the User will be able to explicitly authorise the creation of a qualified electronic signature or seal through their Wallet Unit. | Mandatory (EU law): YES (principle) - eIDAS Reg. (EU) 910/2014, Annex II(1)(d) & Annex II(2) (sole control / protection; data must be shown before signing), which implies explicit user authorisation/confirmation. |
| QES_15 | Wallet Providers SHALL ensure that a Wallet Unit can verify, in remote signature creation scenarios, that the qualified electronic signature or seal creation device is part of a qualified service, which is carried out by a qualified trust service provider. | Mandatory (EU law): PARTLY - Legal basis: (i) qualified status & trusted lists (eIDAS Art. 22 + Impl. Decision (EU) 2015/1505); (ii) remote QSCD management as a qualified trust service (Impl. Reg. (EU) 2025/1567 / eIDAS Art. 29b). Wallet-side verification method is not strictly prescribed. |
| QES_16 | Wallet Providers SHOULD ensure that a Wallet Unit supports multiple-signing scenarios where multiple signatories are required to sign the same document or data. | Mandatory (EU law): NO - stated as SHOULD (non-mandatory) and not required by the cited legal acts. |
| QES_17 | Wallet Providers SHALL ensure that Wallet Units provide a signature creation confirmation upon the creation of a qualified electronic signature, informing the User about the outcome of the signature creation process. _Note: See also QES_17a._ | Mandatory (EU law): PARTLY - Reg. (EU) 2024/2979, Art. 12(2)(d) requires informing the wallet user about the result; including file/digest specifics is ARF. |
| QES_17a | If the Signature Creation Application is external to the Wallet Unit, after the User authorises the usage of the private signing key, the Signature Creation Application SHALL return the outcome of the signature creation process to the Wallet Unit. | Mandatory (EU law): PARTLY - same as QES_17 (Art. 12(2)(d) outcome/result notification), but return mechanics are implementation-specific. |
| QES_18 | Wallet Providers SHALL configure at least one default qualified signing service in the Wallet Unit. | Mandatory (EU law): NO (ARF / UX choice) - “default QTSP” is not prescribed in the cited legal acts. |
| QES_19 | Wallet Providers SHALL ensure that, where the Signature Creation Application is implemented as part of the Wallet Unit, a Wallet Unit supports ETSI TS 119 101 (Electronic Signatures and Infrastructures (ESI) - Policy and security requirements for applications for signature creation and signature validation) when using signing keys managed by the QSCD, whether locally, externally, or remotely in relation to the Wallet Instance. | Mandatory (EU law): NO - ETSI TS 119 101 is a standard; not mandated as such by the cited EU legal acts. |

**Requirements for QTSPs**

|     |     |     |
| --- | --- | --- |
| **Index** | **Requirement specification** | **Mandatory or Not / Official legal source** |
| QES_23 | QTSPs providing the remote QES part of a Wallet Solution SHALL support: 1. ETSI TS 119 431-1 (Electronic Signatures and Infrastructures (ESI) Part 1: TSP service components operating a remote QSCD / SCDev), 2. ETSI TS 119 431-2 (Electronic Signatures and Infrastructures (ESI) - Policy and security requirements for trust service providers - Part 2: TSP service components supporting AdES digital signature creation), 3. ETSI TS 119 432 (Electronic Signatures and Infrastructures (ESI) - Protocols for remote digital signature creation). Wallet Providers and QTSPs providing the remote QES part of a Wallet Solution SHALL comply with Sole Control Assurance Level (SCAL) 2 as defined in CEN EN 419 241-1 (Trustworthy Systems Supporting Server Signing - Part 1: General System Security Requirements). | Mandatory (EU law): PARTLY/CONDITIONAL - For qualified ‘remote QSCD management’ service: Impl. Reg. (EU) 2025/1567 mandates reference standards (Annex includes ETSI TS 119 431-1 and EN 419 241-2). Other listed ETSI/SCAL levels may be best practice but not all are explicitly mandated by 2025/1567. |
| QES_24 | QTSPs providing the Signature Creation Application as part of the remote QES part of a Wallet Solution SHALL support ETSI TS 119 101 (Electronic Signatures and Infrastructures (ESI) - Policy and security requirements for applications for signature creation and signature validation). | Mandatory (EU law): NO - ETSI TS 119 101 is a standard; not mandated as such by the cited EU legal acts. |

**Requirements for Relying Parties**

|     |     |     |
| --- | --- | --- |
| **Index** | **Requirement specification** | **Mandatory or Not / Official legal source** |
| QES_24a | Relying Parties providing the Signature Creation Application in a Relying Party-centric flow SHALL support ETSI TS 119 101 (Electronic Signatures and Infrastructures (ESI) - Policy and security requirements for applications for signature creation and signature validation). | Mandatory (EU law): NO - ETSI TS 119 101 is a standard; not mandated as such by the cited EU legal acts. |

# 8\. Use cases in WE BUILD

## 8.1 Bank account opening use case in WP3 PA1

### _8.1.1 Signing a bank account contract with a QES_

Contract signing with Qualified Electronic Signatures (QES) using the EUDI Wallet is part of the WE BUILD Work Package 3 (WP3) Use Case PA1 (consumer bank account opening). The use case includes the technical, regulatory, and organizational processes to be piloted, the attestations involved, and the partners participating. The aim is to validate the end-to-end process of legally binding digital contract signing for bank account opening, leveraging the EUDI Wallet and QES, in compliance with eIDAS 2.0.

On a high level, the digital contract signing process using the QTSP-centric model for bank account opening involves the following steps:

1.  The customer fills in the bank account application at the bank's web portal.
2.  The bank sends the account opening contract to the customer's EUDI Wallet.
3.  The customer reviews the terms and conditions directly in the EUDI Wallet interface.
4.  The EUDI Wallet is used for identifying the customer to the QTSP. (The identification is performed at eID LoA High.)
5.  The EUDI Wallet submits the contract and a sign request to the QTSP.
6.  The QTSP signs the contract with a QES using its remote QSCD (QTSP-Centric model).
7.  The QTSP returns the signed contract to the EUDI Wallet.
8.  The EUDI Wallet returns the signed contract to the bank.

The QTSP-centric model for QES signing of a bank account contract is illustrated in the picture below.

Note: If the Wallet-Centric model would be used instead, the EUDI Wallet would create the QES with a local QSCD (either a secure element in the phone or a smart card via NFC access) in step 3 after the contract review. The QTSP steps 5-7 would then be used for augmenting the QES with time-stamps and revocation data into a AdES-LTV format.

### 8.1.2 Issuance of (Q)EAAs

In addition to digitally signing a contract, the QTSP could also issue a (Q)EAA in step 6, which would be returned to the customer in step 7. The (Q)EAA contains the customer’s name and the IBAN. In this scenario, the same QTSP is used for issuing the (Q)EAA as signing the contract. The QTSP would need to verify the customer’s IBAN, either by proof of access by the customer, or by verifying the IBAN with the bank as a trusted register (see ETSI TS 119 461 for further details on attribute proofing).

It is also possible to use different QTSPs for signing the contract and issue the (Q)EAA, but that would require two identifications with the EUDI Wallet.

A third option is that the bank itself issues an EAA with the IBAN, although this will not be a QEAA.

The (Q)EAA can be used by the customer to prove ownership of the bank account, which can be used for fraud prevention under PSD3 or for future bank account openings. The (Q)EAA issuance may be performed as part of step 4b above.

## 8.2 \[to be completed\]

## 8.3 \[to be completed\]
