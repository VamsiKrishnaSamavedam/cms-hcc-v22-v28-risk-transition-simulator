# Data lineage

## Source boundary

The supplied Bronze notebook expects approved CMS DE-SynPUF sample beneficiary, inpatient, and outpatient claims files, plus ICD-9-to-ICD-10 GEM and HCC mapping reference files. None of those files are included in this repository.

## Bronze to Silver to Gold

| Layer | Purpose | Principal tables |
| --- | --- | --- |
| Bronze | Preserve validated source extracts and raw reference mappings | `bronze_beneficiary_2008`, `bronze_beneficiary_2009`, `bronze_beneficiary_2010`, `bronze_inpatient_claims`, `bronze_outpatient_claims`, raw HCC mappings, raw GEM |
| Silver | Standardize clinical evidence and calculate member-level HCC features | `silver_claim`, `silver_icd9_icd10_gem`, `silver_icd9_icd10_canonical`, V22/V28 HCC mappings, `silver_diagnosis_hcc`, `silver_member`, `silver_member_hcc_profile`, `silver_member_risk_scores`, `silver_member_risk_final` |
| Gold | Publish reporting-ready dimensions, risk facts, and aggregate model diagnostics | `dim_member`, `dim_model`, `dim_hcc`, `fact_member_risk`, `fact_member_hcc`, model-evaluation and transition facts |

## Processing logic

\`\`\`mermaid
flowchart TD
    R[Authorized sample claims and reference files] --> B[Bronze validation]
    B --> S1[Standardize member and claim evidence]
    B --> S2[Canonical ICD-9 to ICD-10 mapping]
    S1 --> S3[Diagnosis-to-HCC mapping]
    S2 --> S3
    S3 --> S4[V22 and V28 member risk features]
    S4 --> G[Gold dimensions and facts]
    G --> M[Transition analysis and aggregate diagnostics]
\`\`\`

The notebook is responsible for recording the exact source-file versions, cohort filters, and reference-mapping versions for any reproducible run.

