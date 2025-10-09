

# Attestation Rulebook for attestations of type Hello World

**Author(s):**  
Jochem Oosterlee, Cleverbase

**Previous Authors:**  
–

**Versioning**

| Version | Date       | Description |
|---------|------------|-------------|
| 0.1     | 2025-09-29 | Initial draft — Hello World Attestation (≤24h), demo/test only |
| 0.2     | 2025-10-09 | Switched to QEAA profile; updated metadata naming |

**Feedback:**  
https://github.com/webuild-consortium/wp4-qtsp-group/issues

---

## 1 Introduction

### 1.1 Document scope and purpose
This Rulebook defines the **Hello World Attestation (HWA)**, a minimal, short-lived Qualified Electronic Attestation of Attributes (QEAA) compliant with Regulation (EU) 2024/1183.  
Its purpose is to test pre-production QEAA issuance workflows with real Qualified Trust Service Providers (QTSPs) in production-like EUDI Wallet environments, supporting onboarding of trusted wallet systems, demonstration of compliant issuance capabilities, and interoperability across QEAA-compliant implementations.  
The HWA is intended for testing and has limited production trust value within the WEBUILD consortium’s controlled environment.

### 1.2 Document structure
- Chapter 2 describes the attributes and metadata in an encoding-independent way.
- Chapter 3 specifies how these are encoded in ISO/IEC 18013-5 (mdoc) and SD-JWT VC.
- Chapter 4 specifies usage scenarios.  
- Chapter 5 defines trust anchors.  
- Chapter 6 defines revocation (short-lived validity).  
- Chapter 7 provides compliance information.  

### 1.3 Key words
This document uses the capitalised key words **SHALL**, **SHOULD**, and **MAY** as specified in [RFC 2119]. *must* indicates an external requirement; *can* denotes a capability.

### 1.4 Terminology
This document uses the terminology specified in Annex 1 of the ARF, including Qualified Electronic Attestation of Attributes (QEAA), Qualified Trust Service Provider (QTSP), Trusted List, and Annex V metadata requirements.

---

## 2 Attestation attributes and metadata

### 2.1 Introduction
The Hello World Attestation is a **Qualified EAA (QEAA)** for testing purposes.  
It defines a single fixed attribute and mandatory Annex V metadata to comply with Regulation (EU) 2024/1183.  
The attribute `attestation_legal_category` SHALL be `"QEAA"`.

### 2.2 Mandatory attributes

| Data Identifier | Definition         | Data type | Example value  |
|-----------------|-------------------|-----------|----------------|
| message         | Fixed test string | string    | "Hello World!" |
| attestation_legal_category | Legal category | string | "QEAA" |

### 2.3 Optional attributes
_None._

### 2.4 Conditional attributes
_None._

### 2.5 Mandatory metadata

| Data Identifier      | Definition                     | Data type | Example value                  |
|----------------------|-------------------------------|-----------|--------------------------------|
| issuing_authority    | Issuer ID (URI resolvable via Trusted List) | string    | `https://issuer.webuildconsortium.eu` |
| issuer_legal_id      | Legal person identifier       | string    | `LEI:1234567890`              |
| trust_anchor_url     | Trusted List URL              | string    | `https://trustedlist.eu/issuer`|
| attestation_scheme   | Scheme details                | string    | `QEAA:HelloWorld`             |
| issuance_date        | Issue time                    | tdate     | `2025-10-07T12:00:00Z`        |
| expiry_date          | Expiry time                   | tdate     | `2025-10-07T23:59:59Z` (≤24h) |

### 2.6 Optional metadata

_None._

### 2.7 Conditional metadata
_None._

---

## 3 Attestation encoding

## 3.1 ISO/IEC 18013-5-compliant encoding
- **docType:** `eu.webuildconsortium.helloworld.v1`
- **Namespace:** `eu.webuildconsortium.helloworld`
- Attributes:
  - `message`: `tstr` UTF-8
  - `attestation_legal_category`: `tstr` UTF-8
  - `issuing_authority`: `tstr` UTF-8
  - `issuer_legal_id`: `tstr` UTF-8
  - `trust_anchor_url`: `tstr` UTF-8
  - `attestation_scheme`: `tstr` UTF-8
  - `issuance_date`: `tdate` (RFC 3339, UTC, no fractions)
  - `expiry_date`: `tdate` (RFC 3339, UTC, no fractions)
- Proof: COSE_Sign1 in `issuerAuth`, signed by a QTSP key registered in a Trusted List.
- Illustrative example:
  ```json
  {
    "docType": "eu.webuildconsortium.helloworld.v1",
    "issuerSigned": {
      "nameSpaces": {
        "eu.webuildconsortium.helloworld": [
          { "name": "message", "value": "Hello World!" },
          { "name": "attestation_legal_category", "value": "QEAA" },
          { "name": "issuing_authority", "value": "https://issuer.webuildconsortium.eu" },
          { "name": "issuer_legal_id", "value": "LEI:1234567890" },
          { "name": "trust_anchor_url", "value": "https://trustedlist.eu/issuer" },
          { "name": "attestation_scheme", "value": "QEAA:HelloWorld" },
          { "name": "issuance_date", "value": "2025-10-07T12:00:00Z" },
          { "name": "expiry_date", "value": "2025-10-07T23:59:59Z" }
        ]
      },
      "issuerAuth": "<COSE_Sign1 placeholder for demo>"
    }
  }
  ```

