# Scenarios

Implementation status of the qualified trust service part of each WE BUILD
use case scenario. See the [legend and update instructions](README.md).

## Overview

| ID | Scenario | Q services | Q service providers | Notes | Updated | Status |
|---|---|---|---|---|---|---|
| BU1-1 | KYC (B2B vertical) | QEAA | Procivis, Spherity (technology for QTSP) | No qualified QEAA provider named. | 2026-10-05 | ❔ |
| BU1-2 | KYS (B2B vertical) | QEAA | Procivis, Spherity (technology for QTSP) | No qualified QEAA provider named. | 2026-10-05 | ❔ |
| BU1-3 | KYC for sole traders | QEAA | Docaposte, Cleverbase, Procivis (tbc), Signicat (tbc) | Draft specification (v0.7). | 2026-10-05 | ❔ |
| BU1-4 | KYS for sole traders | QEAA | Docaposte, Cleverbase, Procivis (tbc), Signicat (tbc) | Draft specification (v0.7). | 2026-10-05 | ❔ |
| BU1-5 | Know Your Employee (KYE) | QEAA | SwissSign | | 2026-10-05 | ❔ |
| BU2-1 | Create a company branch in an EU or EEA Member State | QEAA, QES, QESeal, QERDS (MVP+) | D-Trust (QEAA, QES) | QESeal required, provider not explicitly assigned. | 2026-10-05 | ❔ |
| BU2-2 | Register a company branch with a tax authority | QEAA, QES | None named | All provider roles empty; assumed to be delivered by WP4. | 2026-10-05 | 🔴 |
| BU3-1 | Filing VAT declaration using tax portal | QEAA, QES, QESeal | QEAA: Reconi, Signicat, Digidentity, Cleverbase. QES: Cleverbase, KPN, Digidentity, Signicat. | QESeal provider not named. | 2026-10-05 | ❔ |
| BU3-2 | Filing VAT declaration using M2M | QEAA, QES, QESeal, QERDS | QEAA: Reconi, Signicat, Digidentity. QES: Cleverbase, KPN, Digidentity, Signicat. QERDS: Cleverbase, Ledger Leopard. | QESeal provider not named. | 2026-10-05 | ❔ |
| BU3-3 | Issuing VAT ID (tax portal) | QEAA, QES, QESeal | QEAA: Reconi, Signicat, Digidentity, Cleverbase. QES: Cleverbase, KPN, Digidentity, Signicat. | QESeal provider not named. | 2026-10-05 | ❔ |
| BU3-4 | Issuing VAT ID using M2M | QEAA, QES, QESeal, QERDS | QEAA: Cleverbase, Signicat. QES: Cleverbase, Signicat. | QESeal and QERDS providers not named. | 2026-10-05 | ❔ |
| BU4-1A | Issue a power of attorney (PoA) | QEAA | Registradores, T-Systems, Cleverbase, Docaposte, Intesi Group | Qualification of the PoA as QEAA still under discussion. | 2026-10-05 | ❔ |
| BU4-1B | Issue powers of representation (PoR) to an EU Business Wallet | QEAA | Registradores, T-Systems, Cleverbase, Docaposte, Intesi Group | | 2026-10-05 | ❔ |
| BU4-1C | Get access to service | QEAA | Registradores, T-Systems, Cleverbase, Docaposte, Intesi Group | | 2026-10-05 | ❔ |
| BU5-1 | Micro-credentials | None | — | | 2026-10-05 | ⚪ |
| BU6-1 | Full power access to OOTS | QEAA or Pub-EAA | Public bodies in a combined QEAA/Pub-EAA role | No QTSP named; PL not filled in. | 2026-10-05 | ❔ |
| BU6-4 | Attestations and OOTS combined | QEAA or Pub-EAA | Public bodies in a combined QEAA/Pub-EAA role; WP4-QEAA placeholder for PL | Draft specification (v0.1). | 2026-10-05 | ❔ |
| SC2-1 | Seamless onboarding across data space initiatives (Agri-X) | None in MVP | — | QES/QTSP role defined but not exercised. | 2026-10-05 | ⚪ |
| SC5-1 | Supplier pre-approval | None | — | WE BUILD QEAA only; attestation is a non-qualified EAA. | 2026-10-05 | ⚪ |
| SC5-2 | Service provider authorization | QEAA (pending analysis) | QEAA provider out of scope | Qualification is a working assumption pending analysis. | 2026-10-05 | ❔ |
| SC5-3 | Service provider authorization verifiable by tax administration (MVP+) | QEAA (pending analysis) | QEAA provider out of scope | Qualification is a working assumption pending analysis. | 2026-10-05 | ❔ |
| SC5-4 | Direct eInvoicing between business wallets | QERDS | Not assigned | QERDS provider is an open point. | 2026-10-05 | 🔴 |
| SC5-5 | Peppol enhancements (MVP+) | QESeal, QERDS | Not assigned | No separate scenario specification; QTSP role not assigned. | 2026-10-05 | 🔴 |
| PA1-1 | Open a personal bank account | QEAA | IDnow Trust Services, InfoCert | | 2026-10-05 | ❔ |
| PA1-3 | Contract signing in banking KYC and onboarding (QES) | QEAA, QES | IDnow Trust Services | Draft specification (v0.82). | 2026-10-05 | ❔ |
| PA1-4 | Issue attestations and micro-credentials to the wallet | None | — | | 2026-10-05 | ⚪ |
| PA2-A1 | A2A strong customer authentication | None | — | | 2026-10-05 | ⚪ |
| PA2-A2 | A2A payment initiation | None | — | QES explicitly "won't have". | 2026-10-05 | ⚪ |
| PA2-B1 | Card-based online payment SCA with EUDIW | None | — | | 2026-10-05 | ⚪ |
| PA2-B2 | Card-based payment initiation with EUDIW | None | — | | 2026-10-05 | ⚪ |
| PA3-1 | Opening a bank account | QEAA, QES | QEAA: Digidentity, Procivis (with QTSPs). QES: Digidentity, D-Trust, Procivis (with QTSPs). | | 2026-10-05 | ❔ |
| PA3-2 | Digital signatures | QEAA, QES | QEAA: Digidentity, PWPW, D-Trust (tbc). QES: Digidentity, PWPW, D-Trust. | Draft specification (no version). | 2026-10-05 | ❔ |
| PA3-3 | IBAN ownership verification | None | — | | 2026-10-05 | ⚪ |
| PA4-1.1 | Card payment / deferred invoice (eReceipt) | None | — | Trust infrastructure for (Q)EAAs is an open point. | 2026-10-05 | ⚪ |
| PA4-1.2 | IBAN payment / upfront invoice (eReceipt) | None | — | | 2026-10-05 | ⚪ |

