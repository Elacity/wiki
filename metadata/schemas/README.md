# Metadata Standard (JSON)

This folder is the main entrypoint for metadata standards across the Ela.City ecosystem.

Its purpose is to define, version, and document all metadata models in a single place using JSON-based standards so contracts, indexers, APIs, SDKs, and client applications can share one consistent interpretation.

## What this folder is for

- Define canonical JSON metadata structures used across ecosystem entities.
- Keep metadata contracts explicit, machine-readable, and validation-friendly.
- Provide a single governance surface for schema evolution and backward compatibility.
- Align on field naming, data types, required fields, enum values, and extension rules.
- Make integrations predictable across onchain and offchain components.

## Ecosystem coverage

This location is intended to enumerate all metadata categories used throughout the ecosystem, including but not limited to:

- Channel metadata
- Asset and item metadata
- Plan and subscription metadata
- Access and entitlement metadata
- Royalty metadata

## Standard principles

- JSON-first: metadata payloads are defined as JSON structures.
- Deterministic: each schema must have clear required/optional semantics.
- Versioned: breaking and non-breaking changes are tracked through schema versioning.
- Extensible: custom fields are allowed only through explicit extension boundaries.
- Interoperable: schema definitions must be stable across contracts, services, and SDKs.

## Recommended structure

Use a domain-first, versioned layout. For a single channel metadata model:

```text
metadata/schemas/
  README.md
  channel/
    v1.0/
      schema.json
      example.json
      changelog.md (optional)
```

Guidelines:

- `schema.json` is the canonical JSON Schema for that version.
- `example.json` is a valid payload for that exact schema version.
- Each new version gets its own folder (for example `v1.1`, `v2.0`).
- Older versions are immutable and remain documented for compatibility.

## Schema references

Current `v1.0` schemas:

- Access Token: `https://raw.githubusercontent.com/Elacity/wiki/main/metadata/schemas/access-token/v1.0/schema.json`
- Asset: `https://raw.githubusercontent.com/Elacity/wiki/main/metadata/schemas/asset/v1.0/schema.json`
- Channel: `https://raw.githubusercontent.com/Elacity/wiki/main/metadata/schemas/channel/v1.0/schema.json`
- Content: `https://raw.githubusercontent.com/Elacity/wiki/main/metadata/schemas/content/v1.0/schema.json`
- Distribution Right: `https://raw.githubusercontent.com/Elacity/wiki/main/metadata/schemas/distribution-right/v1.0/schema.json`
- MCO: `https://raw.githubusercontent.com/Elacity/wiki/main/metadata/schemas/mco/v1.0/schema.json`
- Plan: `https://raw.githubusercontent.com/Elacity/wiki/main/metadata/schemas/plan/v1.0/schema.json`
- Royalty: `https://raw.githubusercontent.com/Elacity/wiki/main/metadata/schemas/royalty/v1.0/schema.json`
- Subscription: `https://raw.githubusercontent.com/Elacity/wiki/main/metadata/schemas/subscription/v1.0/schema.json`
