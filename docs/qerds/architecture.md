# Architecture overview for QERDS in WE BUILD

## Context

### Purpose of this document

This document specifies the high-level architecture for the [provision of qualified electronic registered delivery services (QERDS)](README.md) within WE BUILD.
It aims to guide the specification, development, testing and implementation of QERDS.
It complements the [WE BUILD architecture documentation](https://github.com/webuild-consortium/wp4-architecture) which specifies, among other topics, the transmission of data within WE BUILD in general.

### Definitions

In WE BUILD, QTSPs as defined under [eIDAS] Art. 3(20) provide pre-production ERDS as defined under Art. 3(16)(g) and (h), technically ready to be audited for qualification as defined under Art. 3(17), for QERDS as defined under Art. 3(37).

[eIDAS]: https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A02014R0910-20241018

## Technical specifications for QERDS

### Functional decomposition

The following decomposition is inspired by [ETSI EN 319 521 v1.1.1], [ETSI EN 319 522-1 v1.2.1](https://www.etsi.org/deliver/etsi_en/319500_319599/31952201/01.02.01_60/en_31952201v010201p.pdf), and [ETSI TS 119 471 v1.1.1](https://www.etsi.org/deliver/etsi_ts/119400_119499/119471/01.01.01_60/ts_119471v010101p.pdf).

[ETSI EN 319 521 v1.1.1]: https://www.etsi.org/deliver/etsi_en/319500_319599/319521/01.01.01_60/en_319521v010101p.pdf

- **QERDS:** An electronic service which supports the following QERDS processes. It is provided by a QTSP which is on the *trusted list* upon auditing for conformance to general requirements and to specific EBW interoperability requirements and upon qualification for QEAA issuance.
    - **Message submission:** The *sender* uses the *QTSP* to submit a document or notification to a *wallet*.
    - **Message retrieval:** The *recipient* uses the *QTSP* to retrieve a document or notification in their *wallet*.
    - **Evidence retrieval:** The *QTSP* provides timestamped, sealed evidence to the *wallet* of the *sender* or *recipient* about the transmission.
    - **Message relay:** The *QTSP* relays messages from another ERDS, potentially a gateway during the European Business Wallet derogation period.
    - **Common service access:** The *QTSP* accesses message routing, trust management, capability management, and governance functions from common services such as the *European Digital Directory*.
- **Identity proofing service:** an electronic service by which the identity and additional attributes of an applying *subscriber* are verified. The verification process uses evidence attesting to the required identity attributes, including evidence from *PID/EAA presentation*.
- **Qualified electronic seal creation service (QESeal creation service):** an electronic service by which the *QTSP* attaches or logically associates with data an qualified electronic seal to ensure the data’s origin and integrity.
- **Qualified time stamp creation service (QTS creation service):** an electronic service by which the *QTSP* binds data to a particular time establishing evidence that the latter data existed at that time.

### Policy and security requirements

#### QERDS

The requirements from [(EU) 2025/1944](https://data.europa.eu/eli/reg_impl/2025/1944/oj) Annex apply, based on [ETSI EN 319 521 v1.1.1].

#### Identity proofing service

The requirements from [eIDAS] Art. 24(1a) and [ETSI TS 119 461 v2.1.1](https://www.etsi.org/deliver/etsi_ts/119400_119499/119461/02.01.01_60/ts_119461v020101p.pdf) apply.

#### QESeal creation service

The requirements from [eIDAS] Art. 35, Art. 38, Annex III, [ETSI EN 319 411-2 V2.6.1](https://www.etsi.org/deliver/etsi_en/319400_319499/31941102/02.06.01_60/en_31941102v020601p.pdf), [ETSI EN 319 401 V3.1.1](https://www.etsi.org/deliver/etsi_en/319400_319499/319401/03.01.01_60/en_319401v030101p.pdf) and [ETSI TS 119 431-1 V1.3.1](https://www.etsi.org/deliver/etsi_ts/119400_119499/11943101/01.03.01_60/ts_11943101v010301p.pdf) apply.

#### QTS creation service

The requirements from [eIDAS] Art. 42 apply.

### Deployment model and interfaces

The visualisations below represents a value chain, in which the primary data flows from left to right. The technical control flow may be reverse, for example using service request-response patterns.

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
subgraph COM[Commission]
    DD[Digital
    directory]@{shape: cyl}
end
subgraph TSP[Qualified trust 
service provider]
    IPS[Identity
    proofing
    service]@{shape: rounded}
    QES[QESeal
    creation
    service]@{shape: rounded}
    QTS[QTS
    creation
    service]@{shape: rounded}
    ECS[Evidence
    creation
    service]@{shape: rounded}
    ERDS[QERDS]@{shape: rounded}
    Event[Delivery
    event]@{shape: diamond}
end
subgraph TLSO[TL scheme operator]
    TL["Trusted
    list (TL)"]@{shape: doc}
end
subgraph Sub[Subscriber]
    Wallet[Wallet]
end
subgraph RP[Relying party]
    ERDSVal[Evidence
    validation
    system]@{shape: rounded}
end

DD -------|2\. Identifier discovery| Wallet
DD ---|3\. Service discovery| ERDS
IPS ---|4\. Identity verification| Wallet
IPS ---|5\. Identity proofing| ECS
Event ---|6\. Message delivery| ECS
QES ---|7\. Seal creation| ECS
QTS ---|8\. Time stamp creation| ECS
ECS ---|9\. Evidence creation| ERDS
ERDS ---|10\. Data transmission| Wallet
ERDS ---|11\. Evidence transmission| Wallet
ERDS ---|1\. QTSP registration| TL
ERDS ---|12\. Evidence validation| ERDSVal
```

```mermaid
---
title: Figure 2. Deployment model with data transmission
config:
  htmlLabels: false
  markdownAutoWrap: true
  flowchart:
    #defaultRenderer: "elk"
---
flowchart LR
subgraph Sender
    S-Wallet[Wallet]
end
subgraph S-TSP[Sender qualified trust
service provider]
    S-ERDS[QERDS]
    S-SMP[Service
    metadata]@{shape: docs}
end
subgraph COM[Commission]
    DD[Digital
    directory]@{shape: cyl}
end
subgraph R-TSP[Recipient qualified
trust service provider]
    R-ERDS[QERDS]
    R-SMP[Service
    metadata]@{shape: docs}
end
subgraph Recipient
    R-Wallet[Wallet]
end

S-Wallet ---|13\. Notification creation| S-ERDS
S-Wallet ---|14\. Data submission| S-ERDS
S-ERDS ---|3\. Service discovery| DD
DD --- R-ERDS
S-ERDS ---|15\. Capability discovery| R-SMP
S-ERDS ---|16\. Message relay| R-ERDS
R-ERDS ---|10\. Data transmission| R-Wallet
R-ERDS ---|11\. Evidence transmission| R-Wallet
```

> [!NOTE]
> The sender wallet either submits data or creates a notification.
> The recipient wallet receives notifications and possibly handed over data, matching the submitted data.
> The sender wallet also receives notifications in the same way that the recipient wallet receives notifications, containing evidence of the transmission.

### Data flows and interactions

#### Protocol profiles

*Table 1. Protocol profiles and QTSP roles*

|Interface|Protocol|QTSP role|
|--|--|--|
|1\. QTSP registration|||
|2\. Identifier discovery|||
|3\. Service discovery|||
|4\. Identity verification|[WBCS 2: Credential Presentation](https://github.com/webuild-consortium/wp4-architecture/blob/main/conformance-specs/cs-02-credential-presentation.md)|Verifier|
|5\. Identity proofing|N/A|N/A|
|6\. Message delivery|N/A|N/A|
|7\. Seal creation|||
|8\. Time stamp creation|||
|9\. Evidence creation|N/A|N/A|
|10\. Data transmission|||
|11\. Evidence transmission|||
|12\. Evidence validation|||
|13\. Notification creation|||
|14\. Data submission|||
|15\. Capability discovery|||
|16\. Message relay|||

### Example use case scenarios

#### Data submission between business wallets

```mermaid
---
title: Figure 3. Data submission between business wallets
config:
  htmlLabels: false
  markdownAutoWrap: true
  flowchart:
    #defaultRenderer: "elk"
---
flowchart LR
subgraph Sender[Sender's EBW]
    msg[Document to be 
    delivered]@{shape: docs}
    evidenceSender[Evidence of 
   document sent
    ]@{shape: docs}
end
subgraph Receiver[Receiver's EBW]
    evidenceReceiver[Evidence of 
    received 
    document 
    ]@{shape: docs}
    documentReceiver[Consigned document 
    ]@{shape: docs}
end
subgraph SenderQTSP[Sender's Qualified Trust 
Service Provider]
    QeRDSServiceSender[QeRDS service]@{shape: rounded}
end
subgraph ReceiverQTSP[Receiver's Qualified 
Trust Service Provider]
    QeRDSServiceReceiver[QeRDS service]@{shape: rounded}
end
subgraph EUD[European Digital Directory]
discoveryInfo[ Receiver EBW capabilities, endpoints, 
    etc.]@{shape: cyl}
end
Sender ---|1\. Perform identification and authentication of sender| QeRDSServiceSender
msg ---|2\. Sends document to the QeRDS service used by the sender EBW| QeRDSServiceSender
SenderQTSP ---|3\. Performs discovery about the receiver EBW QeRDS Service| EUD
QeRDSServiceSender ---|4\. Performs handshake with receiver's EBW QeRDS| QeRDSServiceReceiver
QeRDSServiceSender ---|5\. Relays the document to the receiver's EBW QeRDS| QeRDSServiceReceiver
QeRDSServiceReceiver ---|6\. Notify for acceptance of the document reception| Receiver
Receiver ---|7\. Perform identification and authentication of receiver| QeRDSServiceReceiver
QeRDSServiceReceiver ---|8\. Consignment and handover of the document| evidenceReceiver
QeRDSServiceReceiver ---|9. Notify successful consignment and handover of the document| QeRDSServiceSender
```

## Deviations from European Business Wallets

In the WE BUILD pre-production environment, some European Business Wallet roles are simulated:

*Table 2. Role deviations within WE BUILD*

|Role|WE BUILD group|
|--|--|
|Commission|WP4 Trust Registry Infrastructure|
|TL scheme operator|WP4 Trust Registry Infrastructure|
