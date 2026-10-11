# Notebooks

| Notebook | Content |
|---|---|
| [`00_svm_warmup_iris.ipynb`](00_svm_warmup_iris.ipynb) | Warm-up exercise: an SVM on the iris data set, before the main task. |
| [`01_side_effect_classification_svm.ipynb`](01_side_effect_classification_svm.ipynb) | **Main analysis:** exploration, feature encoding, duplicate removal, train/test split, linear SVM, `GridSearchCV`, evaluation and baseline comparison. |

**Order:** read `00` first if you want the basics, then `01`.

**Notes**
* Markdown text and comments are partly in Persian.
* `01` reads `../data/raw/VarDrug.csv`, so open it from this folder (Jupyter does this by default). The grid search takes a few minutes.
* The data-leakage corrections are marked in the notebook with short bilingual notes; see [`../docs/data_leakage_audit.md`](../docs/data_leakage_audit.md).
* One early cell (`data.apply(pd.Series.value_counts)`) fails on current pandas, so its original saved output was kept.