## Scenario details

The details below record what the v1.0 specifications say, including roles
that are not scored, such as Pub-EAA, EAA and PID providers.

### BU1 – Know Your Customer / Supplier / Business partner

#### BU1-1 – KYC (B2B vertical)

- **Source:** `BU1/bu_1_scenario_specification_scen-2.docx`
- **Q services:** QEAA. QES explicitly marked n/a. No QESeal or QERDS.
- **QEAA:** Procivis and Spherity, both *interim, technology for QTSP*.
- **Pub-EAA:** Bundesanzeiger Verlag, KVK, Dutch Tax Agency, DATEV,
  Infogreffe/Docaposte.
- **Wallets:** EBW only: Spherity, Procivis, Credenco.
- **Note:** some holders require the relying party to use an EBW instead of
  an RP component when requesting confidential data.

#### BU1-2 – KYS (B2B vertical)

- **Source:** `BU1/bu_1_scenario_specification_scen.docx`
- **Q services:** QEAA. QES explicitly marked n/a. No QESeal or QERDS.
- **QEAA:** Procivis and Spherity, both *interim, technology for QTSP*.
- **Pub-EAA:** Bundesanzeiger Verlag, KVK, Dutch Tax Agency, DATEV,
  Infogreffe/Docaposte.
- **Wallets:** EBW only: Spherity, Procivis, Credenco.

