# Why a dedicated protocol

Prometheus-X does not use the Eclipse Dataspace Protocol (DSP) for its exchanges, nor the protocols of other connectors such as FIWARE's. This is a deliberate choice: the use cases Prometheus-X was built for, starting with education and skills, need three things these protocols do not support.

## 1. Consent and personal data

In education and skills, much of the valuable data is personal: learning records, skills, diplomas, career paths. Under the GDPR, it can be exchanged when the individual consents, and the individual must be able to see what they agree to and withdraw it.

**What DSP covers.** DSP negotiates a contract between a provider and a consumer, then transfers data. The individual whose data is exchanged is not part of the protocol: there is no notion of privacy notice, consent, or user identity across organisations.

**What Prometheus-X adds.**

- Privacy notices generated from the signed contract, so the individual consents to exactly what the organisations agreed.
- The individual's consent as the **trigger** of the exchange, not just a precondition checked on the side.
- Re-validation of consent by the connector just before the data moves.
- Linking of the same person's accounts across organisations, so each organisation keeps its own identifiers.
- Consent records following ISO/IEC TS 27560.

See [Personal data and consent](../learn/personal-data-and-consent.md).

## 2. Multi-party contracts

Real use cases rarely involve just two organisations. A regional skills programme can bring together schools, training providers, employment services and an analytics provider, under the coordination of an orchestrator.

**What DSP covers.** Contracts are bilateral: one provider, one consumer, one offer. A use case with ten organisations becomes dozens of separate agreements, with no shared view of who is in it, what roles they have and what rules apply to all.

**What Prometheus-X adds.**

- **Ecosystem contracts** signed by all members of a use case.
- An **orchestrator** role that invites members and governs the use case.
- **Roles** with obligations that apply to every member holding them.
- **Per-offer policies**, negotiated with each member, inside one contract.
- Members can join an existing contract over time.

See [Offers, use cases and contracts](../learn/contracts-and-policies.md).

## 3. Service chains

Data often has to be transformed between the provider and the consumer: anonymised, converted, enriched, or processed close to where it lives.

**What DSP covers.** A transfer goes from one provider to one consumer. Any intermediate processing has to be set up as separate exchanges, each with its own agreement, and nothing ties them together.

**What Prometheus-X adds.** Chains of processing services, defined in the use-case contract, executed across the connectors of every participant involved, with policy and consent checks at each step. Processing steps appear in the privacy notice when personal data is involved.

See [Service chains](../learn/service-chains.md).

## The cost, and how it is addressed

A dedicated protocol means Prometheus-X exchanges do not run natively between Prometheus-X and DSP connectors. Prometheus-X addresses this in four ways:

1. **Common ground where possible.** Policies use ODRL, catalogues can be exported as DCAT, the data model follows Gaia-X, and the coming identity layer uses verifiable credentials and Gaia-X compliance.
2. **DSP where it fits.** For bilateral contracts, which DSP covers, Prometheus-X participants can negotiate with DSP connectors over the DSP Contract Negotiation Protocol, as piloted in DSIF. See [Eclipse DSP and EDC](dsp-and-edc.md#contract-negotiation-over-dsp).
3. **Extensions of DSP.** Within [DSIF](dsif.md), Prometheus-X and its partners propose backward-compatible extensions of DSP contract negotiation for multi-party contracts and intermediaries, and a federated consent interface.
4. **Separating control and data.** The connector's control-plane-only mode lets other transfer systems move the data once Prometheus-X has authorised the exchange.

The aim is not a parallel world: it is to bring consent, multi-party agreements and service chains into the common dataspace protocols.
