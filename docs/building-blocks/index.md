---
render_macros: true
---

# Building blocks

Prometheus-X is made of **building blocks**: components that each provide one capability of the dataspace. The **core** building blocks are needed to run a dataspace at all. The others add capabilities that use cases can choose to rely on.

## Core building blocks

Documented in [Core components](../components/index.md) and [Connector](../connector/index.md).

| Building block | Capability |
| --- | --- |
| [Catalogue](../components/catalogue.md) | Onboarding, offers, discovery, use cases, negotiation |
| [Catalog Registry](../components/catalog-registry.md) | Shared reference models and policy templates |
| [Contract Manager](../components/contract-manager.md) | Multi-party and bilateral contracts |
| [Consent Manager](../components/consent-manager.md) | Privacy notices, consent, individuals' identity |
| [Dataspace Connector](../connector/index.md) | Contract, policy and consent enforcement; data exchange |
| [Data Processing Chain Protocol]({{ ptx_github }}/data-processing-chain-protocol) | Service chains, built into the connector ([concepts](../learn/service-chains.md)) |

## Other building blocks

| Building block | Capability | Maintainer | Repository | Documentation |
| --- | --- | --- | --- | --- |
| Contract and consent agent | Rule-based recommendations and automated negotiation, based on participants' and individuals' preferences | Visions | [contract-consent-agent]({{ ptx_github }}/contract-consent-agent) | Coming soon |
| Data veracity assurance | Veracity level agreements in contracts, and verifiable attestations that exchanged data meets them | BME | [data-veracity]({{ ptx_github }}/data-veracity) | Coming soon |
| Edge computing | Privacy-preserving processing close to the data, usable as a service chain step | BME | [edge-computing]({{ ptx_github }}/edge-computing) | Coming soon |
| Dataspace interoperability | Conversion between DSP / DCAT catalogue offers and Prometheus-X offers ([Simpl](../interoperability/simpl.md) first) | Visions | [dataspace-interoperability]({{ ptx_github }}/dataspace-interoperability) | Coming soon |
| Dataspace assistant | Natural-language search and reports over the catalogue | Visions | [dataspace-assistant]({{ ptx_github }}/dataspace-assistant) | Coming soon |

## How building blocks connect to the core

Most building blocks plug into the core in one of these ways:

| Integration point | Examples |
| --- | --- |
| **As a step in a service chain**, behind a connector, published as an infrastructure offer | Edge computing, anonymisation |
| **Inside a core service**, as a library or module | Contract and consent agent in the Contract Manager and Consent Manager |
| **Alongside the connector**, adding checks or records to exchanges | Data veracity assurance |
| **On top of the catalogue**, reading or feeding offers | Dataspace assistant, dataspace interoperability |
| **Through connector hooks** | Billing service (`billingUri`), data value chain tracking |

## Building block documentation

Each building block will get its own section here, written by its maintainers with a common structure: overview, integration with the core, getting started, reference.
