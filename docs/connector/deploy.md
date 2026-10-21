---
render_macros: true
---

# Deploy and configure

This page explains how to run your own connector. If you use a connector hosted by the operator, it is already deployed and configured: skip to [Connect your applications](applications.md).

## Requirements

| Item | Minimum |
| --- | --- |
| Host | Linux server or container platform, 2 vCPU, 4 GB RAM, 5 GB disk |
| Runtime | Docker, or Node.js 22+ with pnpm |
| Database | MongoDB 5.0+ (a container is provided with Docker Compose) |
| Network | A public HTTPS address with a valid certificate, usually behind a reverse proxy on port 443 |

The connector must be able to reach, and be reached by:

| Peer | Direction |
| --- | --- |
| Other participants' connectors | Inbound and outbound HTTPS |
| Catalogue and Contract Manager | Outbound HTTPS |
| Consent Manager (personal data only) | Inbound and outbound HTTPS |
| Your own APIs and applications | Outbound from the connector |
| github.com, at build time | Outbound: some dependencies are pulled from GitHub |

## Install

=== "Docker Compose"

    ```bash
    git clone {{ pdc_repo }}.git
    cd dataspace-connector
    cp .env.sample .env
    cp src/config.sample.json src/config.production.json
    # edit .env and src/config.production.json (see below)
    docker compose up -d --build
    ```

    This starts the connector and a MongoDB container. To use your own MongoDB, set `MONGO_URI` in `.env`.

=== "From source"

    ```bash
    git clone {{ pdc_repo }}.git
    cd dataspace-connector
    npm i -g pnpm
    pnpm i
    cp .env.sample .env
    cp src/config.sample.json src/config.production.json
    # edit .env and src/config.production.json (see below)
    ```

    Then build and start the service with the scripts in `package.json`.

The configuration file is named after `NODE_ENV`: `src/config.production.json` when `NODE_ENV=production`. Set `NODE_ENV=production` for any deployment other than local development.

!!! warning "Change the default database credentials"
    The Docker setup creates a MongoDB user with default credentials. Change them in `.env` and in `mongo_init.js` before deploying.

## Configure

### Configuration file

The minimum configuration to join {{ operator.name }}:

```json
{
  "endpoint": "https://pdc.your-organisation.eu/",
  "serviceKey": "<service key from your catalogue settings>",
  "secretKey": "<secret key from your catalogue settings>",
  "catalogUri": "{{ operator.catalog_uri }}",
  "contractUri": "{{ operator.contract_uri }}",
  "consentUri": "{{ operator.consent_uri }}"
}
```

| Key | Required | Purpose |
| --- | --- | --- |
| `endpoint` | Yes | Public HTTPS address of your connector, with a trailing slash |
| `serviceKey`, `secretKey` | Yes | Your organisation's API keys, from the catalogue ([{{ operator.name }} settings]({{ operator.settings_url }})) |
| `catalogUri`, `contractUri`, `consentUri` | Yes | Addresses of the operator's core services |
| `registrationUri` | No | Endpoint of your application that creates a user account, for consent flows |
| `billingUri` | No | Billing service checked for subscription and payment policies |
| `credentials` | No | Credentials defined in the file instead of through the API |
| `expressLimitSize` | No | Maximum payload size, such as `20mb` |
| `serviceChainAdapter`, `serviceChainAdapterTimeout` | No | Automatic handling of synchronous services in chains |
| `proxy` | No | Outbound proxy |
| `controlPlaneEnabled`, `controlPlaneWebhookUrls` | No | Control-plane-only mode ({{ pdc_next }}) |

The full list is in the [Reference](reference.md#configuration-file).

### Environment

The `.env` file sets the runtime: `NODE_ENV`, `PORT`, `MONGO_URI`, token lifetimes, the exchange timeout, and API keys for exchange triggers and KPI dashboards. See the [Reference](reference.md#environment-variables).

## Start and verify

On start, the connector:

1. reads the configuration file and saves it to its database;
2. authenticates to the catalogue with your keys and registers its `endpoint`;
3. caches the description of your resources.

Check the registration in the catalogue: on {{ operator.name }}, your profile settings, **Endpoints** tab, should show your connector's address.

You can also open `https://pdc.your-organisation.eu/` in a browser: the connector answers with a short description of itself, including its version.

## Change the configuration later

After the first start, the configuration stored in the database takes precedence over the file. After editing the file, reload it:

```bash
curl -X POST https://pdc.your-organisation.eu/private/configuration/reload \
  -H "Authorization: Bearer <admin token>"
```

You can also read and update the configuration through `GET` and `PUT /private/configuration`.

!!! note "Regenerated API keys"
    If you regenerate your organisation's keys in the catalogue, update `serviceKey` and `secretKey` in the connector and reload. Until then, exchanges are blocked.

## Get an admin token

Calls to your connector's private routes need a token. Get one with your organisation's keys:

```bash
curl -X POST https://pdc.your-organisation.eu/login \
  -H "Content-Type: application/json" \
  -d '{ "serviceKey": "<service key>", "secretKey": "<secret key>" }'
```

The response contains a `token`, to send as `Authorization: Bearer <token>`, and a `refreshToken`, to use with `POST /refresh` when the token expires.

## Upgrade

Pull the new version, rebuild and restart. Check the [version compatibility](reference.md#version-compatibility) table: some features need both connectors in an exchange to run a recent enough version.
