# schema-registry-module — Responsibility

## Module Identity

| Property | Value |
|---|---|
| Module ID | `schema-registry` |
| Deployment | `schema-registry-module` |
| Port | 8816 |
| Engine | Spring Boot 3.1.5, Java 17 |

## Why this module exists

Kafka event contracts (topic names, JSON Schemas) are currently only accessible as
classpath resources inside `common-module`. No standalone API surface exposes this
metadata for frontend, admin tools, or other consumers. This module provides a
dedicated, read-only REST API for the Kafka event schema registry.

## Core Responsibilities

1. **Topic catalog** — `GET /api/schema-registry/topics` lists all Kafka topics from `manifest.json`
2. **Schema retrieval** — `GET /api/schema-registry/schemas/{topic}` returns the JSON Schema
3. **Manifest** — `GET /api/schema-registry/manifest` returns the full manifest

## Non-Responsibilities

- This module does NOT replace Confluent Schema Registry
- This module does NOT perform schema compatibility checks
- This module does NOT write or generate schemas — `common-module` is the SSOT

## Dependencies

- `common-module` (for `manifest.json` and JSON Schema files on classpath)
- No database required (read-only service)
