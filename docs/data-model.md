# Gold data model

The Gold model separates reusable member, model, and HCC definitions from facts at a defined analytical grain. It is designed for comparison and reporting, not as a source of payment determination.

\`\`\`mermaid
erDiagram
    dim_member ||--o{ fact_member_risk : has
    dim_model ||--o{ fact_member_risk : scores
    dim_member ||--o{ fact_member_hcc : has
    dim_model ||--o{ fact_member_hcc : classifies
    dim_hcc ||--o{ fact_member_hcc : identifies
\`\`\`

| Table | Grain | Purpose | Key fields |
| --- | --- | --- | --- |
| `dim_member` | One row per member | Analytical member attributes | `member_key`, `reference_year`, `age`, `age_band`, `gender`, `chronic_condition_count` |
| `dim_model` | One row per HCC model version | V22/V28 model reference | `model_key`, `model_name`, `model_version` |
| `dim_hcc` | One row per HCC code and model version | HCC vocabulary | `hcc_key`, `model_key`, `hcc_code`, `hcc_description` |
| `fact_member_risk` | One row per member and model | Standardized risk score and risk tier | `member_key`, `model_key`, `risk_score_standardized`, mapped/payment HCC counts, `risk_tier`, `scoring_status` |
| `fact_member_hcc` | One row per member, model, and HCC code | Diagnosis-derived HCC assignments before hierarchy | `member_key`, `model_key`, `hcc_key`, `assignment_stage` |

## Diagnostic facts

The analysis notebook writes aggregate model diagnostics and transition outputs such as validation/test metrics, prediction diagnostics, transition groups, and direction metrics. These tables should contain only approved, governed analytical outputs in a real workspace.

