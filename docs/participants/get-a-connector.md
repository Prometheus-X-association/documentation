---
render_macros: true
---

# 3. Get a connector

To take part in exchanges, your organisation needs a **Prometheus-X Dataspace Connector (PDC)**. It is the piece of software that sits between your systems and the dataspace: it checks contracts, policies and consent before each exchange, and moves the data. See [What the connector is for](../connector/index.md).

## Three options

| | Self-hosted connector | Hosted connector | Your own implementation |
| --- | --- | --- | --- |
| **What it is** | You run the open-source PDC on your infrastructure | The operator runs a PDC for you ({{ operator.name }} offers this) | You implement the Prometheus-X exchange protocol yourself |
| **Effort** | Deploy and maintain a Node.js service and a MongoDB database | Request it from the operator; nothing to deploy | High: protocol, contract checks, policy enforcement, consent, service chains |
| **Control** | Full: your credentials and data stay on your infrastructure | The host runs the connector, including the credentials it uses to reach your systems | Full |
| **Best for** | Organisations with an IT team and their own hosting | Getting started quickly, pilots, smaller organisations | Rare cases with strong constraints |

Most participants should choose between the first two. Implementing the protocol yourself is possible, since it is open, but you would have to rebuild everything the connector already does.

## Option 1: self-host the connector

The connector is open source under the MIT licence: [{{ pdc_repo }}]({{ pdc_repo }}).

**Requirements**

- A Linux server (2 vCPU, 4 GB RAM, 5 GB disk is enough to start), or a container platform.
- MongoDB 5.0 or later.
- A public HTTPS address with a valid certificate. Other participants' connectors and the core services must be able to reach it.

**Steps**

1. Deploy the connector, with Docker Compose or from source.
2. Configure it with your API keys and the operator's service addresses.
3. Start it: it registers itself in the catalogue.

[Deploy and configure the connector](../connector/deploy.md) covers each step.

## Option 2: use a hosted connector

{{ operator.name }} can deploy a connector for your organisation. It is created already configured with your organisation's keys and the {{ operator.name }} service addresses, and set up for exchange triggers, so you can start exchanges from the {{ operator.name }} interface without writing code.

Things to know:

- The host operates the connector on your behalf. The credentials the connector uses to call your APIs are stored in that hosted connector, and the data you exchange passes through it.
- You still need to make your APIs reachable from the connector and describe them in the catalogue ([step 4](connect-your-systems.md)).
- You can move to a self-hosted connector later: update the connector address in the catalogue and set up your credentials in the new connector.

Contact {{ operator.name }} to get a hosted connector.

## Check the connector is registered

When a connector starts with valid keys, it registers its address in the catalogue. On {{ operator.name }}, open your profile settings, **Endpoints** tab: your connector's HTTPS address should appear there.

## Next step

[4. Connect your systems](connect-your-systems.md)
