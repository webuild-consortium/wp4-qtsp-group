# QERDS interoperability framework

Context:

- [QERDS protocol profiles](architecture.md#protocol-profiles), in particular interfaces:
    - 10. Data transmission
    - 11. Evidence transmission
    - 16. Message relay
- Architecture decision record: [Deliver business wallet data using QERDS](https://github.com/webuild-consortium/wp4-architecture/blob/main/adr/build-qerds.md)

## Introduction

Several options exist for realizing the QERDS. The leading candidate is [AS4 2.0](#edelivery-as4) but several other options are possible. Below is a short list of the most promising candidates. The list was selected based on the following criteria:

1. must be a message protocol that can be adapted to the 4 corner model envisioned for QERDS.
2. must support end2end message encryption independent from the transport layer (TLS)
3.  must be able to support crypto algorithm agility including a clear path to post-quantum algorithm support before 2035.
4.  must be able to support lookup-based trust using the existing EU trust directories.



## Starting point for WE BUILD

### eDelivery AS4

TODO

## Promising candidates for EBW

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
