---
render_macros: true
---

# Glossary

Dataspace vocabulary varies between projects, and the same word can mean different things. These are the meanings used across this documentation.

| Term | Meaning |
| --- | --- |
| **API consumption** | A flow in which a data provider sends its data to a service provider's API and receives the API's response back. |
| **Bilateral contract** | A contract between one data provider and one consumer, covering one offer. |
| **Building block** | A component of the Prometheus-X stack. *Core* building blocks are required to run a dataspace; others add capabilities. See [Building blocks](../building-blocks/index.md). |
| **Catalogue** | The core service where participants register, publish offers, discover each other and build use cases. |
| **Catalog Registry** | The core service that publishes shared reference models: policy templates, roles, categories. |
| **Consent** | An individual's agreement, recorded by the Consent Manager, to have their personal data exchanged under a given privacy notice. |
| **Consent Manager** | The core service that generates privacy notices, records consents and starts consent-based exchanges. |
| **Contract Manager** | The core service that stores signed contracts and their policies. |
| **Contribution** | An orchestrator's own offer, added to its own use case. |
| **Control-plane-only mode** | A connector mode in which the connectors authorise the exchange and the participants' own systems transfer the data. |
| **Credential** (connector) | A secret, such as an API key or a password, stored in the connector and used to reach the participant's own systems. Referenced by its identifier in the catalogue. |
| **Credential** (verifiable) | A signed statement about a participant, held in a wallet. Part of the identity roadmap. |
| **Data resource** | A description of a unit of data: a dataset, a database, an API's output. |
| **Dataspace** | An environment where organisations exchange data under shared rules, with trust and control. |
| **Data exchange** | One transfer of data between connectors, recorded by each connector involved with its status. |
| **DPCP** | Data Processing Chain Protocol. The library inside the connector that runs service chains. |
| **DSIF** | Data Space Interoperability Framework. A cross-dataspace interoperability initiative led by Prometheus-X. |
| **Ecosystem** | See *use case*. |
| **Ecosystem contract** | A multi-party contract binding all members of a use case. |
| **Exchange trigger** | A way to start an exchange from the operator's interface, without writing code. |
| **Individual** | A person whose personal data is exchanged. Also called end user. |
| **Infrastructure offer** | An offer for a processing service that can be a step in a service chain. |
| **Infrastructure provider** | A participant that publishes infrastructure offers. |
| **Invitation** | An orchestrator's invitation for a participant to join a use case. |
| **Join request** | A participant's request to join a use case. |
| **Negotiation** | The exchange of proposals on offers, policies and pricing before signing. |
| **ODRL** | Open Digital Rights Language, the W3C policy language used for policies. |
| **Offer** | A package of one or more resources with business information and policies, visible in the catalogue. Also called service offering. |
| **Operator** | The organisation that runs the core services of a dataspace. |
| **Orchestrator** | The participant that creates and governs a use case. |
| **Participant** | An organisation registered in the dataspace. |
| **PDC** | Prometheus-X Dataspace Connector. The software each participant uses to take part in exchanges. |
| **PDI** | Personal Data Intermediary. The interface where individuals manage their consents. |
| **Policy** | A rule on how an offer may be used, written in ODRL. |
| **Policy injection** | The step where the catalogue writes the negotiated policies into the signed contract. |
| **Policy template** | A reusable, parameterised policy published by the Catalog Registry. |
| **Privacy notice** | The description of an exchange shown to an individual before they consent, generated from the contract. |
| **Project** | See *use case*. |
| **Purpose** | The service for which a consumer receives data. Represented by the consumer's software resource. |
| **Representation** | The technical part of a resource: how the connector reaches it (URL, source type, format, credential identifier). |
| **Self-description** | The public description of a participant, offer, resource or use case, available at its URL. |
| **Service chain** | A sequence of processing services that data flows through, under one use-case contract. |
| **Service provider** | A participant that offers a service using data; the consumer side of an exchange. |
| **Software resource** | A description of a service, application or API that uses data. |
| **Tech Space** | {{ operator.name }}'s sandbox to test a connector against reference connectors. |
| **Use case** | A group of participants working towards a shared goal under one contract, set up by an orchestrator. Also called project or ecosystem. |