#### BU1-3 – KYC for sole traders, and BU1-4 – KYS for sole traders

- **Source:** `BU1/scenario_specification_sc_3_4_v1.docx` (one document
  for both scenarios)
- **Q services:** QEAA. QES "not needed for the scenarios". No QESeal or
  QERDS.
- **QEAA:** Docaposte, Cleverbase, Procivis (tbc), Signicat (tbc).
- **Pub-EAA:** KvK, Infogreffe, Dutch Tax Agency, Bundesanzeiger,
  Bolagsverket, BRREG, MRIT; tbc: IRN/Arte, Skatteverket, Finnish Tax
  Agency, Infocamere.
- **Wallets:** Docaposte (EBW), Digidentity (EUDI), Dutch EUDI wallet
  (EUDI); tbc: iGrant.io (hybrid), Credenco (EBW), SIROS (EBW/hybrid),
  Procivis (EBW).

#### BU1-5 – Know Your Employee (KYE)

- **Source:** `PA1/BU1 Scenario specifications - Scenario5 KYE V1.0.docx`
  (a BU1 scenario, filed under PA1)
- **Q services:** QEAA. QES n/a. No QESeal or QERDS.
- **QEAA:** SwissSign.
- **EAA:** SICPA, Bosch.
- **Wallets:** EUDI: Credenco, walt.id. No EBW provider listed.

### BU2 – Create company branch

#### BU2-1 – Create a company branch in an EU or EEA Member State

- **Source:** `BU2/BU2-Specifications-createCompanyBranch.md`
- **Q services:**
  - QEAA for EUCC and EU PoA issuance.
  - QES: remote QES over the application, declaration or notarial deed,
    through a QTSP-backed signature creation application.
  - QESeal: listed as required QTSP capability (QES/QESeal signing,
    qualified time stamps).
  - QERDS: MVP+ only, for confirmation or notification to the company in
    the home Member State.
- **QEAA, EAA, QES:** D-Trust. QEAA issuance and qualified signing are
  assumed to be delivered by WP4.
- **Wallets:** EUDI and EBW: iGrant.io, Credenco, Procivis, SIROS. The user
  must hold both an EUDI wallet and an EU Business Wallet.

#### BU2-2 – Register a company branch with a tax authority

- **Source:** `BU2/BU2-Specifications-taxAuthorityRegistration.md`
- **Q services:**
  - QEAA: EUCC and attestations can be issued as QEAA by a business
    register, QTSP, notary or the legal entity.
  - QES: remote QES over the application or declaration, including
    first-time QES certificate enrolment with the QTSP.
  - No QESeal or QERDS in scope.
- **QEAA, EAA, QES:** not filled in. Assumed to be delivered by WP4.
- **Wallets:** EUDI and EBW: iGrant.io, Credenco, Procivis, SIROS.

### BU3 – Foreign tax declaration (VAT)

#### BU3-1 – Filing VAT declaration using tax portal

- **Source:** `BU3/bu3_s1_vatportal_specs_1.0.docx`
- **Q services:**
  - QES: the natural person approves the VAT declaration with a QES through
    the EUDI wallet; also used in sub-scenario b.3a for issuing an
    authorisation attestation.
  - QESeal: VAT declaration receipt sealed by the tax administration;
    sub-scenario b.3b issues the authorisation attestation with a seal.
  - QEAA.
  - No QERDS.
- **QEAA:** Reconi, Signicat, Digidentity, Cleverbase.
- **QES:** Cleverbase, KPN, Digidentity, Signicat.
- **QESeal:** no provider named.
- **PID:** Digidentity, KPN, Cleverbase, Signicat, Digdir, DIGG.
- **Wallets:** EUDI: Cleverbase, Digidentity, KPN, Digdir. Users of EUDI
  and business wallet are mocked; no separate EBW provider listed.

