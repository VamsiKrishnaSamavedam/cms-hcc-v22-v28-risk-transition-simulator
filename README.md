# CMS-HCC V22 to V28 Risk Transition Simulator

Microsoft Fabric project scaffold for analyzing how member risk scores and payment-model categories may change when a population is evaluated under CMS-HCC V22 and V28.

## Current status

Repository structure and documentation are ready for publication. No source notebook, mapping tables, cohort extract, score outputs, or validation results were supplied with the materials reviewed on November 10, 2026. This repository intentionally makes no claims about model accuracy, savings, score deltas, or member outcomes.

## Intended workflow

1. Load a de-identified member cohort and diagnosis evidence.
2. Apply versioned V22 and V28 mapping/rule logic.
3. Produce member-level score and category transitions.
4. Aggregate transition counts and score deltas.
5. Validate against an approved reference implementation before reporting results.

## Architecture

\`\`\`mermaid
flowchart LR
  A[De-identified member and diagnosis data] --> B[Fabric Lakehouse]
  B --> C[V22 scoring rules]
  B --> D[V28 scoring rules]
  C --> E[Member-level comparison]
  D --> E
  E --> F[Transition and impact reporting]
\`\`\`

See docs/architecture.md for data-handling boundaries and the publication checklist.

## What must be added before an analytical release

- Fabric notebook(s) or pipeline definition
- Authorized, versioned V22/V28 mapping and scoring references
- Data dictionary and cohort inclusion/exclusion logic
- De-identification and disclosure-control review
- Test cases, reconciliation evidence, and approved results

## Safety and scope

Use only approved, de-identified or synthetic data. This repository is for analytical comparison and is not clinical, reimbursement, coding, or eligibility advice.

## Environment

The intended runtime is Microsoft Fabric Spark. No external Python packages were verified from the provided materials; see requirements.md.
