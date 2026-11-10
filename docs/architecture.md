# Architecture and release checklist

## Logical design

The proposed design keeps raw source data inside an approved Fabric workspace. Only de-identified, aggregated transition summaries should be exported for reporting or publication.

\`\`\`mermaid
flowchart TD
  R[Approved source] --> D[De-identification and quality checks]
  D --> L[Lakehouse curated tables]
  L --> V22[V22 scoring path]
  L --> V28[V28 scoring path]
  V22 --> C[Comparison controls]
  V28 --> C
  C --> A[Aggregate reporting]
\`\`\`

## Required controls

- Version-control rule tables and effective dates.
- Keep member identifiers, workspace identifiers, secrets, and raw extracts out of Git.
- Document scoring assumptions and reconciliation thresholds.
- Obtain domain review before using any results in a business decision.

## Publication checklist

- [ ] Add notebook or pipeline files.
- [ ] Add a synthetic sample or a reproducible data-access procedure.
- [ ] Add a data dictionary.
- [ ] Add automated tests for versioned mappings.
- [ ] Add evaluated metrics only after validation.
- [ ] Review repository history for sensitive data before pushing.
