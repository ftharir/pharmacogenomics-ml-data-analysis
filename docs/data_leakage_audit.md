# Data leakage: what was wrong and what changed

Data leakage means the model sees, during training or evaluation, information it would not have for a genuinely new example. The test score then looks better than the real performance.

## Problems found in the first version of the main notebook

| # | Problem | Why it leaks |
|---|---|---|
| 1 | `SideEffect_encoded` (the frequency of the label) stayed in `X` | It is a number that identifies the class, so the model is practically given the answer. |
| 2 | `Drug_encoded`, `Gene_encoded`, `Conequence_encoded` were computed on the whole table before the train/test split | Test labels contributed to the test features. |
| 3 | `scaler.fit_transform(X_test_numeric)` | The scaler was fitted again on the test data instead of reusing the training statistics. |

## Changes made (all other cells are untouched)

1. `X = data.drop(columns=['SideEffect', 'SideEffect_encoded'])` - the label-derived column is no longer a feature.
2. One new code cell right after `train_test_split` recomputes the three `*_encoded` columns **from the training rows only** and maps them onto the test rows (unseen categories get the training mean).
3. `scaler.transform(X_test_numeric)` instead of `fit_transform`.
4. The data path was changed from an absolute local path to `../data/raw/VarDrug.csv`, and the notebook was re-run so that the printed outputs belong to the corrected code. (One early cell, `data.apply(pd.Series.value_counts)`, fails on current pandas versions, so its original output was kept.)

## Effect

| | Accuracy | Weighted F1 |
|---|---|---|
| Original (leaking) | 0.5929 | 0.5033 |
| After the fix | 0.5121 | 0.5035 |
| Baseline (most frequent class) | 0.3227 | 0.1575 |

Not changed on purpose: the 30 % exact-duplicate rows (a duplicate can still land in both train and test) and the in-sample target statistics inside the training set. Both are listed as limitations in the README.
