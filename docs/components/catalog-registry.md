---
render_macros: true
---

# Catalog Registry

The Catalog Registry publishes the **reference models** of a dataspace: the shared vocabularies that the catalogue and the Contract Manager rely on, such as policy templates, roles and categories.

| | |
| --- | --- |
| **Repository** | [catalog-registry]({{ ptx_github }}/catalog-registry) |
| **Source of the models** | The `reference-models` repository of the Prometheus-X organisation, synchronised on a schedule |
| **Used by** | The catalogue (to offer choices to users) and the Contract Manager (to build policies) |

## What it publishes

| Reference type | Content |
| --- | --- |
| **Rules** | Policy templates in ODRL form. Three families: access rules, billing rules and responsibility rules. |
| **Roles** | Reference roles (data provider for personal data, orchestrator, personal data intermediary...), each linked to its responsibility rules. |
| **Building blocks** | Descriptions of the Prometheus-X building blocks that use cases can require. |
| **Data categories** and **service categories** | Used to classify offers and match them with use cases. |
| **Pricing models**, **business models**, **value sharing** | Used to describe the economics of offers and use cases. |
| **Architecture**, **decision process**, **direction**, **perimeter**, **requirement** | Governance descriptions for a dataspace instance. |

## Policy templates

A template is a policy with placeholders, plus the list of fields to fill in.

```json
{
  "uid": "rule-access-4",
  "name": "Count",
  "description": [{ "@value": "MUST not use data for more than n times", "@language": "en" }],
  "policy": {
    "permission": [{
      "action": "use",
      "target": "@{target}",
      "constraint": [{ "leftOperand": "count", "operator": "lt", "rightOperand": "@{value}" }]
    }]
  },
  "requestedFields": ["target", "value"]
}
```

When participants agree on a policy, the catalogue sends the template identifier and the values (`{"ruleId": "rule-access-4", "values": {"value": 10}}`). The Contract Manager fetches the template, replaces each `@{field}` and stores the resulting policy in the contract.

| Family | Examples |
| --- | --- |
| `rule-access-*` | No restriction, time interval, count, notification |
| `rule-billing-*` | Subscription expiry, compensation payment, usage count |
| `rule-responsibility-*` | Obligations attached to roles in a use case |

## Customising an instance

An operator can override the data and service categories of its dataspace in the registry's `instance.config.json`, and add its own reference entries through the API.

## API

| Route | Purpose |
| --- | --- |
| `GET /v1/references` | List reference types |
| `GET /v1/references/{type}` | List the entries of a type |
| `GET /v1/references/{type}/{fileName}` | Get one entry |
| `GET /static/references/{type}/{file}.json` | Static JSON-LD file of an entry |

Static files are what the Contract Manager reads when it builds policies. The registry's JSON-LD is intentionally light: it is a reference registry, not an ontology server.
