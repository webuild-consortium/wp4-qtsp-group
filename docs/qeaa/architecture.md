# Architecture overview for QEAA in WE BUILD

## Context

### Purpose of this document

This document specifies the high-level architecture for the [issuance and validation of qualified electronic attestations of attributes (QEAA)](README.md) within WE BUILD.
It aims to guide the specification, development, testing and implementation of QEAA.
It complements the [WE BUILD architecture documentation](https://github.com/webuild-consortium/architecture) which specifies, among other topics, the issuance and validation of attestations within WE BUILD in general.

### Definitions

In WE BUILD, QTSPs as defined under [eIDAS] Art. 3(20) provide pre-production EAA issuance and validation services as defined under Art. 3(16)(g) and (h), technically ready to be audited for qualification as defined under Art. 3(17), for QEAA as defined under Art. 3(45).

[eIDAS]: https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A02014R0910-20241018

## Technical specifications for QEAA

### Functional decomposition

The following decomposition is inspired by [ETSI TS 119 471 v1.1.1](https://www.etsi.org/deliver/etsi_ts/119400_119499/119471/01.01.01_60/ts_119471v010101p.pdf).

- **QEAA service:** An electronic service which supports the following QEAA processes, governed under an *EAA scheme*. It is provided by a QTSP which is on the *trusted list* upon auditing for conformance to general requirements and to specific *EAA schemes* and upon qualification for QEAA issuance.
    - **QEAA issuance:** The *QTSP* issues a QEAA upon the request of a *subscriber* into their *wallet*.
    - **QEAA usage:** The *subscriber* uses the QEAA in accordance to the service terms and conditions.
    - **QEAA renewal:** The *QTSP* issues a new QEAA with the same attribute values as the previous QEAA.
    - **QEAA revocation:** The *QTSP* revokes a QEAA upon a trigger *revocation event*, such as an authorised request by a subject or subscriber.
    - **QEAA validation:** The *relying party* uses a *relying party instance* to verify and confirm that a QEAA is valid, typically under mutual authentication with its containing *wallet*.
- **Identity proofing service:** An electronic service by which the identity and additional attributes of an applying *subscriber* are verified. The verification process uses evidence attesting to the required identity attributes, including evidence from *PID/EAA presentation*, *attribute retrieval*, and *attribute verification*.
- **Attribute proofing service:** An electronic service by which the attributes of an *EAA subject* are verified. The verification process uses evidence attesting to the required attributes, including evidence from *attribute retrieval* and *attribute verification*. To discover authentic sources, the QTSP may consult the *catalogue of attributes*.

### Policy and security requirements

#### QEAA service

The requirements from [ETSI TS 119 471 v1.1.1](https://www.etsi.org/deliver/etsi_ts/119400_119499/119471/01.01.01_60/ts_119471v010101p.pdf) apply.

#### Identity proofing service

The requirements from [eIDAS] Art. 24(1a) and [ETSI TS 119 461 v2.1.1](https://www.etsi.org/deliver/etsi_ts/119400_119499/119461/02.01.01_60/ts_119461v020101p.pdf) apply.

#### Attribute proofing service

The requirements from [eIDAS] Art. 24(1b) and [ETSI TS 119 461 v2.1.1](https://www.etsi.org/deliver/etsi_ts/119400_119499/119461/02.01.01_60/ts_119461v020101p.pdf) apply.

### Deployment model and interfaces

The visualisation below represents a value chain, in which the primary data flows from left to right. The technical control flow may be reverse, for example using service request-response patterns.

```mermaid
---
title: Figure 1. Deployment model and interfaces
config:
  htmlLabels: false
  markdownAutoWrap: true
  flowchart:
    #defaultRenderer: "elk"
---
flowchart LR
subgraph Responsible body
    AS[Authentic
    source]@{shape: cyl}
end
subgraph EC[Commission]
    CatAtt[Catalogue of
    attributes]@{shape: docs}
    CatSch[Catalogue of
    EAA schemes]@{shape: docs}
end
subgraph QTSP[Qualified trust service provider]
    APS[Attribute
    proofing
    service]@{shape: rounded}
    IPS[Identity
    proofing
    service]@{shape: rounded}
    EAAS["QEAA service"]@{shape: rounded}
    Rev[Revocation
    event]@{shape: diamond}
end
subgraph Sub[Subscriber]
    Wallet
end
subgraph Relying party
    RP["Relying
    party
    instance"]
end
subgraph "TL scheme operator"
    TL["Trusted
    list (TL)"]@{shape: doc}
end
CatSch ---|1\. EAA scheme publication| EAAS
EAAS   ---|2\. QTSP registration     | TL
EAAS   ---|9\. QEAA issuance         | Wallet
IPS    ---|3\. Identity verification | Wallet
CatAtt ---|4\. Source discovery      | APS
AS     ---|5\. Attribute retrieval   | APS
AS     ---|6\. Attribute verification| APS
Rev    ---|11\. QEAA revocation      | EAAS
APS    ---|8\. Attribute proofing   | EAAS
IPS    ---|7\. Identity proofing     | EAAS
EAAS   ---|10\. QEAA validation       | RP
```

> [!NOTE]
> Since WE BUILD focuses on wallets, in this model interface 9 for identity verification is visualised with the subscriber’s wallet in this model. Outside of WE BUILD, under TS 119 461 it can be performed by various means.

> [!NOTE]
> This WE BUILD deployment model omits three other means from TS 119 461 by which the attribute proofing service could perform attribute verification: trusted registers other than authentic sources (Clauses 8.2.6 & 8.3.6), proof-of-access mechanisms (Clauses 8.2.7 and 8.3.7), and accepted documents and attestations (Clauses 8.2.8 and 8.3.8). The model assumes that each trusted register in WE BUILD is an authentic source. For proof-of-access mechanisms, the attribute proofing service is assumed to interact with the EAA subject. For attribute verification using electronic attestations of attributes, interface 3 for identity verification can be applied.

### Data flows and interactions

#### Protocol profiles

The interfaces listed below are those from the [Deployment model and interfaces](#deployment-model-and-interfaces).
Each protocol ideally is specified in a [WE BUILD Conformance Specification](https://github.com/webuild-consortium/wp4-architecture) (WBCS).
This way, interoperability can be tested on the [Interoperability Test Bed](https://github.com/webuild-consortium/wp4-interop-test-bed).
Some interfaces do not have common protocols since these are considered to be internal implementation details where WE BUILD does not require interoperability.

*Table 1. Protocol profiles and QTSP roles*

|Interface|Protocol|QTSP role|
|--|--|--|
|1\. EAA scheme publication|||
|2\. QTSP registration     |||
|3\. Identity verification  |[WBCS 2: Credential Presentation](https://github.com/webuild-consortium/wp4-architecture/blob/main/conformance-specs/cs-02-credential-presentation.md)|Verifier|
|4\. Source discovery      |||
|5\. Attribute retrieval   |||
|6\. Attribute verification|||
|7\. Identity proofing     |N/A|N/A|
|8\. Attribute proofing    |N/A|N/A|
|9\. QEAA issuance         |[WBCS 1: Credential Issuance](https://github.com/webuild-consortium/architecture/blob/main/conformance-specs/cs-01-credential-issuance.md)|Attestation Provider (Issuer)|
|10\. QEAA validation       |||
|11\. QEAA revocation      |N/A|N/A|

> [!NOTE]
> While the WE BUILD conformance specifications deliberately limit the degrees of freedom, outside of WE BUILD more options are possible under the applicable standards.

### Example use case scenarios

#### [QEAA issuance to EUDIW, Wallet-initiated](./issuance-to-eudiw.feature.md#scenario-wallet-initiated)

```mermaid
---
title: Figure 2. QEAA issuance to EUDIW, Wallet-initiated, main success scenario
---
sequenceDiagram
box Qualified trust service provider
    participant APS as Attribute<br>proofing<br>service
    participant EAAS as QEAA service
    participant IPS as Identity<br>proofing<br>service
end
participant Wallet
Wallet ->>+ EAAS : 9. QEAA issuance (authorization request)
EAAS ->>+ IPS : 7. Identity proofing (request)
loop For each required evidence
    IPS <<->> Wallet : 3. Identity verification
end
IPS -->>- EAAS : 7. Identity proofing (response)
EAAS ->>+ APS : 8. Attribute proofing (request)
note over APS : [ref] Source discovery and<br>attribute verification
APS -->>- EAAS : 8. Attribute proofing (response)
EAAS -->>- Wallet : 9. QEAA issuance (authorization grant)
activate Wallet
Wallet <<->> EAAS : 9. QEAA issuance (access)
Wallet <<->>- EAAS : 9. QEAA issuance (credential)
```

#### [QEAA issuance to EUDIW, Issuer-initiated](./issuance-to-eudiw.feature.md#scenario-issuer-initiated) with a pre-authorised code

```mermaid
---
title: Figure 3. QEAA issuance to EUDIW, Issuer-initiated, main success scenario
---
sequenceDiagram
box Qualified trust service provider
    participant APS as Attribute<br>proofing<br>service
    participant EAAS as QEAA service
    participant IPS as Identity and<br>attribute<br>proofing<br>service
end
participant Wallet
actor User
User ->>+ EAAS : 9. QEAA issuance (request)
EAAS ->>+ IPS : 7. Identity proofing (request)
loop For each required evidence
    IPS <<->> Wallet : 3. Identity verification
end
IPS -->>- EAAS : 7. Identity proofing (response)
EAAS ->>+ APS : 8. Attribute proofing (request)
note over APS : [ref] Source discovery and<br>attribute verification
APS -->>- EAAS : 8. Attribute proofing (response)
EAAS -)+ Wallet : 9. QEAA issuance (credential offer)
EAAS -->>- User : 9. QEAA issuance (transaction code)
User -) Wallet : Transaction code
Wallet <<->> EAAS : 9. QEAA issuance (access)
Wallet <<->>- EAAS : 9. QEAA issuance (credential)
```

#### Source discovery and attribute verification

In this example use case scenario, the attributes are verified at an authentic source. Other means for attribute proofing exist.

```mermaid
---
title: Figure 4. Source discovery and attribute verification
---
sequenceDiagram
participant AS as Authentic<br>source
participant CatAtt as Catalogue of<br>attributes
box Qualified trust service provider
    participant APS as Attribute<br>proofing<br>service
end
loop For each authentic source for additional attributes
    APS <<->> CatAtt : 4. Source discovery
    APS <<->> AS : 6. Attribute verification
end
```

## Deviations from European Digital Identity

In the WE BUILD pre-production environment, some European Digital Identity framework roles are simulated:

*Table 2. Role deviations within WE BUILD*

|Role|WE BUILD group|
|--|--|
|Commission||
|TL scheme operator|WP4 Trust Registry Infrastructure|
