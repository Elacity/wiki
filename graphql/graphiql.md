---
description: Interactive GraphiQL explorer embeds by environment.
---

# GraphiQL Explorer

Use these embedded explorers for an interactive GraphQL workflow.

{% hint style="info" %}
Replace each placeholder URL below with your hosted GraphiQL page URL for that environment.
{% endhint %}

{% tabs %}
{% tab title="Staging" %}
{% embed url="https://staging.ela.city/api/2.0/graphql" %}
{% endtab %}

{% tab title="ESC" %}
{% embed url="https://next.ela.city/api/2.0/graphql" %}
{% endtab %}

{% tab title="Base" %}
{% embed url="https://base.ela.city/api/2.0/graphql" %}
{% endtab %}
{% endtabs %}

## Notes

- Ensure CORS allows requests from your docs domain.
- If auth is required, make sure your hosted GraphiQL supports header/token injection.