#### BU3-2 – Filing VAT declaration using M2M

- **Source:** `BU3/bu3_s2_vatm2m_specs_v1.0.docx`
- **Q services:**
  - QESeal: the intermediary's qualified electronic seal protects the
    filing (origin and integrity); also sub-scenario 2b-1b.
  - QERDS: exchanges the document and the attestations together in M2M
    interaction with the business wallet.
  - QES (variation): an authorised natural person countersigns the filing
    with a QES from their EUDI wallet.
  - QEAA.
- **QEAA:** Reconi, Signicat, Digidentity.
- **QES:** Cleverbase, KPN, Digidentity, Signicat.
- **QERDS:** Cleverbase, Ledger Leopard.
- **QESeal:** no provider named.
- **Wallets:** EUDI: Cleverbase, Digidentity, KPN, Digdir. EBW: Credenco;
  QEAA and QERDS are delivered towards the business wallet.

#### BU3-3 – Issuing VAT ID (tax portal)

- **Source:** `BU3/bu3_s3_vatidportal_specs_1.0.docx`
- **Q services:**
  - QEAA: the VAT ID certificate can be issued as QEAA by a QTSP
    (alternative: Pub-EAA by the tax administration).
  - QESeal: the Pub-EAA variant is sealed by the tax administration;
    sub-scenario b.3b issues the authorisation attestation with a seal.
  - QES: sub-scenario b.3a, confirmation of the authorisation register
    entry through the wallet.
  - No QERDS.
- **QEAA:** Reconi, Signicat, Digidentity, Cleverbase.
- **QES:** Cleverbase, KPN, Digidentity, Signicat.
- **QESeal:** no provider named.
- **Pub-EAA:** Netherlands Tax Administration.
- **Wallets:** EUDI: Cleverbase, Digidentity, KPN, Digdir. EBW: Credenco,
  Ledger Leopard, Cleverbase. The VAT ID must be issuable to both an EUDI
  wallet and a business wallet.

#### BU3-4 – Issuing VAT ID using M2M

- **Source:** `BU3/bu3_s4_vatidm2m_specs_1.0.docx`
- **Q services:**
  - QESeal: the intermediary company's identity is guaranteed by a
    qualified electronic seal on the VAT ID request; the Pub-EAA variant is
    sealed by the tax administration.
  - QEAA: VAT ID certificate issued by a QTSP that verifies the attributes
    at the tax administration.
  - QES: where national law requires a natural person's approval.
  - QERDS: referenced as the M2M exchange mechanism per the WE BUILD
    architecture.
- **QEAA:** Cleverbase, Signicat.
- **QES:** Cleverbase, Signicat.
- **QESeal, QERDS:** no provider named.
- **Pub-EAA:** Netherlands Tax Administration.
- **Wallets:** EUDI: Cleverbase, Digidentity, KPN, Ubiqu. Primarily a
  business wallet scenario; the Pub-EAA is issued towards the business
  wallet, the EUDI wallet is optional for confirming authorisations.

### BU4 – Company representative acting on behalf of company

#### BU4-1A – Issue a power of attorney (PoA)

- **Source:** `BU4/scenarios/1A-PoA-Issuance`
- **Q services:** QEAA. The PoA is signed or sealed by the trust
  infrastructure and transmitted to the wallet as a QEAA; its qualification
  is still under discussion. QES only appears as an optional user
  authentication method and as a risk (the wallet must be able to reach a
  QES provider). No QESeal or QERDS.
- **QEAA:** Registradores, T-Systems, Cleverbase, Docaposte, Intesi Group.
- **EAA:** Registradores, T-Systems, DATEV, Cleverbase, Docaposte, Intesi
  Group.
- **QES:** empty.
- **Wallets:** EBW: Procivis, Credenco. No EUDI wallet (EUBW-based MVP).

#### BU4-1B – Issue powers of representation (PoR) to an EU Business Wallet

