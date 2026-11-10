# ML evaluation evidence

This document reports only aggregate results saved in the supplied risk-analysis notebook. Notebook outputs containing environment information were removed from the published copy.

## Evaluation setup

| Item | Value recorded in notebook |
| --- | --- |
| Analytical rows | 116,352 |
| Features | Age, sex indicators, chronic-condition count, V22 HCC counts, and standardized V22 risk score |
| Split approach | Member-isolated train/validation/test split |
| Training rows | 81,387 |
| Validation rows | 17,677 |
| Test rows | 17,288 |
| Selected model | XGBoost |

## Held-out test results

| Metric | XGBoost estimate of standardized V28 score | V22-score baseline |
| --- | ---: | ---: |
| R² | 0.9085 | Not reported |
| MAE | 0.3269 | Not reported |
| RMSE | 0.6925 | 0.7207 |

The notebook reported a 3.90% RMSE reduction compared with the V22-score baseline and 64.61% directional accuracy. A threshold of 0.9900 was used to flag large score transitions.

## Interpretation limits

- These results came from sample-data notebook execution and are not production validation.
- The model estimates a standardized V28 score from V22-era features; it does not determine official payment, coding, eligibility, or clinical care.
- Any deployment needs independent validation, data-quality monitoring, governance review, protected-attribute assessment, versioned CMS references, and approval by the responsible organization.

