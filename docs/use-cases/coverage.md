# Use case coverage

Per use case, whether the qualified trust services it needs have enough
providers attached. See the [legend and update instructions](README.md).

The service columns give the number of providers attached across the
scenarios of the use case. Providers marked *tbc* or *technology for QTSP*
are counted between brackets and do not count towards the two-provider
threshold. `—` means the service is not required.

## Overview

| Use case | Title | QEAA | QES | QESeal | QERDS | Notes | Updated | Status |
|---|---|---|---|---|---|---|---|---|
| BU1 | Know Your Customer / Supplier / Business partner | 3 (+3) | — | — | — | BU1-1 and BU1-2 have only technology providers. | 2026-10-05 | 🟢 |
| BU2 | Create company branch | 1 | 1 | 0 | MVP+ | QESeal not explicitly assigned; only D-Trust attached; BU2-2 has no providers. | 2026-10-05 | 🟡 |
| BU3 | Foreign tax declaration (VAT) | 4 | 4 | 0 | 2 | QESeal provider not named in any scenario. | 2026-10-05 | 🟡 |
| BU4 | Company representative acting on behalf of company | 5 | — | — | — | QES only optional; qualification of the PoA still under discussion. | 2026-10-05 | 🟢 |
| BU5 | Issue micro-credentials | — | — | — | — | | 2026-10-05 | ⚪ |
| BU6 | Business access to OOTS | 0 (+1) | — | — | — | QEAA or Pub-EAA; filled by public bodies, QEAA only as WP4 placeholder for PL. | 2026-10-05 | 🟡 |
| SC2 | Trusted data sharing for data spaces | — | — | — | — | QES/QTSP role defined but not exercised in the MVP. | 2026-10-05 | ⚪ |
| SC5 | eInvoicing | 0 | — | MVP+ | 0 | QERDS in SC5-4 not assigned; QEAA qualification pending analysis. | 2026-10-05 | 🔴 |
| PA1 | Consumer banking | 2 | 1 | — | — | QES only by IDnow Trust Services. | 2026-10-05 | 🟡 |
| PA2 | Consumer payments | — | — | — | — | | 2026-10-05 | ⚪ |
| PA3 | Corporate banking | 2 (+2) | 3 (+1) | — | — | | 2026-10-05 | 🟢 |
| PA4 | Corporate payments | — | — | — | — | Trust infrastructure for (Q)EAAs is an open point. | 2026-10-05 | ⚪ |

## Providers per use case

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