- **Source:** `BU4/scenarios/1B-PoR-Issuance`
- **Q services:** QEAA. The PoR is issued by a QTSP acting on behalf of an
  authentic source, or by a public body; a dedicated *QEAA (QTSP) provider*
  role with requirements is defined. No QES provider assigned. No QESeal or
  QERDS.
- **QEAA:** Registradores, T-Systems, Cleverbase, Docaposte, Intesi Group.
- **EAA:** Registradores, T-Systems, DATEV, Cleverbase, Docaposte, Intesi
  Group.
- **Wallets:** EBW: Procivis, Credenco. No EUDI wallet.

#### BU4-1C – Get access to service

- **Source:** `BU4/scenarios/1C-Service-Access`
- **Q services:** QEAA. PoA and PoR are issued as QEAA by QTSPs; the PoE is
  issued directly by organisations as non-qualified EAA. No QES provider
  assigned. No QESeal or QERDS.
- **QEAA:** Registradores, T-Systems, Cleverbase, Docaposte, Intesi Group.
- **EAA:** Registradores, T-Systems, DATEV, Cleverbase, Docaposte, Intesi
  Group.
- **Trusted list registrar:** IDunion.
- **Wallets:** EBW: Procivis, Credenco; Orange is also listed as wallet
  provider. End users access services through both the EUDI wallet and the
  European Business Wallet.

### BU5 – Issue micro-credentials

#### BU5-1 – Micro-credentials

- **Source:** `BU5/wp2_bu5_microcredential_scenario.docx`
- **Q services:** none in the MVP. QEAA and Pub-EAA "n/a in MVP", QES "n/a
  in BU5". No QESeal, QERDS or RP certificates specified.
- **EAA:** the authentic sources (Linköping University, University of the
  Aegean) issue their own credentials. Trusted lists are supplied by the WE
  BUILD project.
- **Wallets:** EUDI: GRNET, GUnet (wwWallet), Procivis (Procivis One),
  E-gov Moldova (Evo), Sphereon, SIROS (SIROS ID). No EBW.

### BU6 – Business access to OOTS

#### BU6-1 – Full power access to OOTS

- **Source:** `BU6/2026_06_18_scenario_1_v1.0.docx`
- **Q services:** QEAA or Pub-EAA: a combined role for issuing the PoR
  attestation, Member State specific. QES n/a. No QESeal or QERDS.
- **QEAA / Pub-EAA (PoR):** GRNET/MoDG, ARTE, MDT, KVK; PL not filled in.
- **PID:** GRNET/MoDG, MC, ARTE, MDT, KVK.
- **Wallets:** EUDI: GRNET/MoDG, Ministry of Digital Affairs (PL), ARTE,
  MDT, KVK. No EBW listed.

#### BU6-4 – Attestations and OOTS combined

- **Source:** `BU6/BU6-4_scenario_specification_v1.0.docx`
- **Q services:** QEAA or Pub-EAA, which must be able to issue both PoR and
  CR attestations (Member State specific). QES n/a. No QESeal or QERDS.
- **QEAA / Pub-EAA (PoR):** GRNET/MoDG, WP4-QEAA (PL), ARTE, MDT, KVK.
- **EAA:** must also be able to issue PoR and CR attestations.
- **Wallets:** EUDI: GRNET/MoDG, Ministry of Digital Affairs (PL), ARTE,
  MDT, KVK. No EBW listed.

### SC2 – Trusted data sharing for data spaces

#### SC2-1 – Seamless onboarding across data space initiatives (Agri-X)

- **Source:** `SC2/Specification Scenario 1 - MVP v1.0.pdf`
- **Q services:** none in the MVP. QEAA "not used in MVP (future role)",
  Pub-EAA "not explicitly defined in MVP". A QES provider / QTSP role is
  defined but not exercised in the flow. No QESeal or QERDS. The membership
  credential is a W3C VC / EAA.
