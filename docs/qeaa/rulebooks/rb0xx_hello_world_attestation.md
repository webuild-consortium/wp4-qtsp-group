

# Attestation Rulebook for attestations of type `eu.web.hwa.v1` (Hello World Attestation)

**Author(s):**  
Jochem Oosterlee, Cleverbase

**Previous Authors:**  
–

**Versioning**

| Version | Date       | Description |
|---------|------------|-------------|
| 0.1     | 2025-09-29 | Initial draft — Hello World Attestation (≤24h), demo/test only |

**Feedback:**  
https://github.com/webuild-consortium/wp4-qtsp-group/issues

---

## 1 Introduction

### 1.1 Document scope and purpose
This Rulebook defines the **Hello World Attestation (HWA)**, a minimal, short-lived Electronic Attestation of Attributes (EAA).  
Its purpose is to provide a trivial, universal test credential that can be issued and presented in EUDI Wallet implementations for **onboarding, demonstration, and interoperability testing**.  
It has **no production-trust value**.

### 1.2 Document structure
- Chapter 2 describes the attributes and metadata in an encoding-independent way.  
- Chapter 3 specifies how these are encoded in ISO/IEC 18013-5 (mdoc), SD-JWT VC, and W3C VCDM v2.0 (JSON-LD VC).  
- Chapter 4 specifies usage scenarios.  
- Chapter 5 defines trust anchors.  
- Chapter 6 defines revocation (short-lived validity).  
- Chapter 7 provides compliance information.  

### 1.3 Key words
This document uses the capitalised key words **SHALL**, **SHOULD**, and **MAY** as specified in [RFC 2119]. *must* indicates an external requirement; *can* denotes a capability.

### 1.4 Terminology
This document uses the terminology specified in Annex 1 of the ARF.

---

## 2 Attestation attributes and metadata

### 2.1 Introduction
The Hello World Attestation is a **non-qualified EAA**.  
It defines a single fixed attribute and minimal metadata.  
The attribute `attestation_legal_category` SHALL be `"non-qualified-EAA"`.

### 2.2 Mandatory attributes

| Data Identifier | Definition         | Data type | Example value  |
|-----------------|-------------------|-----------|----------------|
| message         | Fixed test string | string    | "Hello World!" |
| attestation_legal_category | Legal category | string | "non-qualified-EAA" |

### 2.3 Optional attributes
_None._

### 2.4 Conditional attributes
_None._

### 2.5 Mandatory metadata

| Data Identifier | Definition   | Data type | Example value                  |
|-----------------|-------------|-----------|--------------------------------|
| issuer          | Issuer ID   | string    | `did:example:issuer123`        |
| issuedAt        | Issue time  | tdate     | `2025-09-29T12:00:00Z`         |
| expiry          | Expiry time | tdate     | `2025-09-29T23:59:59Z` (≤24h)  |

### 2.6 Optional metadata

| Data Identifier   | Definition          | Data type | Example value               |
|-------------------|--------------------|-----------|-----------------------------|
| credentialStatus  | Revocation reference | object  | URL to status list endpoint |

### 2.7 Conditional metadata
_None._

---

## 3 Attestation encoding

### 3.1 ISO/IEC 18013-5-compliant encoding
- **docType:** `eu.web.hwa.v1`  
- **Namespace:** `eu.web.hwa`  
- Attributes:  
  - `message`: `tstr` UTF-8  
  - `attestation_legal_category`: `tstr` UTF-8  
  - `issuedAt`, `expiry`: `tdate` (RFC 3339, UTC, no fractions)  
- Proof: COSE_Sign1 in `issuerAuth`.  
- Illustrative example:
  ```json
  {
    "docType": "eu.web.hwa.v1",
    "issuerSigned": {
      "nameSpaces": {
        "eu.web.hwa": [
          { "name": "message", "value": "Hello World!" },
          { "name": "attestation_legal_category", "value": "non-qualified-EAA" },
          { "name": "issuedAt", "value": "2025-09-29T12:00:00Z" },
          { "name": "expiry", "value": "2025-09-29T23:59:59Z" }
        ]
      },
      "issuerAuth": "<COSE_Sign1 placeholder for demo>"
    }
  }
  ```