## 3.2 SD-JWT VC-based encoding
- **Verifiable Credential Type (vct):** `eu.webuildconsortium.helloworld.v1`
- Claims:

| Data Identifier      | Attribute identifier     | Encoding format | Notes | Disclosable |
|----------------------|--------------------------|----------------|-------|-------------|
| message              | message                  | string         | Fixed value | MUST |
| attestation_legal_category | attestation_legal_category | string | QEAA | MUST NOT |
| issuing_authority    | iss                      | string         | JWT claim | MUST NOT |
| issuer_legal_id      | issuer_legal_id          | string         | Annex V | MUST NOT |
| trust_anchor_url     | trust_anchor_url         | string         | Annex V | MUST NOT |
| attestation_scheme   | attestation_scheme       | string         | Annex V | MUST NOT |
| issuance_date        | iat                      | number (epoch) | JWT claim | MUST NOT |
| expiry_date          | exp                      | number (epoch) | ≤24h after iat | MUST NOT |

- Note (ARB_31/ARB_32): A Type Metadata Document (TMD) and normative JSON Schema for the SD-JWT VC claims structure SHALL be referenced for QEAA compliance. The JSON Schema is available at `docs/qeaa/data-schemas/ds0xx_hello_world.json` (WEBUILD repo, https://github.com/webuild-consortium/wp4-qtsp-group).
- Illustrative example:
  ```json
  {
    "vct": "eu.webuildconsortium.helloworld.v1",
    "iss": "https://issuer.webuildconsortium.eu",
    "iat": 1760025600,
    "exp": 1760111999,
    "message": "Hello World!",
    "attestation_legal_category": "QEAA",
    "issuer_legal_id": "LEI:1234567890",
    "trust_anchor_url": "https://trustedlist.eu/issuer",
    "attestation_scheme": "QEAA:HelloWorld"
  }
  ```
- Issued SD-JWT (base64 placeholder): `eyJ...`
- Disclosures: `["<base64-encoded disclosure for message: Hello World!>"]`

---

## 4 Attestation usage
- **Use case:** Testing (pre-production) QEAA issuance, presentation, and verification in production-like EUDI Wallet environments with trusted wallets and relying parties.  
- **Wallets** SHALL reject attestations with expiry >24h.  
- **Relying Parties** SHALL validate Trusted List-bound signatures, Annex V metadata, and (if present) status.  
- PID binding not required.  
- The HWA is intended for testing within the WEBUILD consortium and has limited production trust value.  
- No transactional data defined as per Topic 20.

---

## 5 Trust anchors
- The HWA SHALL be issued by QTSPs with keys registered in a Member State’s Trusted List, per Regulation (EU) 2024/1183 (Annex V) and ARB_20.  
- Issuer keys SHALL be discoverable via trust anchor URLs provided in the `trustAnchorUrl` metadata.  
- RPs SHALL resolve signing keys via Trusted List URLs and verify signatures against trust anchors.  

---

## 6 Revocation
- Attestations SHALL be short-lived (≤24h), aligning with Topic 7, which makes revocation OPTIONAL for such profiles.  
- Revocation SHALL NOT be implemented. Attestations SHALL be valid until expiry. 
- If implemented, SD-JWT MAY use Status List 2021.  
- If not, attestation SHALL be valid until expiry.

---

## 7 Compliance
This Rulebook complies with Regulation (EU) 2024/1183 (Annex V) and the ARF for QEAAs:  
- Attributes defined encoding-independent (§2).  
- Encodings for ISO/IEC 18013-5 and SD-JWT VC (§3), compliant with ARB_01a/ARB_02/ARB_03.  
- Trusted List-bound issuance (§5) aligned with ARB_20.  
- Usage guidance (§4) and trust anchors (§5) aligned with ARB_21/ARB_26.  
- Short-lived validity (§6) aligned with Topic 7.  
- No transactional data defined (§4, Topic 20).  
It follows [RFC 2119], [RFC 8174], ISO/IEC 18013-5, SD-JWT VC, and Regulation (EU) 2024/1183.

---

## 8 References

| Item Reference | Standard name/details |
|----------------|-----------------------|
| [European Digital Identity Regulation] | Regulation (EU) 2024/1183 |
| [ARF] | EUDI Architecture and Reference Framework |
| [ISO/IEC 18013-5] | ISO/IEC 18013-5:2021, mDL |
| [SD-JWT VC] | IETF draft-ietf-oauth-sd-jwt-vc-11 |
| [IANA JWT Claims] | IANA JSON Web Token Claims Registry |
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

