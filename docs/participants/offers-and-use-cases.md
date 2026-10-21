---
render_macros: true
---

# 2. Offers and use cases

Once registered, you describe what you bring to the dataspace and agree with other participants on how it is used. For the concepts behind this page, see [Offers, use cases and contracts](../learn/contracts-and-policies.md).

## Publish your offers

Providers describe their data and services as resources, then package them in offers. Only published offers are visible to others.

1. **Create an offer**: name, short description, category, location.
2. **Add resources** to it:
    - a **data resource** for each dataset or API that provides data. State whether it contains personal data;
    - a **software resource** for each service or application that uses data.
3. **Set pricing and default policies.** The default policy is "no restriction"; more specific policies are usually agreed during negotiation.
4. **Publish** the offer.

You fill in the technical part of each resource (how your connector reaches it) in [step 4](connect-your-systems.md). You can publish first and complete it later.

{{ operator.name }} guides: [creating an offer]({{ operator.docs_url }}/application/registering-resources/creating-an-offer.html), [data resources]({{ operator.docs_url }}/application/registering-resources/data-resources.html), [service resources]({{ operator.docs_url }}/application/registering-resources/service-resources.html), [infrastructure offers]({{ operator.docs_url }}/application/registering-resources/infrastructure-offers.html).

!!! tip "Categories matter"
    The catalogue matches offers with use cases by category. Choose categories carefully so that orchestrators looking for your kind of data or service find you.

## Join a use case

Exchanges happen within a **use case** (called a *project* in {{ operator.name }}). There are two ways in:

| Way in | Who starts it |
| --- | --- |
| **Invitation** | The orchestrator invites your organisation and proposes which of your offers to use |
| **Join request** | You ask to join a published use case, proposing your offers. Only organisations with published offers can ask. |

You then **negotiate** with the orchestrator: which offers are used, which policies apply to each one (for example a time limit or a maximum number of uses), and at what price. Each side can accept or send a counter-proposal. Changing any value means sending a counter-proposal.

When you agree, you **sign**. Your offers become usable in exchanges once the use-case contract is signed.

{{ operator.name }} guides: [project negotiation]({{ operator.docs_url }}/application/negotiation/project-negotiation.html), [invitations]({{ operator.docs_url }}/application/negotiation/invitations.html), [join requests]({{ operator.docs_url }}/application/negotiation/join-requests.html).

## Create a use case (orchestrators)

1. **Create the project**: name, description, categories.
2. **Describe what you are looking for**: the categories of data and services you need, and the building blocks the use case relies on.
3. **Add your own offers**, if you also provide data or services in it (*contributions*).
4. **Publish** the project. This creates its contract. On {{ operator.name }}, publishing a project requires a subscription.
5. **Invite participants** and handle join requests.
6. **Negotiate and sign** with each member.
7. **Build service chains**, if data must go through processing steps.

{{ operator.name }} guides: [creating a project]({{ operator.docs_url }}/application/projects/creating-a-project.html), [project settings]({{ operator.docs_url }}/application/projects/project-settings.html), [service chains]({{ operator.docs_url }}/application/projects/service-chains.html).

## Bilateral agreements

Two participants can also agree on a contract directly, outside a use case: the consumer requests access to an offer, the provider accepts, both negotiate and sign. Bilateral contracts are fully supported by the Contract Manager and the connector; check with your operator whether its interface offers them.

## Next step

[3. Get a connector](get-a-connector.md)
