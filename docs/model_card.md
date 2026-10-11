# Model card - tuned SVM (RBF)

| | |
|---|---|
| Task | 12-class prediction of the effect of a genetic variant on a drug |
| Model | `sklearn.svm.SVC`, best of a grid: `kernel=rbf`, `C=100`, `gamma=scale` |
| Input | encoded drug, gene, consequence statistics and the two alleles (standardised) |
| Training data | 3,288 rows (70 % of 4,698) |
| Test data | 1,410 rows |
| Test accuracy / weighted F1 / macro F1 | 0.512 / 0.504 / 0.32 |
| Baseline accuracy | 0.323 |

**Intended use:** learning and portfolio demonstration of a classification workflow and of finding data leakage.
**Not for:** clinical decisions or any real prescribing. The data origin is unverified, rare classes are essentially unpredictable, and the score is probably still somewhat optimistic because of duplicate rows.
**Per-class results:** [`reports/tables/classification_report_tuned_svm.csv`](../reports/tables/classification_report_tuned_svm.csv).
