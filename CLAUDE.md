# CLAUDE.md

## Overview

This repository contains control catalogs for the [FINOS Common Cloud Controls](https://commoncloudcontrols.com) project. Each `controls.yaml` file conforms to the Gemara `ControlCatalog` schema.

## Gemara MCP Server

This repo includes a `.mcp.json` that configures the Gemara MCP server. Use it to **validate catalog files** against the schema during editing:

- `validate_gemara_artifact` — validate a controls.yaml file against the Gemara schema
- `migrate_gemara_artifact` — migrate a file to a newer schema version
- `gemara://schema/definitions` — browse the schema type definitions
- `gemara://lexicon` — look up Gemara terminology

**Always validate after making changes** to a controls.yaml file.

## File Structure

```
<category>/<service>/controls.yaml
```

Each file has:
- `imports:` — references to core controls (`CCC.Core.Controls`)
- `controls:` — flat list of service-specific control entries (not nested under `control-families:`)

## Entry Structure

```yaml
controls:
  - id: CCC.<Service>.CN<nn>
    title: Human-readable title
    group: <GroupID>
    objective: |
      What this control ensures.
    assessment-requirements:
      - id: CCC.<Service>.CN<nn>.AR<nn>
        text: |
          When <trigger>, the service MUST <requirement>.
        applicability:
          - tlp-clear
          - tlp-green
          - tlp-amber
          - tlp-red
        recommendation: ""
    threat-mappings:
      - reference-id: CCC.<Service>.Threats
        entries:
          - reference-id: CCC.<Service>.TH<nn>
    guideline-mappings:
      - reference-id: CCM
        entries:
          - reference-id: <framework-id>
```

## Groups

Every control must have a `group:` field. The group describes what security domain the entry belongs to, not what service it's part of.

| Group ID | Use When The Entry Is About... |
|---|---|
| `Encryption` | Cryptographic protection — encryption, key management, certificates |
| `Access` | Authentication, authorization, trust perimeters, least privilege |
| `Observability` | Logging, metrics, alerting, audit trail integrity |
| `Data` | Replication, backup, recovery, data retention, region restrictions |
| `Resource` | Resource lifecycle, scaling, cost management, tagging |
| `Compute` | Runtime execution, infrastructure configuration |
| `Ingestion` | Input validation, filtering, data ingestion controls |
| `Networking` | Network boundaries, endpoints, traffic exposure |
| `Orchestration` | Pipeline integrity, build automation, job coordination |
| `Processing` | ETL, data transformation, lineage |
| `Messaging` | Message delivery, ordering, retention |
| `MachineLearning` | Model governance, version pinning, red teaming |

**Decision rule:** Ask "what breaks if this control is missing?" and pick the group that matches the answer.
