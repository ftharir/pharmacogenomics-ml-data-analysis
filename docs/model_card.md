# Model card - tuned SVM (RBF)

| | |
|---|---|
| Task | 12-class prediction of the effect of a genetic variant on a drug |
| Model | `sklearn.svm.SVC`, best of a grid: `kernel=rbf`, `C=10`, `gamma=scale` |
| Input | encoded drug, gene, consequence statistics (from training rows only) and the two alleles, standardised |
| Training data | 2,306 rows (70 % of the 3,295 unique rows) |
| Test data | 989 rows |
| Test accuracy / weighted F1 / macro F1 | 0.422 / 0.417 / 0.29 |
| Baseline accuracy | 0.345 |

**Intended use:** learning and portfolio demonstration of a classification workflow and of finding and removing data leakage.
**Not for:** clinical decisions or any real prescribing. The data origin is unverified, the rare classes are essentially unpredictable, and the model is only modestly better than the baseline.
**Per-class results:** [`reports/tables/classification_report_tuned_svm.csv`](../reports/tables/classification_report_tuned_svm.csv).
