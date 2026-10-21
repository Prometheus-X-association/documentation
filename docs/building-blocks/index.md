---
render_macros: true
---

# Building blocks

Prometheus-X is made of **building blocks**: components that each provide one capability of the dataspace. The **core** building blocks are needed to run a dataspace at all. The others add capabilities that use cases can choose to rely on.

!!! info "A growing list"
    The building blocks below are a selection, not an exhaustive list. Their maintainers are adding their documentation to this site, and more building blocks will join over time.

## Core

Documented in [Core components](../components/index.md) and [Connector](../connector/index.md).

| Building block | Capability |
| --- | --- |
| [Catalogue](../components/catalogue.md) | Onboarding, offers, discovery, use cases, negotiation |
| [Catalog Registry](../components/catalog-registry.md) | Shared reference models and policy templates |
| [Contract Manager](../components/contract-manager.md) | Multi-party and bilateral contracts |
| [Consent Manager](../components/consent-manager.md) | Privacy notices, consent, individuals' identity |
| [Dataspace Connector](../connector/index.md) | Contract, policy and consent enforcement; data exchange |
| [Data Processing Chain Protocol]({{ ptx_github }}/data-processing-chain-protocol) | Service chains, built into the connector ([concepts](../learn/service-chains.md)) |
| [Contract and consent negotiating agent]({{ ptx_github }}/contract-consent-agent) | Rule-based recommendations and automated negotiation, based on participants' and individuals' preferences |

## Trustworthy data sharing

| Building block | Capability | Documentation |
| --- | --- | --- |
| [Decentralized AI training]({{ ptx_github }}/decentralized-ai-training) | Train machine learning models by federated learning, without gathering the data in one place | Coming soon |
| [Edge computing: AI processing]({{ ptx_github }}/edge-computing) | Privacy-preserving processing close to the data, usable as a service chain step | Coming soon |
| Trustworthy AI: algorithm assessment | A toolbox of three complementary tools to assess the trustworthiness of AI algorithms: AffectLog's ALT-AI, LORIA's LOLA and the University of Koblenz's CARiSMA | Coming soon |

## Data transformation

| Building block | Capability | Documentation |
| --- | --- | --- |
| [Data Alignment, Aggregation and Vectorisation (DAAV)]({{ ptx_github }}/daav) | Visual workflows to align, aggregate and vectorise data from several sources and formats | Coming soon |
| [Edge translators]({{ ptx_github }}/edge-translators) | Translate jobs, skills and qualifications data between frameworks and formats, such as national frameworks and ESCO | Coming soon |
| [Learning Object Metadata Crowd Tagging (LOMCT)]({{ ptx_github }}/lomct) | Let users enrich the metadata of learning resources | Coming soon |
| [Learning Records Converter]({{ ptx_github }}/learning-records-converter) | Convert learning records from various formats (SCORM, IMS Caliper, cmi5, proprietary) to xAPI | Coming soon |
| [Learning traces from VR activities]({{ ptx_github }}/learning-traces) | Produce xAPI learning traces from virtual reality training activities | Coming soon |
| Personal Learning Record Store (PLRS) | Let individuals keep and control their own learning records | Coming soon |
| [Web Analytics Learning Records Universal Connector (WALRUC)]({{ ptx_github }}/walruc) | Turn web analytics data into xAPI learning records | Coming soon |

## Utility

| Building block | Capability | Documentation |
| --- | --- | --- |
| [Data value chain tracker]({{ ptx_github }}/data-value-chain-tracker) | Track each participant's contribution along a data value chain, as a basis for sharing the value created | Coming soon |
| [Data veracity assurance]({{ ptx_github }}/data-veracity) | Veracity level agreements in contracts, and verifiable attestations that exchanged data meets them | Coming soon |
| [Distributed data visualisation]({{ ptx_github }}/distributed-datavis) | Reusable on-device components to show analytics, predictions and recommendations to end users | Coming soon |
| [Dataspace interoperability]({{ ptx_github }}/dataspace-interoperability) | Conversion between DSP / DCAT catalogue offers and Prometheus-X offers ([Simpl](../interoperability/simpl.md) first) | Coming soon |
| [Dataspace assistant]({{ ptx_github }}/dataspace-assistant) | Natural-language search and reports over the catalogue | Coming soon |

## How building blocks connect to the core

Most building blocks plug into the core in one of these ways:

| Integration point | Examples |
| --- | --- |
| **As a step in a service chain**, behind a connector, published as an infrastructure offer | Edge computing, anonymisation |
| **Inside a core service**, as a library or module | Contract and consent negotiating agent in the Contract Manager and Consent Manager |
| **Alongside the connector**, adding checks or records to exchanges | Data veracity assurance |
| **On top of the catalogue**, reading or feeding offers | Dataspace assistant, dataspace interoperability |
| **Through connector hooks** | Billing service (`billingUri`), data value chain tracking |

## Building block documentation

Each building block will get its own section here, written by its maintainers with a common structure: overview, integration with the core, getting started, reference.
