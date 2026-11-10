# CMS-HCC V22 to V28 Risk Transition Simulator

A Microsoft Fabric analytical workflow for comparing CMS-HCC V22 and V28 risk scores using CMS DE-SynPUF sample claims data. The project moves source records through Bronze, Silver, and Gold layers, produces a member-level V22/V28 comparison, and evaluates a model that estimates the standardized V28 score from V22-era features.

> **Data boundary:** This repository contains code, a logical pipeline template, table definitions, and aggregate evaluation results only. It contains no source records, member identifiers, mapping files, credentials, Fabric workspace identifiers, or saved notebook outputs.

## What is included

| Area | Contents |
| --- | --- |
| `notebooks/` | Cleaned Fabric notebooks for ingestion, transformation, dimensional modeling, and risk-model analysis |
| `pipeline/` | A portable logical pipeline template with no tenant, workspace, or notebook IDs |
| `docs/data-model.md` | Gold-layer dimensions and facts, including table grain |
| `docs/data-lineage.md` | Bronze → Silver → Gold lineage and source-data boundary |
| `docs/ml-evaluation.md` | Saved aggregate evaluation evidence and limitations |
| `docs/architecture.md` | Publication and data-handling boundaries |

## Workflow

\`\`\`mermaid
flowchart LR
    A[Approved CMS DE-SynPUF sample files and versioned references] --> B[Bronze ingestion and validation]
    B --> C[Silver clinical transformations]
    C --> D[Gold dimensional model]
    D --> E[V22 to V28 risk analysis]
    E --> F[Aggregate transition and diagnostic facts]
\`\`\`

1. **Bronze** — validate and load beneficiary, inpatient, outpatient, GEM, and HCC mapping reference files as Delta tables.
2. **Silver** — standardize claim diagnosis evidence, map ICD-9 to ICD-10, apply V22 and V28 HCC mappings, and calculate member risk features.
3. **Gold** — publish member, model, and HCC dimensions plus member-risk and member-HCC facts.
4. **Analysis** — compare standardized scores and train/evaluate an XGBoost estimate of the V28 score.

## Model evidence captured from the supplied notebook

The notebook's member-isolated held-out test set had **17,288** rows. Its selected XGBoost model reported **R² 0.9085**, **MAE 0.3269**, and **RMSE 0.6925** when estimating the standardized V28 score. The notebook compared this with a V22-score baseline of RMSE **0.7207**, a reported **3.90%** RMSE reduction.

These are historical notebook outputs, not a claim of production performance. See [docs/ml-evaluation.md](docs/ml-evaluation.md) for the complete aggregate evidence and important limitations.

## Gold model

The Gold layer contains `dim_member`, `dim_model`, `dim_hcc`, `fact_member_risk`, and `fact_member_hcc`. It also publishes aggregate model-evaluation and transition-analysis facts. See [docs/data-model.md](docs/data-model.md).

## Running in Microsoft Fabric

1. Obtain approval to use only authorized, de-identified or synthetic data.
2. Configure the raw and reference paths in the Bronze notebook for your Lakehouse.
3. Attach a Lakehouse and execute the notebooks in numeric order.
4. Use `pipeline/hcc_v22_to_v28_pipeline.template.json` as a logical implementation guide; replace the notebook references with items in your own workspace.
5. Reconcile results against approved CMS reference logic before any operational use.

## Scope and limitations

This is an analytical comparison project, not clinical, coding, reimbursement, eligibility, or payment advice. The provided workflow is based on CMS DE-SynPUF sample data and needs governance, version-controlled reference mappings, testing, fairness review, and independent validation before any real-world decision use.

## Runtime

Designed for Microsoft Fabric Spark. See [requirements.md](requirements.md) for packages referenced by the supplied notebooks.

