# QERDS interoperability framework requirements

## Introduction

Just like the European Business Wallet (EBW) ecosystem, WE BUILD requires an interoperability framework to [Deliver business wallet data using QERDS](https://github.com/webuild-consortium/wp4-architecture/blob/main/adr/build-qerds.md). This includes the creation of [QERDS protocol profiles](architecture.md#protocol-profiles), in particular for interfaces:

- *10. Data transmission* of user messages (notifications or other data) between the QERDS provider and the wallet owner, for example represented by a wallet user or a business application.
- *11. Evidence transmission* from the QERDS provider and the business wallet, to enable the provision of a common dashboard for the wallet owner to access, store and verify communications.
- *16. Message relay* between interoperable QERDS providers.

While compatibility with additional interfaces is required, for example to discover services and capabilities, the current document assumes that these can be designed as orthogonal concerns.

Several options exist for realising a framework with these these protocol profiles. The leading candidate is based on [AS4 2.0](#edelivery-as4) but several other options are possible. Below is a short list of the most promising candidates. The list was selected based on the following criteria:

1. Must be a message protocol that can be adapted to the 4 corner model envisioned for QERDS.
2. Must support end-to-end message encryption, independent from the transport layer (TLS).
3. Must be able to support crypto algorithm agility including a clear path to post-quantum algorithm support before 2035.
4. Must be able to support service and capability discovery using a protocol-independent EU Digital Directory.
5. Must support any transmitted data and evidence formats, for example using Media Types (IETF RFC 2046).

For the end-to-end message encryption, the note regarding [QERDS protocol profiles](architecture.md#protocol-profiles) applies: messages can be encrypted either between provider access points or between subscriber access points.
This does not need to affect the applicability of profiles to the interfaces, but may affect what combinations may be suitable in an interoperability framework.

## Starting point for WE BUILD

### eDelivery AS4

The [eDelivery AS4 specifications](https://ec.europa.eu/digital-building-blocks/sites/spaces/DIGITAL/pages/467117619/AS4+Access+Point+specifications) are applied in various EU-initiated networks for *electronic data interchange over the internet with internet integration* ([EDIINT](https://datatracker.ietf.org/wg/ediint/about/)).
These are based on *applicability statement 4* ([AS4](https://docs.oasis-open.org/ebxml-msg/ebms/v3.0/profiles/AS4-profile/v1.0/AS4-profile-v1.0.html)) as specified by OASIS and standardised internationally as [ISO 15000-2](https://www.iso.org/standard/79109.html).
The [EN 319 522-4-1](https://www.etsi.org/deliver/etsi_en/319500_319599/3195220401/01.02.01_60/en_3195220401v010201p.pdf) European standard provides a binding of AS4 to the *16. Message relay* interface.

For WE BUILD, [WP4 has decided](https://github.com/webuild-consortium/wp4-architecture/blob/main/adr/build-qerds.md) to start with eDelivery AS4 for *16. Message relay*.
Whether to use version 1 (widely deployed, strong open source support) or version 2 (newer, with more modern cryptography) still needs to be decided.

For *10. Data transmission* and *11. Evidence transmission*, eDelivery AS4 may be applied as well.
In that case, the business wallet or other wallet owner software needs to implement an eDelivery AS4 access point to communicate with the QERDS provider.
However, alternative protocols can be considered as well.

## Promising candidates for EBW

### JMAP

The JMAP [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620.html) framework could be applied to interfaces *10. Data transmission* and *11. Evidence transmission*.
While this has previously been done for email, similar application patterns are applicable to the QERDS.

### Matrix

[Matrix](https://matrix.org/) is a protocol designed for real time messaging. It is JSON-based and runs on top of HTTP transport. Matrix uses *message level security* (MLS, [RFC 9420](https://datatracker.ietf.org/doc/rfc9420/)) to provide group messaging with perfect forward secrecy.

### ActivityPub

[ActivityPub](https://www.w3.org/TR/activitypub/) is a protocol for event-based messaging over HTTP transport. The protocol was initially designed to be used for “social media” event streams but is in fact a general event message protocol. The protocol uses JSON-LD framing. Work on integrating MLS in ActivityPub is [at the Social Web Incubator Community Group](https://github.com/swicg/activitypub-e2ee) which adds group messaging to ActivityPub.

While ActivityPub may be suitable for *16. Message relay* between QERDS providers, another protocol such as [JMAP](#jmap) is required for *10. Data transmission* and *11. Evidence transmission*.

### DIDComm Messaging

[DIDComm Messaging v2.1](https://identity.foundation/didcomm-messaging/spec/v2.1/) is a message-oriented protocol over multiple transports (HTTP, WebSockets, etc.) that is used in the W3C credential ecosystem. DIDComm doesn’t have a group-keying solution but MLS would be a possibility.

Applying its mediator role, DIDComm Messaging might support the four-corner model and provide protocols for *16. Message relay* as well as *10. Data transmission* and *11. Evidence transmission*.

## Candidates disregarded by WE BUILD

- Email, e.g. AS2 ([RFC 4130](https://www.rfc-editor.org/rfc/rfc4130.html)): provides no clear benefits over [eDelivery AS4](#edelivery-as4).
- XMPP: faces the same XML-based protocol challenges as [eDelivery AS4](#edelivery-as4) without clear benefits.

## Discussion

All of the promising candidates are relatively mature protocols and each of them arguably have a larger deployed base than AS4 has today, when disregarding public and private business-to-business and business-to-government interchange. In the case of Matrix, the deployed base is very large and ActivityPub (without the MLS extension) is even more widely deployed. The DIDComm protocol is perhaps the choice that is closest to AS4 in structure even if DIDComm Messaging v2.1 has a somewhat different key management mechanism at its core. All of these would be easy to adapt to the current EU trust directory (e.g. the one used in Peppol).

The existence of a group-based encryption mechanism means that flows that are not just 1–1 – i.e. “mailing list”-type flows with perfect forward secrecy becomes possible. For the EBW this could be important for use cases where more than two parties need to exchange messages, for instance when an auditor is invited to monitor a message exchange betwen a government agency and a legal entity. Within WE BUILD, no use case is known yet which could validate this assumption.

# Appendix A: DIDComm/ERDS gap analysis

**[Full document →](https://docs.google.com/document/d/1y6e9bI4kVCKQwbdgbNLdu53exWAoG0CQ/edit)**
*Architectural Compatibility, 4-Corner Model Mapping, and Feasibility of Profiling DIDComm 2.1 as a Post-Quantum ERDS | April 2026*

---

## Central Finding

Both ETSI EN 319 522-1 (ERDS) and DIDComm Messaging v2.1 independently arrive at a **4-corner architectural model** for trusted message delivery. They are structurally isomorphic at the level of roles, routing, and evidence of delivery. A DIDComm-based ERDS profile is **technically feasible** and offers meaningful advantages over the current AS4/ebXML approach.

## Architecture Mapping

| ERDS Concept | DIDComm 2.1 Equivalent |
|---|---|
| Sender / ERD-UA | Sender agent (Alice) |
| Sender's ERDS (S-ERDS) | Sender's Mediator(s) |
| ERDS Relay Interface | Forward message / mediator routing |
| Recipient's ERDS (R-ERDS) | Recipient's Mediator(s) |
| Recipient / ERD-UA | Recipient agent (Bob) |
| Common Service Interface (CSI) | DID resolution infrastructure |
| Trust Status List (TSL) | Trusted DID method / trust registry |
| ERDS Evidence | *(No direct equivalent — must be profiled)* |

## Strengths of DIDComm as an ERDS Foundation

- **Post-quantum ready:** Native JSON/JOSE cryptography enables clean migration to ML-DSA and ML-KEM algorithms.
- **Algorithm-agile identity:** DID Documents support flexible key management without protocol changes.
- **Transport independence:** Runs over HTTPS, WebSockets, Bluetooth, SMTP, and others without protocol-level changes.
- **Decentralized trust:** Reduces dependence on centralized AS4 connectivity networks; DID Document updates propagate routing changes automatically.
- **Store-and-forward:** DIDComm mediator semantics match ERDS consignment and asynchronous delivery models natively.

## Key Gaps Requiring Profiling

1. **ERDS Evidence Protocol** *(most critical)*: DIDComm has no equivalent to ERDS's 17 legally-binding evidence event types (covering submission, relay, consignment, and handover). A purpose-built DIDComm evidence protocol must be defined, covering mandatory events: `SubmissionAcceptance`, `RelayAcceptance`, `ContentConsignment`, `ContentHandover`, and their failure variants.

2. **Legal Identity Binding:** DIDs are cryptographic identifiers by default. The profile must specify acceptable DID methods that bind to verified legal identity (candidates: `did:ebsi`, `did:elsi`, eIDAS-anchored methods, or Verifiable Credentials).

3. **Trusted Timestamping:** RFC 3161 timestamp tokens must be embedded in evidence messages; no built-in mechanism exists in DIDComm.

4. **Capability Negotiation:** ERDS requires a structured handshake covering supported formats, evidence types, and authentication strength; DIDComm offers only basic service endpoint metadata.

## Conclusion

DIDComm 2.1 provides a strong cryptographic and transport foundation that maps naturally onto ERDS requirements. The primary work for a conformant profile is defining the **evidence protocol** — the single most important deliverable of any profiling effort. With this protocol in place, DIDComm offers a modern, post-quantum-ready path forward for registered electronic delivery.
