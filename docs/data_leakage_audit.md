# Data leakage: what was wrong and what changed

Data leakage means the model sees, during training or evaluation, information it would not have for a genuinely new example. The test score then looks better than the real performance.

## Problems found in the first version of the main notebook

| # | Problem | Why it leaks |
|---|---|---|
| 1 | `SideEffect_encoded` (the frequency of the label) stayed in `X` | It is a number that identifies the class, so the model is practically given the answer. |
| 2 | `Drug_encoded`, `Gene_encoded`, `Conequence_encoded` were computed on the whole table before the train/test split | Test labels contributed to the test features. |
| 3 | `scaler.fit_transform(X_test_numeric)` | The scaler was fitted again on the test data instead of reusing the training statistics. |
| 4 | About 30 % of the rows (1,403) are exact duplicates | A copy in the test set whose twin is in the training set is a test on data the model has already seen. |

## Changes made (all other cells are untouched)

1. `X = data.drop(columns=['SideEffect', 'SideEffect_encoded'])` - the label-derived column is no longer a feature.
2. One new code cell right after `train_test_split` recomputes the three `*_encoded` columns **from the training rows only** and maps them onto the test rows (unseen categories get the training mean).
3. `scaler.transform(X_test_numeric)` instead of `fit_transform`.
4. One new code cell before the features are built drops the duplicate rows (`drop_duplicates` on the five original columns), leaving 3,295 unique rows.
5. The data path was changed from an absolute local path to `../data/raw/VarDrug.csv`, and the notebook was re-run so that the printed outputs belong to the corrected code. (One early cell, `data.apply(pd.Series.value_counts)`, fails on current pandas versions, so its original output was kept.)

## Effect

| | Accuracy | Weighted F1 |
|---|---|---|
| Original (leaking) | 0.593 | 0.503 |
| After changes 1-3 | 0.512 | 0.504 |
| After changes 1-4 (final) | **0.422** | **0.417** |
| Baseline on the final split (most frequent class) | 0.345 | 0.177 |

Still not addressed: the category statistics inside the training set are computed on the same rows the model trains on (mild over-fitting, not leakage into the test set), and only one random split is used. Both are listed in the README.
