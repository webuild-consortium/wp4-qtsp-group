# Use case tracking

This section of the [QTSP documentation](../README.md) monitors how
qualified trust services are applied in the WE BUILD use cases, and which
providers deliver them.

- [Scenarios](scenarios.md): implementation status of the qualified trust
  service part of each use case scenario.
- [Use case coverage](coverage.md): per use case, whether the qualified
  trust services it needs have enough providers attached.
- [Providers](providers.md): providers that deliver qualified trust
  services within WE BUILD, and their involvement in the QTSP group and the
  use cases.

## Scope

The tracking covers these trust services:

| Abbreviation | Service |
|---|---|
| QEAA | Qualified electronic attestation of attributes |
| QES | Qualified electronic signature (natural person) |
| QESeal | Qualified electronic seal (legal person) |
| QERDS | Qualified electronic registered delivery service |
| RPAC/RPRC | Relying party access certificate and registration certificate (WRPAC/WRPRC) |

RPAC/RPRC is not a qualified trust service, but its issuance is in scope
of the QTSP group, see the [RPAC/RPRC documentation](../rpac-rprc/README.md).
According to the WE BUILD blueprint, relying parties, PID providers and
attestation providers need at least one access certificate per service
([trust ecosystem](https://github.com/webuild-consortium/wp4-architecture/blob/main/blueprint/appendix-trust-ecosystem.md)).
RPAC/RPRC is therefore considered required in every scenario. Where a
specification does not mention it, this is recorded as *not specified*,
so that it can be followed up with the use case.

Pub-EAA and non-qualified EAA are mentioned in the scenario details for
context, but are not scored.

A *provider* is any organisation that delivers one of these services in a
WE BUILD scenario, **whether or not it holds a qualified status** for that
service. This allows monitoring the application of qualified trust
services during the pilots, including providers that supply technology to
a QTSP or deliver a WE BUILD variant without legal qualification. The
[providers](providers.md) list records the qualification status
separately.

## Status legend

### Scenarios

Scores the qualified trust service part of the scenario, which is what
the QTSP group can influence, not the pilot as a whole.

| Status | Meaning |
|---|---|
| 🟢 Working | The qualified trust services work in the pilot according to the WE BUILD specifications, and are tested (link the test results). |
| 🟡 In progress | It is clear what needs to be done, but work remains. |
| 🔴 Blocked | It is unclear what needs to be done: a blocking issue, missing specifications or no provider assigned (link the issue). |
| ⚪ Not applicable | The scenario uses none of the services in scope. |
| ❔ Not assessed | No status has been assessed yet. |

### Use case coverage

Scored over the required services of the use case. Services marked MVP+
are listed but not scored. Services with too few providers are explained
in the *Notes* column.

| Status | Meaning |
|---|---|
| 🟢 Covered | Qualified trust services are applied, and each required service has at least two providers attached. |
| 🟡 Limited | Qualified trust services are applied, but a required service has fewer than two providers, or only providers marked *tbc*. |
| 🔴 Gap | Qualified trust services are required, but none of them has a provider assigned. |
| ⚪ Not applicable | The use case uses none of the services in scope. |

At least two providers per service are needed to demonstrate
interoperability between independent providers.

### Providers

| Status | Meaning |
|---|---|
| 🟢 Involved | Involved in both the QTSP group and at least one use case scenario. |
| 🟡 Partly involved | Involved in either the QTSP group or a use case scenario. |
| 🔴 Not involved | Involved in neither the QTSP group nor a use case scenario. |

## Sources and method

The initial content is based on the WE BUILD v1.0 scenario specifications
(April 2026), taken from the *Roles and participants* sections, the
attestation tables and the scenario flow descriptions. Where a
specification leaves a role blank or marks it n/a, that is stated
literally rather than inferred.

Points to be aware of:

- **WE BUILD QEAA is not an eIDAS QEAA.** SC5 states explicitly that WE
  BUILD does not issue eIDAS-qualified attestations or eIDAS RP access
  certificates. It issues WE BUILD variants that are technically
  interoperable and ITB-tested, but carry no legal qualification. The same
  pilot caveat applies across the project.
- **Technology providers.** In BU1 scenarios 1 and 2, Procivis and
  Spherity are listed as QEAA provider with the annotation *interim,
  technology for QTSP*. They supply the technology, not the qualified
  status.
- **RPAC/RPRC** are only described in PA1 scenario 1, PA2 scenario A.2
  and SC5, and no specification names a TSP as issuer. Other scenarios
  rely on them implicitly through the WP4 trust infrastructure. This is an
  action point for the QTSP group and the use cases.
- **Draft specifications.** BU1 scenarios 3/4 (v0.7), BU6 scenario 4
  (v0.1), PA1 scenario 3 (v0.82) and PA3 scenario 2 (no version) were still
  drafts at the time of export and may change.
- **Wallet codes** used in some specifications: NW = natural person wallet
  (EUDI), BW = business wallet (EBW), NP/LP = natural/legal person (PA3),
  RW = reference wallet (PA1/PA2).
- **Country codes** are recorded once per provider in the
  [providers](providers.md) list, since the specifications are not
  consistent about them.

## How to update

The content may be outdated, since the scenario specifications have
evolved since April 2026. Use case and provider contacts are asked to
update the tables when the status or the parties involved have changed.

Changes follow the normal [contribution process](../../README.md#contributing):
fork the repository and create a pull request. Small edits can be made
directly in the GitHub web editor.

When changing a status:

1. Update the status in the rightmost column.
2. Add a short reason in the *Notes* column and set the *Updated* date
   (YYYY-MM-DD).
3. For 🔴, link the GitHub issue that describes the blocking point.
4. For 🟢 scenarios, link the central test results rather than copying
   them.

Use the provider names exactly as in the [providers](providers.md) list.
For providers in the [QTSP catalogue](../../catalogue/catalogue.md), use
the name of their catalogue entry.
