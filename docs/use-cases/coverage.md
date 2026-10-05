# Use case coverage

Per use case, whether the trust services in scope that it needs have
enough providers attached. See the [legend and update instructions](README.md).

The service columns give the number of providers attached across the
scenarios of the use case. Providers marked *tbc* or *technology for QTSP*
are counted between brackets and do not count towards the two-provider
threshold. `—` means the service is not required.

> [!NOTE]
> This overview is based on the use case scenario specifications from
> April 2026 and may be outdated. If the status or the parties involved in
> your use case have changed, please update the table, see
> [how to update](README.md#how-to-update).

## Overview

| Use case | Title | QEAA | QES | QESeal | QERDS | RPAC/RPRC | Notes | Updated | Status |
|---|---|---|---|---|---|---|---|---|---|
| BU1 | Know Your Customer / Supplier / Business partner | 3 (+3) | — | — | — | 0 | BU1-1 and BU1-2 have only technology providers. RPAC/RPRC not specified. | 2026-10-05 | 🟡 |
| BU2 | Create company branch | 1 | 1 | 0 | MVP+ | 0 | QESeal not explicitly assigned; only D-Trust attached; BU2-2 has no providers. RPAC/RPRC not specified. | 2026-10-05 | 🟡 |
| BU3 | Foreign tax declaration (VAT) | 4 | 4 | 0 | 2 | 0 | QESeal provider not named in any scenario. RPAC/RPRC not specified. | 2026-10-05 | 🟡 |
| BU4 | Company representative acting on behalf of company | 5 | — | — | — | 0 | QES only optional; qualification of the PoA still under discussion. RPAC/RPRC not specified. | 2026-10-05 | 🟡 |
| BU5 | Issue micro-credentials | — | — | — | — | 0 | RPAC/RPRC not specified. | 2026-10-05 | 🔴 |
| BU6 | Business access to OOTS | 0 (+1) | — | — | — | 0 | QEAA or Pub-EAA; filled by public bodies, QEAA only as WP4 placeholder for PL. RPAC/RPRC not specified. | 2026-10-05 | 🟡 |
| SC2 | Trusted data sharing for data spaces | — | — | — | — | 0 | QES/QTSP role defined but not exercised in the MVP. RPAC/RPRC not specified. | 2026-10-05 | 🔴 |
| SC5 | eInvoicing | 0 | — | MVP+ | 0 | 0 | QERDS in SC5-4 not assigned; QEAA qualification pending analysis. RPAC/RPRC specified, no issuer named. | 2026-10-05 | 🔴 |
| PA1 | Consumer banking | 2 | 1 | — | — | 0 | QES only by IDnow Trust Services. RPAC/RPRC specified in PA1-1 only, no issuer named. | 2026-10-05 | 🟡 |
| PA2 | Consumer payments | — | — | — | — | 0 | RPAC/RPRC specified in PA2-A2 only, no issuer named. | 2026-10-05 | 🔴 |
| PA3 | Corporate banking | 2 (+2) | 3 (+1) | — | — | 0 | RPAC/RPRC not specified. | 2026-10-05 | 🟡 |
| PA4 | Corporate payments | — | — | — | — | 0 | Trust infrastructure for (Q)EAAs is an open point. RPAC/RPRC not specified. | 2026-10-05 | 🔴 |

## Providers per use case

### RPAC/RPRC (all use cases)

No specification names a TSP as issuer of RPAC/RPRC.

- **PA1-1:** WRPAC from an access CA and WRPRC; WP4 trust infrastructure
  with Raidiam as trusted list registrar and IDunion for the trust list /
  LoTE.
- **PA2-A2:** WE BUILD RP access certificate for the merchant or its PISP.
- **SC5:** WE BUILD RP access certificates; IDunion as trusted list
  registrar.
- **All other scenarios:** not specified.

### BU1 – Know Your Customer / Supplier / Business partner

- **QEAA:** Docaposte, Cleverbase, SwissSign; tbc: Procivis, Signicat;
  technology for QTSP: Procivis, Spherity.

### BU2 – Create company branch

- **QEAA:** D-Trust.
- **QES:** D-Trust.
- **QESeal:** required in BU2-1, not explicitly assigned.
- **QERDS:** MVP+ in BU2-1, not assigned.

### BU3 – Foreign tax declaration (VAT)

- **QEAA:** Reconi, Signicat, Digidentity, Cleverbase.
- **QES:** Cleverbase, KPN, Digidentity, Signicat.
- **QESeal:** required in all four scenarios, no provider named.
- **QERDS:** Cleverbase, Ledger Leopard (BU3-2); BU3-4 has no provider
  named.

### BU4 – Company representative acting on behalf of company

- **QEAA:** Registradores, T-Systems, Cleverbase, Docaposte, Intesi Group.

### BU6 – Business access to OOTS

- **QEAA or Pub-EAA:** GRNET/MoDG, ARTE, MDT, KVK as public bodies;
  WP4-QEAA placeholder for PL (BU6-4).

### SC5 – eInvoicing

- **QEAA:** out of scope; qualification pending analysis (SC5-2, SC5-3).
- **QES:** ValidatedID and Banqup in the shared SC5 role, but no scenario
  uses QES.
- **QESeal:** MVP+ (SC5-5), not assigned.
- **QERDS:** SC5-4 not assigned; SC5-5 (MVP+) not assigned.

### PA1 – Consumer banking

- **QEAA:** IDnow Trust Services, InfoCert.
- **QES:** IDnow Trust Services.

### PA3 – Corporate banking

- **QEAA:** Digidentity, PWPW; tbc: D-Trust; with QTSPs: Procivis.
- **QES:** Digidentity, PWPW, D-Trust; with QTSPs: Procivis.
