# Roadmap

This page lists the main changes under way in the Prometheus-X core. Items here are plans, not commitments: their scope and timing can change.

## Decentralised identity for participants

**Today**, the operator's catalogue holds each organisation's identity and issues the API keys its connector uses. This works within one dataspace, but an organisation onboarded by one operator is unknown to any other, and it cannot prove who it is outside Prometheus-X without going through that operator. See [Identity and trust](learn/identity-and-trust.md).

**Target.** Each participant gets a wallet holding verifiable credentials:

- Gaia-X credentials (Legal Person, Legal Registration Number, Terms and Conditions, and the resulting Gaia-X Compliance Credential);
- a revocable Prometheus-X membership credential.

Any Prometheus-X service, any Prometheus-X operator and any Gaia-X-aligned ecosystem can then check these credentials against public trust anchors, without calling the operator that onboarded the participant. Connectors will verify each other through these credentials before an exchange.

Prometheus-X is working on this with Orange, based on the Orange Live Identity Wallet, using open standards: W3C Verifiable Credentials, OpenID for Verifiable Credential Issuance and Presentation (OID4VCI, OID4VP) and `did:web`.

What does **not** change: contracts, policies and consent still decide what a participant may do. The wallet answers "who is this participant, and who vouches for it?".

## Connector: separate control plane and data plane

The connector is being split into a control plane (authorisation, contract and policy checks, consent) and a data plane (moving the bytes). The first step is the **control-plane-only mode** in PDC 1.12.0, which lets participants' own systems transfer the data once the connectors have authorised the exchange. See [Exchange flows](connector/exchange-flows.md#control-plane-only-mode).

The identity handshake between connectors will live in this control plane.

## Interoperability with other dataspaces

Prometheus-X leads the [Data Space Interoperability Framework (DSIF)](interoperability/dsif.md), together with other dataspace initiatives. Its strands progress at different speeds:

| Strand | Goal | Status |
| --- | --- | --- |
| Catalogue federation | Discover offers across dataspaces through DCAT-AP catalogues | Most mature, proof of concept running |
| Contract federation | Multi-party and intermediated negotiation, and an information model describing the minimal set of actions required to reach protocol interoperability | Specification in progress |
| Consent federation | Check and relay consent between personal data intermediaries | Specification and pilot in progress |
| Identity and trust | A common assertion format based on verifiable credentials | Proposal under review |
| Connector and service chains | Cross-dataspace exchanges and processing chains | Early work |

## Building blocks

Partners are documenting the building blocks that complement the core (veracity, edge computing, agents and more) on this site. See [Building blocks](building-blocks/index.md).