- **QES / QTSP:** Signicat (potential), Digidentity. The WP4 QTSP group
  supplies mock data: Spherity, Digidentity.
- **EAA:** ILVO–DjustConnect, ITC–DADS, Dataspace Europe–Tritom.
- **Trusted list:** LoTE set up by the use case lead in the WP4 LoTL
  (Raidiam); credential catalogue by SIROS.
- **Wallets:** Spherity (EBW), Digidentity (EUDI and EBW), LutraLabs (EBW).

### SC5 – eInvoicing

Shared across SC5: EAA provider DATEV; QES provider role ValidatedID and
Banqup; trusted list registrar IDunion; EBW providers Sphereon and Credenco.
No EUDI wallet: SC5 is fully EBW and system-to-system. RP certificates are
WE BUILD RP access certificates; WE BUILD does not issue eIDAS RP access
certificates.

Sources: `SC5/Scenario/Scenario1.md` to `Scenario4.md`, and
`SC5/Scenario/Description.md`.

#### SC5-1 – Supplier pre-approval

- **Q services:** QEAA only as *WE BUILD QEAA*: technically interoperable
  and ITB-tested, without eIDAS qualification. The Approved Supplier
  attestation is deliberately a non-qualified EAA. No QES, QESeal or QERDS.
- **QEAA, Pub-EAA:** out of scope.
- **Pilots:** Banqup (supplier) and Sphereon (buyer); pilot 3: ValidatedID
  (supplier) and GUnet (buyer).

#### SC5-2 – Service provider authorization

- **Q services:** the Authorized Service Provider attestation is an EAA;
  qualification as QEAA is a working assumption pending analysis. No QES,
  QESeal or QERDS.
- **QEAA, Pub-EAA:** out of scope.
- **Pilots:** Banqup / Sphereon (pilot 1), TBD from IDunion (pilot 2),
  ValidatedID / GUnet (pilot 3).

#### SC5-3 – Service provider authorization verifiable by tax administration (MVP+)

- **Q services:** as SC5-2. No QES, QESeal or QERDS.
- **QEAA, Pub-EAA:** out of scope.
- **Tax authorities** on supplier and buyer side: TBD.

#### SC5-4 – Direct eInvoicing between business wallets

- **Q services:**
  - QERDS: discussed as the alternative transport next to OID4VP/DCQL
    when legally strong, dispute-proof delivery evidence (proof of sending,
    proof of receipt) is required. Sequence diagrams model Supplier_QERDS
    and Buyer_QERDS.
  - The eInvoice attestation is issued by the supplier's business wallet
    and is not qualified.
- **QERDS:** not yet assigned (open point).
- **Pilot 4:** Robert Bosch as supplier and buyer, Sphereon as
  supplier-side wallet.

#### SC5-5 – Peppol enhancements (MVP+)

- **Source:** `SC5/Scenario/Description.md` only; no separate scenario
  specification.
- **Q services:** the QTSP is modelled as trust service provider for
  QESeal and ERDS/QERDS. QEAA as in SC5-1 to SC5-3.
- **QESeal, QERDS:** not yet assigned to a named partner.

### PA1 – Consumer banking

#### PA1-1 – Open a personal bank account

- **Source:** `PA1/WEBUILD WP3 PA1-01 specification v1.0.docx`
- **Q services:** QEAA. No QES, QESeal or QERDS.
- **RP certificates (not scored):** central to the scenario. The RP or its
  intermediary must obtain a WRPAC from an access CA and, where issued,
  present a WRPRC. Alternative flows AF7/AF8 cover failed access
  certificate validation and out-of-scope requests.
- **QEAA:** IDnow Trust Services, InfoCert.
- **EAA:** BankID CZ, Mastercard.
- **Trust infrastructure:** WP4 (Raidiam as trusted list registrar,
  IDunion for the trust list / LoTE).
- **Note:** Tax ID and certificate of residence are mocked by the
  attestation providers; no authentic source available.