- A supplementary device response example is provided in `docs/qeaa/examples/mdoc/device_response.json` (WEBUILD repo, https://github.com/webuild-consortium/wp4-qtsp-group).

### 3.2 SD-JWT VC-based encoding
- **Verifiable Credential Type (vct):** `eu.web.hwa.v1`  
- Claims:

| Data Identifier | Attribute identifier     | Encoding format | Notes | Disclosable |
|-----------------|--------------------------|----------------|-------|-------------|
| message         | message                  | string         | Fixed value | MUST |
| attestation_legal_category | attestation_legal_category | string | Non-qualified-EAA | MUST NOT |
| issuer          | iss                      | string         | JWT claim | MUST NOT |
| issuedAt        | iat                      | number (epoch) | JWT claim | MUST NOT |
| expiry          | exp                      | number (epoch) | ≤24h after iat | MUST NOT |

- Note (ARB_31/ARB_32): For production-grade rulebooks, the template recommends referencing a Type Metadata Document (TMD) and a normative JSON Schema for the SD-JWT VC claims structure. For this Hello World demo attestation, these artifacts are OPTIONAL; if available, they SHOULD be cited here with stable URLs to enable automated validation and improved interoperability. The JSON Schema is available at `docs/qeaa/data-schemas/ds0xx_hello_world.json` (WEBUILD repo, https://github.com/webuild-consortium/wp4-qtsp-group).
- Illustrative example:
  ```json
  {
    "vct": "eu.web.hwa.v1",
    "iss": "https://issuer.example.org",
    "iat": 1730131200,
    "exp": 1730217599,
    "message": "Hello World!",
    "attestation_legal_category": "non-qualified-EAA"
  }
  ```
- Issued SD-JWT (base64 placeholder): `eyJ...`
- Disclosures: `["<base64-encoded disclosure for message: Hello World!>"]`
- A presentation example, including optional holder-binding JWT (`kb_jwt`), is provided in `docs/qeaa/examples/sd-jwt/presentation.json` (WEBUILD repo).

### 3.3 W3C VCDM v2.0-based encoding (JSON-LD VC)
- Context: `https://www.w3.org/ns/credentials/v2` + `hello-world-v1.context.jsonld`  
- Proof: W3C Data Integrity Proof (e.g., `ecdsa-rdfc-2019`)  
- Illustrative example:
  ```json
  {
    "@context": [
      "https://www.w3.org/ns/credentials/v2",
      "https://example.org/credentials/hwa#"
    ],
    "id": "urn:uuid:1234",
    "type": ["VerifiableCredential", "HelloWorldAttestation"],
    "issuer": "https://issuer.example.org",
    "issuanceDate": "2025-09-29T12:00:00Z",
    "expirationDate": "2025-09-29T23:59:59Z",
    "credentialSubject": {
      "hwa:message": "Hello World!",
      "hwa:attestation_legal_category": "non-qualified-EAA"
    },
    "proof": {
      "type": "DataIntegrityProof",
      "cryptosuite": "ecdsa-rdfc-2019",
      "created": "2025-09-29T12:00:00Z",
      "verificationMethod": "did:example:123#key-1",
      "proofPurpose": "assertionMethod",
      "proofValue": "zBase58Signature"
    }
  }
  ```
- Context definition: Available at `docs/qeaa/examples/json-ld/hello-world-v1.context.jsonld` (WEBUILD repo).

---

## 4 Attestation usage
- **Use case:** Demo / onboarding / testing flows.  
- **Wallets** SHALL reject attestations with expiry >24h.  
- **Relying Parties** SHALL validate signature, expiry, and (if present) status.  
- PID binding not required.  
- Attestation SHALL NOT be trusted for production.
- No transactional data defined as per Topic 20.

---

## 5 Trust anchors
- Any issuer MAY issue HWA.  
- Issuer keys MAY be discovered via DID or JWKS.  
- RPs SHALL use the `issuer` metadata to resolve signing keys via DID or JWKS discovery.  
- No QTSP trusted list binding.  
- Verifiers SHALL treat all issuers as **non-trustworthy** (demo only).

---

## 6 Revocation
- Attestations SHALL be short-lived (≤24h), aligning with Topic 7 which makes revocation OPTIONAL for such profiles.  
- Revocation is OPTIONAL.  
- If implemented, SD-JWT MAY use Status List 2021.  
- If not, attestation SHALL be valid until expiry.

---

## 7 Compliance
This Rulebook complies with the ARF (Topic 12):  
- Attributes defined encoding-independent (§2).  
- Encodings for ISO/IEC 18013-5, SD-JWT VC, and W3C VCDM v2.0 (§3).  
- Usage guidance (§4) and trust anchors (§5) aligned with ARB_21/ARB_26.  
- Revocation approach (§6) aligned with Topic 7 for short-lived EAAs.  
- No transactional data defined (§4, Topic 20).  
It follows [RFC 2119], [RFC 8174], ISO/IEC 18013-5, SD-JWT VC, and W3C VCDM v2.0.  

---

## 8 References

| Item Reference | Standard name/details |
|----------------|-----------------------|
| [European Digital Identity Regulation] | Regulation (EU) 2024/1183 |
| [ARF] | EUDI Architecture and Reference Framework |
| [ISO/IEC 18013-5] | ISO/IEC 18013-5:2021, mDL |
| [SD-JWT VC] | IETF draft-ietf-oauth-sd-jwt-vc-11 |
| [IANA JWT Claims] | IANA JSON Web Token Claims Registry |
| [W3C VCDM v2.0] | W3C Verifiable Credentials Data Model v2.0 |
| [RFC 2119] | Key words for use in RFCs |
| [RFC 8174] | Ambiguity of uppercase key words |
| [RFC 3339] | Date and Time on the Internet |
| [RFC 8949] | CBOR |
| [RFC 8610] | CDDL |
| [RFC 8943] | CBOR Tags for Date |
| [OIDC] | OpenID Connect Core 1.0 |
| [HAIP] | OpenID4VC High Assurance Interop Profile draft-03 |
| [Topic 7] | ARF Annex 2 – Revocation |
| [Topic 10] | ARF Annex 2 – Issuance |
| [Topic 12] | ARF Annex 2 – Attestation Rulebooks |
| [Topic 20] | ARF Annex 2 – Strong User Authentication |

