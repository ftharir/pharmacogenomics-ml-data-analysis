# Methodology

## 1. Problem
Multi-class classification: given a genetic variant (`VariantID`), its consequence, the gene and the drug, predict the reported effect (`SideEffect`, 12 classes = direction x kind).

## 2. Data
4,698 variant-drug records (see [`data/README.md`](../data/README.md)). Main properties: 12 imbalanced classes, 30 % exact duplicates, 12.5 % ambiguous variant-drug pairs.

## 3. Pipeline (notebook `01_side_effect_classification_svm`)
| Step | What is done |
|---|---|
| Exploration | dtypes, missing values, value counts per column, distributions |
| Parsing | `VariantID` -> reference and alternate allele |
| Encoding | frequency encoding of the label; per-category label statistics for `Drug`, `Gene`, `Conequence`; label encoding of alleles |
| Split | 70 % train / 30 % test, `random_state=42` |
| Leakage guard | the category statistics are recomputed from training rows only; the label-derived column is not a feature |
| Scaling | `StandardScaler` fitted on train, applied to test |
| Models | linear SVM, then GridSearchCV (`C` in 0.1-100, kernel linear/rbf, gamma scale/auto, 5-fold, accuracy) |
| Evaluation | accuracy, weighted precision / recall / F1, confusion matrix, comparison with a most-frequent-class baseline |

## 4. Choice of metrics
Accuracy and weighted F1 are dominated by the large classes. Macro F1 (0.32 for the tuned model) shows how weak the rare classes are, so it should be read together with the weighted scores.

## 5. Threats to validity
* exact duplicates can appear in both train and test;
* one random split only (no repeated or grouped cross-validation);
* the tiny classes (`PD`, `Other`, `Toxicity`) have too few examples for a reliable estimate;
* the origin of the data is unknown, so label noise cannot be assessed.

## 6. Possible next steps
Deduplicate before splitting, group the split by drug or by variant to test generalisation to new drugs, use stratified repeated CV, merge the rare classes, and compare with tree-based models and richer gene/drug descriptors.
