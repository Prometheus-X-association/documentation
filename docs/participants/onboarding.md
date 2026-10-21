---
render_macros: true
---

# 1. Onboard to a catalogue

Your first step is to register your organisation in the catalogue of the dataspace operator. This creates your **participant** identity in the dataspace.

## Sign up

On {{ operator.name }}:

1. Go to [{{ operator.app_url }}]({{ operator.app_url }}) and sign up with your email address.
2. Enter the verification code sent to you.
3. Create your personal account.
4. Fill in your organisation's information: name, description, logo.

The detailed screens are described in the [{{ operator.name }} onboarding guide]({{ operator.docs_url }}/application/onboarding.html).

## What onboarding creates

| Created | What it is used for |
| --- | --- |
| **Participant** | Your organisation's identity in the dataspace. It has a public description at a stable URL, which contracts and consents use to refer to you. |
| **Administrator account** | Your personal access to the catalogue's interface. You can invite colleagues as administrators. |
| **API keys** (service key and secret key) | Used by your connector to authenticate to the core services. Find them in your [profile settings]({{ operator.settings_url }}). |

!!! warning "Keep the secret key secret"
    The secret key identifies your organisation to the dataspace. Store it like a password and only put it in your connector's configuration. If you regenerate it, update your connector at the same time, otherwise its exchanges are blocked.

## Visibility

On {{ operator.name }}, an organisation is public by default: other participants can find it, invite it and negotiate with it. You can make it private, in which case only organisations you grant access to (through an invitation link or a use-case invitation) can interact with you. See [visibility settings]({{ operator.docs_url }}/settings/visibility.html).

## Next step

[2. Offers and use cases](offers-and-use-cases.md)
