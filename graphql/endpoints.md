---
description: Environment-specific GraphQL endpoints.
---

# Endpoints

{% tabs %}
{% tab title="Staging" %}
**Endpoint**  
`https://staging.ela.city/api/2.0/graphql`

```bash
curl -X POST "https://staging.ela.city/api/2.0/graphql" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <TOKEN>" \
  --data '{"query":"query Health { __typename }"}'
```
{% endtab %}

{% tab title="ESC" %}
**Endpoint**  
`https://next.ela.city/api/2.0/graphql`

```bash
curl -X POST "https://next.ela.city/api/2.0/graphql" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <TOKEN>" \
  --data '{"query":"query Health { __typename }"}'
```
{% endtab %}

{% tab title="Base" %}
**Endpoint**  
`https://base.ela.city/api/2.0/graphql`

```bash
curl -X POST "https://base.ela.city/api/2.0/graphql" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <TOKEN>" \
  --data '{"query":"query Health { __typename }"}'
```
{% endtab %}
{% endtabs %}
