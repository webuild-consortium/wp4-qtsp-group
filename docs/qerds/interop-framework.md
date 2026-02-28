# QERDS interoperability framework requirements

## Introduction

Just like the European Business Wallet (EBW) ecosystem, WE BUILD requires an interoperability framework to [Deliver business wallet data using QERDS](https://github.com/webuild-consortium/wp4-architecture/blob/main/adr/build-qerds.md). This includes the creation of [QERDS protocol profiles](architecture.md#protocol-profiles), in particular for interfaces:

- *10. Data transmission* of user messages (notifications or other data) between the QERDS provider and the wallet owner, for example represented by a wallet user or a business application.
- *11. Evidence transmission* from the QERDS provider and the business wallet, to enable the provision of a common dashboard for the wallet owner to access, store and verify communications.
- *16. Message relay* between interoperable QERDS providers.

While compatibility with additional interfaces is required, for example to discover services and capabilities, the current document assumes that these can be designed as orthogonal concerns.

Several options exist for realising a framework with these these protocol profiles. The leading candidate is based on [AS4 2.0](#edelivery-as4) but several other options are possible. Below is a short list of the most promising candidates. The list was selected based on the following criteria:

1. must be a message protocol that can be adapted to the 4 corner model envisioned for QERDS.
2. must support end2end message encryption independent from the transport layer (TLS)
3.  must be able to support crypto algorithm agility including a clear path to post-quantum algorithm support before 2035.
4.  must be able to support service and capability discovery using a protocol-independent EU Digital Directory
5. must support any transmitted data and evidence formats, for example using Media Types (IETF RFC 2046)

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

Matrix is a protocol designed for real time messaging: https://matrix.org/ Matrix is Json based and runs on top of http transport. Matrix uses message level security, RFC9420 (https://datatracker.ietf.org/doc/rfc9420/) aka MLS to provide group messaging with perfect forward secrecy.

### MLS over ActivityPUB 

ActivityPub https://www.w3.org/TR/activitypub/ is a protocol for event-based messaging over http transport. The protocol was initially designed to be used for "social media" event streams but is in fact a general event message protocol. The protocol uses JSON-LD framing. Work on integrating MLS in ActivityPub is at https://github.com/swicg/activitypub-e2ee which adds group-messaging to ActivityPub.

### didcomm2.1

DIDcomm2.1 is a message-oriented protocol over multiple transports (http, websockets etc) that is used in the W3C credential ecosystem. DIDcomm doesn't have a group-keying solution but MLS would be a possibility.

All of these are relatively mature protocols and each of them arguably have a larger deployed base than AS4 has today. In the case of matrix the deployed base is very large and activitypub (without the MLS extension) is even more widely deployed. The didcomm protocol is perhaps the choice that is closest to AS4 in structure even if didcomm2 has a somwehat different key management mechanism at its core. All of these would be easy to adapt to the current EU trust directory (eg the one used in PEPPOL).

The existence of a group-based encryption mechanism means that flows that are not just 1-1 - ie "mailing list"-type flows with perfect forward secrecy becomes possible. For the EUBW this could be important for use-cases where more than 2 parties need to exchange messages, for instance when an auditor is invited to monitor a message exchange betwen a government agency and a legal entity.

## Candidates disregarded by WE BUILD

- XMPP: TODO
