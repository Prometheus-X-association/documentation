---
render_macros: true
---

# Connector reference

Quick reference for connector configuration, routes and statuses. The connector also serves its OpenAPI documentation, and the [connector wiki]({{ pdc_wiki }}) has detailed guides.

## Configuration file

`src/config.<NODE_ENV>.json`. After the first start, the copy stored in the database takes precedence; apply file changes with `POST /private/configuration/reload`.

| Key | Type | Purpose |
| --- | --- | --- |
| `endpoint` | URL | Public address of the connector, with trailing slash |
| `serviceKey` | string | Organisation's service key from the catalogue |
| `secretKey` | string | Organisation's secret key from the catalogue |
| `catalogUri` | URL | Catalogue address |
| `contractUri` | URL | Contract Manager address |
| `consentUri` | URL | Consent Manager address |
| `registrationUri` | URL | Application endpoint that creates user accounts (consent flows) |
| `billingUri` | URL | Billing service for subscription and payment policies |
| `dvctUri` | URL | Data value chain tracker, for contracts that use it |
| `credentials` | array | Credentials `{ _id, type, key, value }` defined in the file |
| `expressLimitSize` | string | Maximum request size, such as `20mb` |
| `serviceChainAdapter` | boolean | Resume service chains automatically for synchronous services |
| `serviceChainAdapterTimeout` | number | Time in ms the adapter waits for the service |
| `controlPlaneEnabled` | boolean | Enable control-plane-only mode ({{ pdc_next }}) |
| `controlPlaneWebhookUrls` | array | Webhooks for control-plane events ({{ pdc_next }}) |
| `proxy` | object | Default outbound proxy `{ host, port, protocol, username, password }` |

## Environment variables

| Variable | Purpose |
| --- | --- |
| `NODE_ENV` | `production` for deployments; also selects the configuration file |
| `PORT` | Listening port |
| `MONGO_URI` | MongoDB connection string |
| `MONGO_INITDB_ROOT_USERNAME`, `MONGO_INITDB_ROOT_PASSWORD` | MongoDB admin user (Docker) |
| `SESSION_SECRET`, `SESSION_COOKIE_EXPIRATION` | Sessions |
| `JWT_BEARER_TOKEN_EXPIRATION`, `JWT_REFRESH_TOKEN_EXPIRATION` | Admin token lifetimes |
| `EXCHANGE_TIMEOUT` | Seconds a triggered exchange waits for completion |
| `EXCHANGE_TRIGGER_API_KEY` | Key the operator uses to start exchanges through exchange triggers |
| `KPI_API_KEY` | Key for monitoring dashboards reading KPIs ({{ pdc_next }}) |
| `CURATOR`, `MAINTAINER` | Shown in the connector's self-description |
| `ENABLE_LOGS`, `WINSTON_LOGS_MAX_FILES`, `WINSTON_LOGS_MAX_SIZE` | Logging |

## Routes for your applications

These are the routes participants call. Private routes need `Authorization: Bearer <token>` from `POST /login`.

| Route | Purpose |
| --- | --- |
| `GET /` | Self-description of the connector, including its version |
| `POST /login`, `POST /refresh` | Get or refresh an admin token with your service key and secret key |
| `POST /exchange` | Trigger an exchange |
| `POST /exchange/external/trigger` | Exchange trigger from the operator (API key) |
| `GET`, `PUT /private/configuration`, `POST /private/configuration/reload` | Read, update and reload the configuration |
| `/private/credentials` | Create, list, read and update credentials |
| `/private/catalogs` | Enable or disable the resources your connector exposes |
| `/private/users` | Manage users for consent flows, including CSV and Excel import |
| `GET /private/pdi` | Consent interface address for a user |
| `/private/consent/*` | Consent operations, relayed to the Consent Manager |
| `/private/kpis/exchanges/*` | Exchange statistics: overview, by offer, service chains, volume, errors ({{ pdc_next }}) |
| `POST /service-chain/resume` | Resume a service chain with your service's result |
| `/controlplane/*` | Control-plane-only mode ({{ pdc_next }}) |

The connector also exposes routes that other connectors and the Consent Manager call during exchanges (`/provider/*`, `/consumer/*`, `/consent/*`, `/data/*`, `/dataexchanges`, `/service-chain/node/*`). Your applications do not call them directly.

## Exchange statuses

| Status | Meaning |
| --- | --- |
| `PENDING` | In progress |
| `EXPORT_SUCCESS` | The provider fetched and sent the data |
| `IMPORT_SUCCESS` | The consumer delivered the data: the exchange succeeded |
| `PEP_ERROR` | Refused by the contract or policy checks |
| `PROVIDER_EXPORT_ERROR` | The provider's connector could not fetch or send the data |
| `CONSUMER_IMPORT_ERROR` | The consumer's connector could not deliver the data |
| `CONSENT_EXPORT_ERROR`, `CONSENT_IMPORT_ERROR` | A consent step failed |
| `NODE_CALLBACK_ERROR` | A service chain node failed |
| `UNDEFINED_ERROR` | Other error |

## Policy operands

The connector evaluates these ODRL constraints locally:

| Operand | Checked against |
| --- | --- |
| `dateTime` | The current date |
| `count` | The number of past exchanges for this contract and resource, counted by the connector |
| `payAmount`, `limitDate`, `usageCount` | The billing service at `billingUri` |

## Version compatibility

Some features require a minimum version on **every** connector involved in an exchange.

| Feature | Minimum version |
| --- | --- |
| Service chains | 1.9.0 |
| Exchange triggers | 1.10.0 |
| Direct response visualization | 1.11.0 |
| Control-plane-only mode, S3 sources, KPI routes | {{ pdc_next }} |

Connectors read each other's version from their self-description before using a feature.
