---
render_macros: true
---

# 5. Run your first exchange

Everything is in place: a signed contract, a running connector and resources that point to your systems. Test the setup, then trigger an exchange.

## Test in the Tech Space

{{ operator.name }}'s **Tech Space** is a sandbox that runs reference connectors you can exchange with. It lets you check, step by step:

1. that your connector answers (ping);
2. a project exchange, as provider or as consumer;
3. a service chain;
4. the API consumption flow;
5. a consent-based exchange.

A log viewer shows what happened at each step. See the [{{ operator.name }} testing guide]({{ operator.docs_url }}/guide/testing.html).

## Trigger an exchange

Exchanges are started on the connector. Either side of the contract can start one.

=== "From the operator's interface"

    With **exchange triggers**, you start exchanges from the {{ operator.name }} interface without writing code. The interface lists the exchanges you can run: as provider, as consumer, or as orchestrator of a service chain. It only lists exchanges for which you are a member of the project, your offer is technically configured, and every party has signed.

    Exchange triggers need connector version 1.10.0 or later. See the [{{ operator.name }} guide]({{ operator.docs_url }}/guide/exchange-triggers.html).

=== "From your application"

    Call your connector's exchange endpoint with the contract and the two offers involved:

    ```bash
    curl -X POST https://your-connector.example/exchange \
      -H "Authorization: Bearer <connector admin token>" \
      -H "Content-Type: application/json" \
      -d '{
        "contract": "{{ operator.contract_uri }}contracts/<contract id>",
        "purposeId": "<consumer offer URL>",
        "resourceId": "<provider offer URL>"
      }'
    ```

    The response tells you whether the exchange succeeded. See [Exchange flows](../connector/exchange-flows.md) for all options.

## What happens during the exchange

1. Your connector reads the contract and the offers' descriptions.
2. It creates an exchange record, shared with the other connector.
3. The provider's connector checks the policies of its offer, then fetches the data from the provider's system.
4. It sends the data to the consumer's connector.
5. The consumer's connector delivers it to the consumer's application.
6. Both connectors mark the exchange as successful, or record the step where it failed.

## When something goes wrong

| Symptom | Check |
| --- | --- |
| The exchange is refused before anything moves | Is the contract signed by all parties? Are both offers in the contract? Do the policies allow the exchange (dates, number of uses)? |
| "Personal data" error | The resource contains personal data: it must go through the [consent flow](personal-data.md). |
| Provider export error | Can the provider's connector reach the data URL? Is the credential identifier right? |
| Consumer import error | Can the consumer's connector reach the delivery URL? Does the application accept the format? |
| Nothing works since the keys were regenerated | Update the keys in the connector configuration and reload it. |

The connector records a status for every exchange (see [Reference](../connector/reference.md#exchange-statuses)), which tells you which step failed.

## Next step

If your offers contain personal data: [6. Personal data](personal-data.md). Otherwise, you are done: your organisation is part of the dataspace.