- **Wallets:** EUDI: walt.id, Aricoma, Lissi, Compellio, Samsung, Izertis.
  No EBW.

#### PA1-3 – Contract signing in banking KYC and onboarding of end users (QES)

- **Source:** `PA1/webuild_wp3_pa1_03_specification.docx`
- **Q services:** QES is the core: a short-term qualified certificate is
  issued and a QES with qualified timestamp is placed on the contract.
  QTSP-centric model with remote QSCD; the wallet-centric model with local
  QSCD is MVP+. Also QEAA. No QESeal or QERDS.
- **QEAA, QES:** IDnow Trust Services (the only QTSP for this scenario).
- **Trusted list registrar:** national supervisory bodies maintaining the
  EU trusted lists.
- **Wallets:** EUDI: walt.id (with National Bank of Greece) and Izertis
  (with Banca Sella); other PA1 wallets are usable if the RPs extend
  coverage. No EBW.

#### PA1-4 – Issue attestations and micro-credentials to the wallet

- **Source:** `PA1/WEBUILD WP3 PA1-04 specification v1.0.docx`
- **Q services:** none. SCA-User, SCA-IBAN and IBAN Ownership are
  non-qualified EAA in SD-JWT, device-bound, through OpenID4VCI.
- **EAA:** BankID CZ, Mastercard. Banks as authentic sources: National
  Bank of Greece, Banca Sella, Banca Transilvania, Air Bank, Komerční Banka,
  ČSOB. WUA supplied by the WP4 wallet group.
- **Wallets:** EUDI: Lissi, Compellio, Samsung, Aricoma, Izertis. No EBW.

### PA2 – Consumer payments

#### PA2-A1 – A2A strong customer authentication

- **Source:** `PA2/wp3_pa2_a1_mvp_v1.0_.docx`
- **Q services:** none. The (Q)EAA, Pub-EAA and QES provider roles "do not
  independently request, issue or verify credentials" in this flow.
- **Roles:** the bank (ASPSP) is both authentic source/issuer and relying
  party for the SCA-IBAN attestation. Banks: National Bank of Greece,
  Komerční banka. PISP: Mastercard Open Banking Services EU, Tink.
- **Wallets:** EUDI: Aricoma. No EBW.

#### PA2-A2 – A2A payment initiation

- **Source:** `PA2/wp3_pa2_a2_mvp_v1.0.docx`
- **Q services:** none. QES capability is explicitly "won't have".
- **RP certificates (not scored):** the merchant or its PISP must be
  registered as relying party and hold a valid WE BUILD RP access
  certificate.
- **EAA:** Bank iD CZ, Fast Ferries. **PID:** GRNET, Aricoma. SCA-IBAN
  from PA1. PISP: Google, Worldline.
- **Wallets:** EUDI: Aricoma, iGrant.io. No EBW.

#### PA2-B1 – Card-based online payment SCA with EUDIW

