# Findings

Numbers are taken from the printed outputs of `notebooks/01_side_effect_classification_svm.ipynb` (70/30 split, `random_state=42`, 1,410 test rows).

| Model | Accuracy | Weighted F1 | Macro F1 |
|---|---|---|---|
| Baseline (most frequent class) | 0.323 | 0.158 | - |
| Linear SVM (fixed) | 0.481 | 0.45 | 0.21 |
| Tuned SVM, RBF, C=100 (fixed) | **0.512** | **0.504** | 0.32 |
| Tuned SVM in the first version (leaking) | 0.593 | 0.503 | - |

1. **The model learns something real**, about 19 points above the baseline, mainly for the large classes (`increased-Efficacy` F1 0.69, `decreased-Dosage` 0.62).
2. **Rare classes are weak.** `decreased-Other`, `decreased-PD`, `increased-Other` and `increased-Toxicity` have F1 around 0 because they have fewer than 15 test examples (`reports/figures/07_per_class_f1.png`).
3. **The most common confusion** is between `decreased-Efficacy` and `increased-Efficacy` (about 85 test rows), i.e. the model often gets the kind of effect right but the direction wrong (`reports/figures/06_confusion_matrix_after_fix.png`).
4. **Leakage inflated the first score** from about 0.51 to 0.59; the weighted F1 stayed the same because the extra accuracy came from the big classes.
5. **Open points / next steps:** remove or group duplicate rows before splitting, use stratified cross-validation, report macro-F1, merge the tiny classes, and try gradient boosting or richer gene/drug features.
