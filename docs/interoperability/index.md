# Interoperability

Prometheus-X is one of several dataspace stacks in Europe. This section explains how it relates to the others, where it follows common standards, where it deliberately differs, and how cross-dataspace interoperability is being built.

## Prometheus-X in the landscape

| Initiative | What it is | Relation to Prometheus-X |
| --- | --- | --- |
| **Eclipse Dataspace Protocol (DSP)** and **Eclipse Dataspace Components (EDC)** | The most widely used dataspace protocol (catalogue, contract negotiation, transfer) and its reference connector | Prometheus-X uses its own exchange protocol, because DSP does not cover consent, multi-party contracts or service chains. Bilateral contracts can already be negotiated with DSP connectors over the DSP Contract Negotiation Protocol, as piloted in DSIF. [Details](dsp-and-edc.md) |
| **Gaia-X** | Trust framework and compliance services for data and cloud ecosystems | The Prometheus-X data model follows Gaia-X naming. Gaia-X compliance credentials are on the identity roadmap. [Details](gaia-x.md) |
| **Simpl** | The European Commission's open-source middleware for common European data spaces, based on DSP and EDC | A middleware brings Simpl catalogue offers into the Prometheus-X catalogue. [Details](simpl.md) |
| **DSIF** | Data Space Interoperability Framework, led by Prometheus-X with other dataspace initiatives | The main vehicle for cross-dataspace federation: catalogue, contract, consent, identity. [Details](dsif.md) |
| **FIWARE** | Open-source data space components and connector | Partner in DSIF; contract mappings with TM Forum APIs are being worked out there |
| **IDSA** | International Data Spaces reference architecture | Prometheus-X follows its principles (connectors, usage control) without its message model |

## The approach

**Use standards where they fit.** ODRL for policies, DCAT for catalogue federation, ISO/IEC TS 27560 for consent records, Gaia-X naming for the data model, W3C Verifiable Credentials and OpenID for Verifiable Credentials for the coming identity layer.

**Extend where they fall short.** Personal data consent, multi-party contracts and service chains are not covered by existing dataspace protocols. Prometheus-X implements them in its own protocol, and proposes them as extensions of common protocols through DSIF. See [Why a dedicated protocol](why-a-dedicated-protocol.md).

**Federate rather than merge.** Each dataspace keeps its own stack. DSIF defines how catalogues, contracts, consents and identities can work across them.

## Integrating with Prometheus-X

From the lightest to the heaviest effort:

| Option | What it involves |
| --- | --- |
| **An application behind a connector** | Your system is a data source or a service behind a Prometheus-X connector. No protocol work: see [Join a dataspace](../participants/index.md). |
| **A processing service in a chain** | Publish an infrastructure offer and run it behind a connector. See [Service chains](../learn/service-chains.md). |
| **Catalogue federation** | Expose or read DCAT-AP catalogues through DSIF, or use the interoperability middleware. See [DSIF](dsif.md#catalogue-federation). |
| **Your own data plane** | Use the connector's control-plane-only mode and move the data with your own transfer system, including another stack's data plane. See [Exchange flows](../connector/exchange-flows.md#control-plane-only-mode). |
| **DSP contract negotiation** | Negotiate bilateral contracts between a Prometheus-X participant and a DSP connector over the DSP Contract Negotiation Protocol. Piloted in DSIF, with trust between the systems configured manually. See [Eclipse DSP and EDC](dsp-and-edc.md#contract-negotiation-over-dsp). |
| **Full protocol bridge** | Multi-party contracts and the rest of the exchange across Prometheus-X and DSP connectors. This is the subject of the DSIF contract and connector strands. |

See [Standards](standards.md) for the detailed status of each standard.