- **Source:** `PA2/wp3_pa2_b1_mvp_v1.0.docx`
- **Q services:** none. QEAA n/a ("assumption that this scenario's
  attestations do not require qualified services"). SCA-card (DPC) and
  SCA-user are non-qualified EAA.
- **EAA:** Veridas (for Caixabank), Visa (for Banca Transilvania and
  SpareBank 1); possible support from Authologic, iGrant.io, Lissi,
  Netcetera, Mastercard, Worldline.
- **Wallets:** EUDI: Google Wallet (tbc), GRNET, iGrant.io, Veridas. No EBW.

#### PA2-B2 – Card-based payment initiation with EUDIW

- **Source:** `PA2/wp3_pa2_b2_mvp_v1.0.docx`
- **Q services:** none. The digital payment credential is a non-qualified
  EAA; Mastercard acts as validator TSP (non-qualified trust service).
- **Roles:** EAA provider and validator TSP: Mastercard. Credential issuance
  facilitator: G+D Netcetera. ACS: Worldline, Entersekt. PSP: Worldline.
- **Wallets:** EUDI: iGrant.io, walt.id, Google. No EBW.

### PA3 – Corporate banking

#### PA3-1 – Opening a bank account

- **Source:** `PA3/Specification - PA3 SC1 - Current - Opening Bank Account.docx`
- **Q services:**
  - QEAA is the backbone: EBWOID (Pub-EAA or QEAA), EUCC issued by a QTSP,
    tax and VAT information, the UBO list from the transparency register,
    and PID through QEAA.
  - QES: the signature process runs through QTSPs (for example D-Trust,
    Digidentity).
  - No QESeal or QERDS explicitly.
- **QEAA:** Digidentity, Procivis in collaboration with QTSPs; QTSPs over
  the transparency register for the UBO list (for example Bundesanzeiger).
- **QES:** Digidentity, Procivis in collaboration with QTSPs; D-Trust in
  the flow description.
- **EAA:** self-issued attestations by companies. Pub-EAA: n/a.
- **Wallets:** EUDI (natural person): Digidentity, Procivis, Governikus,
  SWIYU Swiss reference implementation, Aricoma. EBW (legal person):
  Spherity, Procivis.

#### PA3-2 – Digital signatures

- **Source:** `PA3/specification_pa3_sc2_digital_si.docx`
- **Q services:** QES is the core: legal representatives sign the bank
  account contract with a QES from their EUDI wallet; the bank portal acts
  as signature creation application. The EUCC can be issued as QEAA on
  behalf of the business register (alternatively as Pub-EAA). No QESeal or
  QERDS.
- **QEAA:** Digidentity, PWPW, D-Trust (tbc).
- **QES:** Digidentity, PWPW, D-Trust.
- **Pub-EAA:** KvK, Bundesanzeiger Verlag, CEIDG.
- **PID:** Digidentity (Q-PID), Bundesdruckerei, PESEL / Ministry of
  Digital Affairs (PL, tbc).
- **Wallets:** EUDI: Digidentity, Procivis; other TBD for DE and PL. EBW:
  Credenco, Spherity, Procivis; other TBD for PL. The bank also needs a
  business wallet to issue the contract.

#### PA3-3 – IBAN ownership verification

- **Source:** `PA3/specification_pa3_sc3_current_ba.docx`
- **Q services:** none. The IBAN-OV attestation is a non-qualified EAA
  issued by the bank from its business wallet. The wallets perform no full
  verification of issuers ("assumed trust").
- **EAA:** Deutsche Bank.
- **Wallets:** EUDI: Procivis. EBW: Spherity.

### PA4 – Corporate payments

#### PA4-1.1 – Card payment / deferred invoice (eReceipt)

- **Source:** `PA4/pa4_scenario_1.1_card_payment_er.docx`
- **Q services:** none. All attestations (eReceipt, payment attestation,
  eReceipt routing credential, invoice payment confirmation) are EAA issued
  from the business wallet as SD-JWT VC. The trust infrastructure for
  (Q)EAAs and certificates is flagged as an open point.
- **EAA:** merchant or PSP/acquirer through the eReceipt intermediary
  (ReceiptHero); OP Bank for the card attestation; employer through EBW/HR
  system. Intermediaries: University of the Aegean (wallet integration),
  Worldline (PSP acquiring).
- **Wallets:** EBW: iGrant.io (buyer 2, travel agent), Procivis (seller).
  EUDI: iGrant.io (buyer 1, sole trader).

#### PA4-1.2 – IBAN payment / upfront invoice (eReceipt)

- **Source:** `PA4/pa4_scenario_1.2_iban_payment_er.docx`
- **Q services:** none. All attestations are EAA issued from the business
  wallet as SD-JWT VC.
- **EAA:** merchant or PSP/acquirer through the eReceipt intermediary;
  Deutsche Bank for the IBAN attestation; employer through EBW/HR system.
  Intermediaries: DATEV (buyer's ERP), Bosch ERP (eReceipt).
- **Wallets:** EBW: SIROS (buyer), Procivis (seller). No EUDI wallet.
