# Findings

Numbers are taken from the printed outputs of `notebooks/01_side_effect_classification_svm.ipynb` (duplicates removed, 70/30 split, `random_state=42`; 2,306 training and 989 test rows).

## Final results

| Model | Accuracy | Weighted F1 | Macro F1 |
|---|---|---|---|
| Baseline (most frequent class) | 0.345 | 0.177 | - |
| Linear SVM | 0.41 | 0.38 | 0.20 |
| **Tuned SVM, RBF, C=10** | **0.422** | **0.417** | **0.29** |

## How the score changed as leakage was removed

| Stage | Accuracy |
|---|---|
| First version (label-derived feature, encodings on all data, scaler fitted on test) | 0.593 |
| After removing those three leaks | 0.512 |
| After also removing duplicate rows before the split | **0.422** |

1. **The model still learns something real:** about 8 points above the baseline, mainly on the large classes (`increased-Efficacy` F1 0.60, `decreased-Efficacy` 0.42, `increased-PK` 0.40).
2. **A large part of the first score was leakage and memorised duplicates.** The drop from 0.593 to 0.422 is the honest correction; the real skill is modest.
3. **Rare classes are not learnable here.** The `Other`, `Toxicity` and `PD` classes have 2-19 test rows each and F1 between 0 and 0.12 (the 0.50 and 0.67 of the two `PD` classes rest on 2 and 3 rows, so they mean nothing) - see `reports/figures/07_per_class_f1.png`.
4. **The most common confusion** is between `decreased-Efficacy` and `increased-Efficacy` (63 + 107 test rows): the kind of effect is often right but the direction wrong (`reports/figures/06_confusion_matrix_final.png`).
5. **Next steps:** group the split by drug to measure generalisation to new drugs, stratified repeated cross-validation, merging the tiny classes, gradient boosting and richer gene/drug features.
