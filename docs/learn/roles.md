---
render_macros: true
---

# Roles

A role describes what an organisation or a person does in the dataspace. One organisation often holds several roles: a school can provide data in one use case and consume a service in another.

## Participant roles

| Role | What it does | What it publishes in the catalogue |
| --- | --- | --- |
| **Data provider** | Makes data available to other participants | Offers containing *data resources* |
| **Service provider** (data consumer) | Offers a service that uses data, such as an application, an API or an analytics tool. It receives data from providers | Offers containing *software resources* |
| **Infrastructure provider** | Offers a processing service that other participants can insert in a [service chain](service-chains.md), such as anonymisation or edge computing | *Infrastructure offers* |
| **Orchestrator** | Creates and governs a use case: invites members, sets roles and rules, builds service chains, signs the use-case contract | Use cases (also called projects or ecosystems) |

!!! note "Consumer or service provider?"
    In Prometheus-X, the participant that receives data is described by the *service* it uses the data for. This service is the **purpose** of the exchange. That is why the consumer side is called a service provider, and why its offer contains software resources.

## Other roles

| Role | What it does |
| --- | --- |
| **Individual** (end user) | A person whose personal data is exchanged. They read privacy notices and give or withdraw consent through a personal data intermediary. |
| **Personal data intermediary** | The interface where individuals manage their consents. In Prometheus-X it is provided by the Consent Manager and embedded in participants' applications. |
| **Operator** | Runs the core services of the dataspace. Today this is {{ operator.name }}. |
| **Governance authority** | Defines the rules of a dataspace or a sector. |

## Reference roles in the Catalog Registry

The [Catalog Registry](../components/catalog-registry.md) publishes a list of reference roles that use cases can use, each linked to the responsibilities that come with it. Among them:

- data provider for organisational data;
- data provider for personal data;
- service provider for organisations;
- service provider for individuals;
- orchestrator;
- governance authority;
- personal data intermediary;
- end user.

An orchestrator assigns these roles to the members of a use case. The responsibilities linked to a role become obligations in the use-case contract.
