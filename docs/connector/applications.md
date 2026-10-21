---
render_macros: true
---

# Connect your applications

The connector talks to your systems in two directions: it **fetches** data from your data sources when you are the provider, and **delivers** data to your applications when you are the consumer. This page explains how to set up both.

The connector learns where to go from the **representation** of each resource in the catalogue, and how to authenticate from the **credentials** it stores.

## Credentials

A credential is a secret the connector uses to call one of your systems. It is stored only in your connector; the catalogue only holds its identifier.

```bash
curl -X POST https://pdc.your-organisation.eu/private/credentials \
  -H "Authorization: Bearer <admin token>" \
  -H "Content-Type: application/json" \
  -d '{ "type": "apiKey", "key": "x-api-key", "value": "<secret>" }'
```

The response contains the credential's `_id`: copy it into the resource representation in the catalogue.

| Type | `key` | `value` | Used for |
| --- | --- | --- | --- |
| `apiKey` | Header name | Header value | APIs protected by an API key header |
| `basic` | Username | Password | HTTP basic authentication |
| `proxy` | Proxy username | Proxy password | Authenticated outbound proxy for one resource |
| `s3` | | Bucket settings | S3-compatible storage ({{ pdc_next }}) |

Manage credentials with `GET /private/credentials`, `GET /private/credentials/{id}` and `PUT /private/credentials/{id}`. To get an admin token, see [Deploy and configure](deploy.md#get-an-admin-token).

## Representations

A representation is set on each data resource and software resource in the catalogue.

| Field | Meaning |
| --- | --- |
| Source type | `REST` for HTTP APIs. PostgreSQL is experimental; S3 comes with {{ pdc_next }}. |
| URL | Where the connector fetches (data resource) or delivers (software resource) the data |
| Authentication | `none`, `basic` or `apiKey` |
| Credential | The identifier of the credential in your connector |
| Format | The MIME type, such as `application/json` |
| Query parameters | Parameters the other party may pass at exchange time |
| Proxy | Host, port and optional credential of a proxy to use for this resource |

### URL variables

| Variable | Replaced with |
| --- | --- |
| `{userId}` | Your own identifier for the user concerned (consent flows) |
| `{url}` | The URL stored on that user in your connector |

### Query parameters

If a data resource allows query parameters (for example `from` and `to`), the party triggering the exchange can pass values, and your connector adds them to the URL when it fetches the data. Only the parameters declared on the resource are forwarded.

### Formats

| Format | Supported flows |
| --- | --- |
| `application/json` (default) | All flows |
| `application/pdf`, `text/csv`, `text/plain`, `application/octet-stream` | Business-to-business exchanges |

For files, the connector checks the size, the MIME type and a checksum on both sides.

## What your applications receive

### Fetching data (provider side)

Your connector calls the data resource URL with a `GET`, with the credential headers and any query parameters. Your API answers with the data.

### Delivering data (consumer side)

Your connector calls the software resource URL with a `POST` carrying the data. Your application should answer quickly with a success status: it can process the data afterwards.

### Headers

The connector adds headers that describe the exchange, so your systems can trace and log it:

| Header | Content |
| --- | --- |
| `x-ptx-dataExchangeId` | Identifier of the exchange |
| `x-ptx-contractId`, `x-ptx-contractURL` | The contract the exchange runs under |
| `x-ptx-incomingDataspaceConnectorURI` | The other party's connector |
| `x-ptx-consent-id` | The consent behind the exchange (consent flows) |
| `x-ptx-service-chain-id`, `x-ptx-target-id`, `x-ptx-service-chain-next-target`, `x-ptx-service-chain-previous-target` | Position in a service chain |

## Users (personal data)

For consent-based exchanges, register your users in the connector with your own identifier and their email:

| Route | Purpose |
| --- | --- |
| `POST /private/users` | Register one user: `{ "email": "...", "internalID": "..." }` |
| `POST /private/users/import` | Import users from a CSV or Excel file |
| `GET /private/users`, `GET /private/users/{id}` | List and read users |
| `PUT /private/users/{id}`, `DELETE /private/users/{id}` | Update or remove a user |
| `GET /private/pdi?userId={internalID}&privacyNoticeId={id}` | Address of the consent interface for this user |

If you are a consumer, set `registrationUri` in the configuration to an endpoint of your application that creates an account for a person you do not know yet. It receives the person's details and must return your new internal identifier for them.

## Service chain nodes

If you are an infrastructure provider, your service receives the data and the chain parameters from your connector. When it is done, it hands the result back:

```bash
curl -X POST https://pdc.your-organisation.eu/service-chain/resume \
  -H "Content-Type: application/json" \
  -d '{ "chainId": "<x-ptx-service-chain-id>", "targetId": "<x-ptx-target-id>", "data": { }, "params": { } }'
```

For services that answer synchronously and quickly, set `serviceChainAdapter: true` in the configuration: the connector then takes the response of your service and resumes the chain on its own. See [Exchange flows](exchange-flows.md#service-chains).
