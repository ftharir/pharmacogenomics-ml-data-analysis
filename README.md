# Pharmacogenomics ML Data Analysis

Data analysis and machine-learning classification of **drug side effects / response from genetic variants**.
Given a variant, its gene, the drug and the variant's molecular consequence, an SVM predicts one of 12 effect classes
(for example `increased-Efficacy`, `decreased-Dosage`, `increased-PK`).

> **Timeline.** Worked on between **26 April and 11 May 2025** (dates of the original files), as a research-style task.
> In **October 2026** I reviewed it, found a data-leakage problem in my own pipeline, fixed it with
> a few small edits and added the figures and notes in this repository. The notebook itself is my original work.

## Contents

| File / folder | What it is |
|---|---|
| [`task.ipynb`](task.ipynb) | **Main notebook (my own code):** EDA, feature encoding, SVM (linear), GridSearchCV, evaluation, baseline comparison. Comments and text are partly in Persian. |
| [`practice.ipynb`](practice.ipynb) | Warm-up notebook: SVM on the iris dataset. |
| [`genetest.ipynb`](genetest.ipynb) | Empty scratch notebook, kept as it was. |
| [`VarDrug.csv`](VarDrug.csv) | The data set: 4,698 variant-drug records (columns `VariantID`, `Conequence` [sic], `Gene`, `Drug`, `SideEffect`). |
| [`figures/`](figures) | Plots made from the data and from the results (below). |
| [`docs/data_leakage.md`](docs/data_leakage.md) | What was wrong in the first version and exactly what changed. |

## The data

* 4,698 rows, no missing values, **12 classes**, 406 drugs, 605 genes, 21 consequence types.
* **Strong class imbalance:** the biggest class (`increased-Efficacy`) holds about 32 % of the rows, the smallest ones only a handful.
* **1,403 rows (30 %) are exact duplicates**, and 12.5 % of the variant-drug pairs appear with more than one effect.

![classes](figures/01_class_distribution.png)
![drugs and genes](figures/02_top_drugs_and_genes.png)
![duplicates](figures/04_duplicates_and_ambiguity.png)

## Method (as in `task.ipynb`)

1. Exploration: types, value counts, missing values, distributions.
2. Features: frequency encoding of the target, per-category target statistics for `Drug`, `Gene` and `Conequence`, label encoding of the alleles parsed from `VariantID`.
3. 70/30 train/test split (`random_state=42`), standardisation, linear SVM, then GridSearchCV over `C`, `kernel` and `gamma` (5-fold).
4. Evaluation: accuracy / weighted precision / recall / F1, confusion matrix, comparison with a most-frequent-class baseline.

## Results and the data-leakage fix

My first version reported **59.3 % accuracy**. That number was too optimistic: part of the information about the test labels
had reached the features. After the fix the same pipeline gives:

| Model | Test accuracy | Weighted F1 |
|---|---|---|
| Baseline (always the most frequent class) | 0.323 | 0.158 |
| SVM, first version (with leakage) | 0.593 | 0.503 |
| **SVM, after the fix (best of the grid: RBF, C=100)** | **0.512** | **0.504** |

![before and after](figures/05_before_after_leakage_fix.png)

The model still clearly beats the baseline, but the honest figure is about **51 %**, not 59 %. Details and the three
one-line changes are in [`docs/data_leakage.md`](docs/data_leakage.md).

### Known limitations

* The 30 % duplicate rows are still in the data, so identical rows can fall into both train and test; scores remain somewhat optimistic.
* Rare classes (fewer than 15 test rows) are almost never predicted correctly; macro-averaged scores would be much lower than the weighted ones above.
* Only variant-level categorical information is used, so the model learns mostly drug/gene statistics, not biology.

## Run it

```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
jupyter notebook task.ipynb
```

The notebook reads `VarDrug.csv` from the same folder.

## Data and licence

`VarDrug.csv` was provided to me for a research task; its original source is not documented. It is included only so the analysis can be reproduced.
If you know the source or hold rights to it and want it credited or removed, please open an issue.
