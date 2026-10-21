---
render_macros: true
---

# Exchange flows

Exchanges vary along two axes: the **contract** (use-case or bilateral) and the **data** (non-personal or personal). On top of these, three variants exist: **API consumption**, **service chains** and **control-plane-only**. This page describes each one.

| Flow | When to use it |
| --- | --- |
| [Business-to-business](#business-to-business-exchange) | Non-personal data from a provider to a consumer's service |
| [API consumption](#api-consumption) | A provider sends its data to a consumer's API and gets the result back |
| [Consent-based](#consent-based-exchange) | Personal data, triggered by the individual's consent |
| [Service chains](#service-chains) | Data goes through processing steps between provider and consumer |
| [Control-plane-only](#control-plane-only-mode) | The connectors authorise, your own systems move the data ({{ pdc_next }}) |

## Triggering an exchange

Any party to the contract can trigger an exchange on its own connector.

```bash
curl -X POST https://pdc.your-organisation.eu/exchange \
  -H "Authorization: Bearer <admin token>" \
  -H "Content-Type: application/json" \
  -d '{
    "contract": "{{ operator.contract_uri }}contracts/<id>",
    "purposeId": "<consumer offer URL>",
    "resourceId": "<provider offer URL>",
    "resources": [{ "resource": "<data resource URL>", "params": { "query": [{ "from": "2026-01-01" }] } }]
  }'
```

| Field | Meaning |
| --- | --- |
| `contract` | URL of the contract. Use-case contracts contain `/contracts/`, bilateral ones `/bilaterals/`. |
| `purposeId` | The consumer's offer. Required for use-case contracts; for bilateral contracts the connector takes it from the contract. |
| `resourceId` | The provider's offer. Same rule as `purposeId`. |
| `resources` | Optional: restrict to some data resources and pass query parameters to each. |
| `serviceChainId` | Optional: run a service chain defined in the contract. |
| `data` | Optional: send this payload instead of fetching it from the provider. |
| `directResponseVisualization` | Optional: return the consumer application's response to the caller (connector 1.11.0+, not for consent flows). |

The call waits until the exchange finishes or times out (`EXCHANGE_TIMEOUT`), then returns the exchange record and its status.

Exchanges can also be started from the operator's interface with **exchange triggers** (see [Run your first exchange](../participants/first-exchange.md)).

## Business-to-business exchange

```mermaid
sequenceDiagram
  autonumber
  participant CA as Consumer application
  participant C as Consumer connector
  participant K as Contract Manager
  participant G as Catalogue
  participant P as Provider connector
  participant PA as Provider API
  CA->>C: POST /exchange
  C->>K: Get the contract
  C->>G: Get offers and resources descriptions
  C->>C: Refuse if personal data (consent flow required)
  C->>P: Create the shared exchange record
  C->>P: Ask for the data
  P->>K: Get the contract, evaluate the offer's policies
  P->>PA: Fetch the data (credentials, query parameters, headers)
  P->>C: Send the data
  C->>CA: Deliver to the software resource URL
  C->>P: Mark the exchange as successful
  C-->>CA: Exchange result
```

The provider's connector always evaluates its own offer's policies before fetching any data. If the provider triggers the exchange, it runs the same steps starting from its side.

## API consumption

In this variant, the provider sends its data to the consumer's API, and the API's response comes back to the provider.

```mermaid
flowchart LR
  PA[Provider data] --> P[Provider connector] --> C[Consumer connector] --> API[Consumer API]
  API --> C --> P --> R[Provider's response endpoint]
```

Set up:

- the provider's data resource is marked as **payload for an API**, with a response representation saying where the result goes;
- the consumer's software resource is marked as an **API**.

## Consent-based exchange

Personal data is exchanged only when the individual consents. The consent starts the exchange.

```mermaid
sequenceDiagram
  autonumber
  actor U as Individual
  participant PA as Provider application
  participant P as Provider connector
  participant CM as Consent Manager
  participant C as Consumer connector
  participant CA as Consumer application
  Note over PA,CM: Signed contract, both participants registered with the Consent Manager, users registered
  PA->>P: GET /private/pdi
  P-->>PA: Consent interface address
  PA->>U: Embed or redirect
  U->>CM: Reads the privacy notice, gives consent
  CM->>P: Signed consent
  P-->>CM: Exchange token
  CM->>C: Signed consent and token
  C->>P: Request the data for this consent
  P->>CM: Validate the consent and token
  P->>P: Evaluate policies
  P->>PA: Fetch the data with the provider's user identifier
  P->>C: Send the data with the consumer's user identifier
  C->>CA: Deliver, with consent headers
```

Points to note:

- If the consumer does not know the person yet, the consent waits while the consumer's connector asks the application (`registrationUri`) to create the account, then resumes.
- Consents can also select one of the service chains of the contract.
- `directResponseVisualization` is not available in consent flows.

## Service chains

A chain is defined by the orchestrator in the use-case contract. Triggering it:

```json
{
  "contract": "{{ operator.contract_uri }}contracts/<id>",
  "serviceChainId": "<chain id from the contract>"
}
```

Then:

1. The triggering connector creates the exchange record on every connector of the chain and starts the chain.
2. The first node (the provider) fetches the data and sends it to the second node.
3. Each intermediate connector checks its service is part of the chain in the contract, re-checks consent if any, and calls its service with `{ data, contract, params, previousNodeParams }`.
4. The service processes the data and calls `POST /service-chain/resume` on its connector, or the connector does it automatically if `serviceChainAdapter` is enabled.
5. The last node (the consumer) delivers the result.

Requirements and limits:

- connectors 1.9.0 or later on every node;
- every member of the chain has signed the contract;
- JSON data;
- one input per node, no nested chains;
- a node can pull additional data from other providers before processing; the connector merges it as `{ "origin": [...], "additionalData": [...] }`.

## Control-plane-only mode

!!! info "Connector {{ pdc_next }}, opt-in"
    This mode is part of connector version {{ pdc_next }}, in development at the time of writing.

In the default flows, data passes through both connectors. In **control-plane-only mode**, the connectors only authorise the exchange; the participants' own systems move the data, with any protocol they choose (direct HTTPS, S3, SFTP...). This suits large files, streaming, or existing transfer pipelines.

```mermaid
sequenceDiagram
  autonumber
  participant PS as Provider system
  participant P as Provider connector
  participant C as Consumer connector
  participant CS as Consumer system
  CS->>C: Create the exchange
  C->>P: Shared exchange record
  PS->>P: Authorise as provider
  CS->>C: Authorise as consumer
  Note over P,C: Each connector checks contract, policies and consent
  PS->>CS: Transfer the data directly
  PS->>P: Report transfer result
  CS->>C: Report transfer result
```

1. **Create** the exchange: `POST /controlplane/exchanges` (or the trigger and consent variants).
2. **Authorise**: each side calls `POST /controlplane/exchanges/{id}/authorize` on its own connector, which evaluates the contract, policies and consent. An authorisation is valid for 5 minutes.
3. **Transfer** the data between your systems.
4. **Report**: each side calls `POST /controlplane/exchanges/{id}/transfer` with the result and metadata (checksum, MIME type, size).
5. **Follow** the exchange with `GET /controlplane/exchanges/{id}`, or with webhooks (`exchange.created`, `participant.authorized`, `participant.denied`, `transfer.completed`, `transfer.failed`).

Enable it with `controlPlaneEnabled: true` and optionally `controlPlaneWebhookUrls`. Service chains are not available in this mode.
