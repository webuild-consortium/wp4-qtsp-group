# rb0xx_hello_world_attestation

# Rulebook RB0XX — Hello World Attestation

**Status:** Under development

**Version:** 0.1 (Draft) — short‑lived credential (max 24h)

**Editors:**

**Related Data Schema:** DS0XX — Hello World (see `docs/data-schemas/ds0xx_hello_world.json`)

---

## 1. Introduction and Scope

This Rulebook defines the **governance, functional, and technical requirements** for the **Hello World Attestation (HWA)**.

HWA is a minimal example of an Electronic Attestation of Attributes (EAA), issued without external source verification, intended solely for demonstration, testing, and onboarding of EUDI Wallet components.

Scope:
- This rulebook covers the attestation definition, issuance, presentation, and revocation model.

- It does not establish binding trust for production use cases.

- It is based on the EUDI Wallet ARF and the template provided in the [EUDI standards repository](https://github.com/eu-digital-identity-wallet/eudi-doc-standards-and-technical-specifications).

## 2. Definitions and Terminology

Terms such as **MUST**, **SHOULD**, and **MAY** are as in RFC 2119 / RFC 8174.

- **Hello World Attestation (HWA)**: a simple, universal value attestation containing a fixed attribute.

- **Issuer**: any entity authorized to issue the Hello World Attestation.

- **Wallet (Holder)**: an implementation of the EUDI Wallet that stores and presents the Hello World Attestation.

- **Relying Party (Verifier/RP)**: any service or verifier that requests and consumes the Hello World Attestation.

## 3. Attestation Type and Namespace

- Identifier: `eu.web.hwa.v1`
- Namespace: `eu.web.hwa`

## 4. Attribute Schema

The Hello World Attestation SHALL contain the following attribute(s):

| Attribute | Type | Required | Description |
| --- | --- | --- | --- |
| message | string | YES | Fixed value “Hello World!” |

## 5. Metadata and Common Attributes

Every Hello World Attestation SHALL include:
- `issuer` (string, DID or X.509 identifier)

- `issuedAt` (timestamp, ISO 8601)

- `expiry` (timestamp, MANDATORY, MUST be ≤24h after `issuedAt`)

- `credentialStatus` (object, MAY reference a status list endpoint)

## 6. Representation and Encoding

Three profiles are defined (equal status). Example files live under `docs/qeaa/examples/`:

1. **SD-JWT VC** — Issued as SD-JWT VC, proof via JWS, selective disclosure possible. See `docs/qeaa/examples/sd-jwt/`.

2. **mdoc** — `docType = eu.web.hwa.v1`, namespace `eu.web.hwa`, proof via COSE_Sign1. See `docs/qeaa/examples/mdoc/`.

3. **JSON-LD VC (W3C Data Integrity proof)** — See `docs/qeaa/examples/json-ld/`.

### 6.1 Normative requirements

- An implementation MUST support at least one of the three profiles.
- Wallets SHOULD support at least two profiles for interoperability.
- Relying parties MUST declare which profile(s) they support.
- The semantic content (`message = "Hello World!"`) MUST be preserved across profiles.

## 7. Proof and Cryptographic Binding

- Proof SHALL be established through a digital signature by the issuer.
- Acceptable proof types: `JWT proof`, `SD‑JWT proof`, or `COSE_Sign1` in mdoc.
- Binding to the wallet holder is OPTIONAL in this demo attestation.

## 8. Validity, Revocation and Status

- Default validity: **24 hours maximum** from issuance.
- Wallets MUST reject Hello World Attestations with validity periods exceeding 24h.
- Revocation MAY be supported (e.g., Status List 2021 for SD‑JWT).
- For mdoc, revocation mechanism is OPTIONAL.
- If no revocation mechanism is provided, attestations SHALL be considered valid until expiry.

## 9. Presentation and Selective Disclosure

- In SD‑JWT VC: selective disclosure is possible but trivial (only one attribute).
- In mdoc: ItemsRequest/DeviceResponse SHALL be supported.
- In JSON‑LD VC: standard VC Data Model v2 presentation SHALL be supported.
- Relying parties MUST NOT assume trustworthiness for production use.

## 10. Trust and Authorization

- Any issuer MAY issue the Hello World Attestation.
- No external source verification is required.
- Relying parties MUST treat the Hello World Attestation as **non‑trustworthy** for production purposes, and use it solely for demo/test purposes.

## 11. Interoperability and Conformance

- Issuer, Wallet, and Verifier MUST implement requirements in Sections 3–7.
- Wallets and relying parties SHALL follow the ARF and this rulebook for encoding, proof handling, and presentation.
- Conformance testing MAY be conducted using the examples under `docs/examples/`.

## 12. Normative References

- EUDI ARF (latest)
- ISO/IEC 18013‑5/7
- SD‑JWT VC Profile
- W3C VC Data Model v2
- Status List 2021
- RFC 2119 / RFC 8174

---
