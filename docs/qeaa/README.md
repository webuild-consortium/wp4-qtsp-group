# QEAA documentation

This is part of the [QTSP documentation](../README.md).

## Reference model

![QEAA value chain](qeaa-value-chain.svg)

## Feature definitions

Below is a non-exhaustive overview of QEAA features that use cases may choose to pilot.
For each feature in scope for the pilots, the QTSP group develops an interop profile and ensures available service compatibility.

- [QEAA issuance to EUDIW](issuance-to-eudiw.feature.md)
- [EAA validation at RP](validation.feature.md)
- [Verification of attributes](verification.feature.md)

## Schemes for QEAA

Participants of the QTSP group may issue QEAA under any of the schemes referenced below.

# Hello World Attestation (HWA) Rulebook Description

**Hello World Rulebook**: The [Hello World Attestation (HWA) Rulebook](rb0xx_hello_world_attestation.md) defines a minimal, short-lived Qualified Electronic Attestation of Attributes (QEAA) compliant with Regulation (EU) 2024/1183 and the EUDI Architecture and Reference Framework (ARF). It is designed for testing pre-production QEAA issuance, presentation, and verification workflows with real Qualified Trust Service Providers (QTSPs) in production-like EUDI Wallet environments, supporting onboarding of trusted wallet systems, demonstration of compliant issuance capabilities, and interoperability across QEAA-compliant implementations. The HWA has limited production trust value within the WEBUILD consortium’s testing context and requires QTSP issuance with Trusted List-bound signatures for source verification. It includes a single fixed attribute (`message: "Hello World!"`), the mandatory `attestation_legal_category: "QEAA"`, and Annex V metadata (e.g., issuer legal identifiers, trust anchor URLs). It supports two encodings: ISO/IEC 18013-5 (mdoc) and SD-JWT VC. Revocation is not implemented; attestations are valid until expiry (≤24h).

- **Examples**: Illustrative examples are embedded in the Rulebook (Sections 3.1–3.2). Testable versions are available in the repository:
  - mdoc: [`docs/qeaa/examples/mdoc/issuance.json`](docs/qeaa/examples/mdoc/issuance.json), [`docs/qeaa/examples/mdoc/device_response.json`](docs/qeaa/examples/mdoc/device_response.json)
  - SD-JWT VC: [`docs/qeaa/examples/sd-jwt/issuance.json`](docs/qeaa/examples/sd-jwt/issuance.json), [`docs/qeaa/examples/sd-jwt/presentation.json`](docs/qeaa/examples/sd-jwt/presentation.json)
  - JSON Schema: [`docs/qeaa/data-schemas/ds0xx_hello_world.json`](docs/qeaa/data-schemas/ds0xx_hello_world.json)
- **Feedback**: Submit issues or PRs at [https://github.com/webuild-consortium/wp4-qtsp-group/issues](https://github.com/webuild-consortium/wp4-qtsp-group/issues).

## Informative references

- Catalogues
  - [Attestation Rulebooks Catalog](https://github.com/eu-digital-identity-wallet/eudi-doc-attestation-rulebooks-catalog): Catalogue of schemes for EAA in EUDI
- Standards
  - [TS 119 471 v1.1.1](https://www.etsi.org/deliver/etsi_ts/119400_119499/119471/01.01.01_60/ts_119471v010101p.pdf): requirements for EAA Providers
- Technical reports
  - [TR 119 476 v1.2.1](https://www.etsi.org/deliver/etsi_tr/119400_119499/119476/01.02.01_60/tr_119476v010201p.pdf): SD and ZKP for EAA analysis
  - [TR 119 476-1 v1.3.1](https://www.etsi.org/deliver/etsi_tr/119400_119499/11947601/01.03.01_60/tr_11947601v010301p.pdf): SD and ZKP for EAA feasibility
  - [TR 119 479-2 v1.1.1](https://www.etsi.org/deliver/etsi_tr/119400_119499/11947902/01.01.01_60/tr_11947902v010101p.pdf): EAA extended validation