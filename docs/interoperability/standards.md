# Standards

How Prometheus-X relates to the main standards used in dataspaces.

| Standard | Status in Prometheus-X |
| --- | --- |
| **ODRL 2.2** | Policies follow the ODRL structure (permissions, prohibitions, constraints, duties) in JSON. Left operands are Prometheus-X names, and no ODRL profile is published yet. Policies are evaluated by the connector with Prometheus-X's open-source ODRL engine. |
| **DCAT 3 / DCAT-AP** | The open-source catalogue can export offers and resources in DCAT form. DSIF catalogue federation uses DCAT-AP JSON-LD. |
| **Eclipse Dataspace Protocol** | Exchanges within Prometheus-X use the Prometheus-X protocol, by design (see [Why a dedicated protocol](why-a-dedicated-protocol.md)). The **Contract Negotiation Protocol** is implemented for bilateral contracts with DSP-based dataspaces, as piloted in DSIF, with trust between systems configured manually until identity federation is in place ([details](dsp-and-edc.md#contract-negotiation-over-dsp)). DSIF contract federation extends it for multi-party contracts and intermediaries. |
| **Decentralized Claims Protocol (DCP)** | Not used. |
| **Gaia-X Trust Framework** | Data model follows Gaia-X naming. Gaia-X compliance credentials are on the identity roadmap. |
| **W3C Verifiable Credentials and DIDs** | Not used in core flows today. Planned for participant identity (with `did:web`) and for the DSIF assertion. The Data Veracity building block already issues verifiable credentials. |
| **OID4VCI / OID4VP** | Planned for issuing and presenting participant credentials. |
| **ISO/IEC TS 27560** (consent records) | Consent receipts follow its structure. |
| **GDPR** | Privacy notices are structured after the information required by articles 13 and 14; fields that contracts do not contain (such as retention or the data protection officer) are completed by the organisations. Consent is the legal basis handled. |
| **IDSA Reference Architecture** | Principles followed (connectors, usage control, intermediaries); the IDS information model and token service are not used. |
| **TM Forum Open APIs** | Mappings with contract negotiation and service chains being worked out in DSIF with FIWARE. |
| **JSON-LD** | Descriptions carry `@context` and `@type`, with Prometheus-X vocabularies. |

## Gaps and next steps

The areas where Prometheus-X is moving closer to standards:

- **Identity**: verifiable credentials, OID4VP and Gaia-X compliance ([roadmap](../roadmap.md)).
- **Contracts**: bilateral negotiation over DSP beyond pilots, then multi-party and intermediary extensions of DSP contract negotiation ([DSIF](dsif.md#contract-federation)).
- **Policies**: publishing an ODRL profile for Prometheus-X operands.
- **Catalogues**: DCAT-AP federation ([DSIF](dsif.md#catalogue-federation)).
