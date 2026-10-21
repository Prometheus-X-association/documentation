---
render_macros: true
---

# 6. Personal data (optional)

If your offers contain personal data, they can only be exchanged with the consent of each individual concerned. This step sets that up. For the concepts, read [Personal data and consent](../learn/personal-data-and-consent.md) first.

Both the provider and the consumer of personal data do these steps.

## 1. Register with the Consent Manager

Connect your connector to the operator's Consent Manager. On {{ operator.name }}, this is done from the **Endpoints** tab of your profile settings; your connector's configuration must contain the consent address `{{ operator.consent_uri }}`.

See [Registering to the consent service]({{ operator.docs_url }}/guide/setting-up-for-consent.html).

## 2. Mark your resources as personal data

Tick "contains personal data" on your data resources, and "uses personal data" on your software resources. Use `{userId}` in their URLs so your connector can fetch and deliver data for a given person.

## 3. Register your users

Tell your connector who your users are: their email and your own internal identifier for them. The connector passes this on to the Consent Manager, which links the accounts of the same person across organisations.

```bash
curl -X POST https://your-connector.example/private/users \
  -H "Authorization: Bearer <connector admin token>" \
  -H "Content-Type: application/json" \
  -d '{ "email": "learner@example.org", "internalID": "u-12345" }'
```

Bulk import from CSV or Excel files is also available.

If you are a consumer and a person may not have an account with you yet, set `registrationUri` in your connector's configuration to an endpoint of your application that creates accounts. When needed, your connector calls it, receives the new identifier, and the exchange resumes.

## 4. Show the consent interface to your users

Ask your connector for the address of the consent interface for a user and a privacy notice, then embed it in your application (in a frame) or redirect the user to it.

```bash
curl "https://your-connector.example/private/pdi?userId=u-12345&privacyNoticeId=<id>" \
  -H "Authorization: Bearer <connector admin token>"
```

The user reads the privacy notice and gives or refuses consent.

## 5. What happens next

When the user consents, the exchange starts on its own:

1. The Consent Manager sends the signed consent to the provider's and consumer's connectors.
2. The provider's connector checks the consent is still valid and fetches the user's data from your system, using your identifier in `{userId}`.
3. The consumer's connector delivers it to the consumer's application, using the consumer's identifier for the same person.

Your application receives headers with the consent identifier, so you can link each delivery to the consent behind it.

When the user withdraws consent, no further exchange happens under it.

See [Exchange flows](../connector/exchange-flows.md#consent-based-exchange) for the full sequence.
