# Reports

Everything here is produced from the data or from the printed results of the main notebook.

## figures/
| File | Shows |
|---|---|
| `01_class_distribution.png` | Number of records per side-effect class |
| `02_top_drugs_and_genes.png` | The 12 most frequent drugs and genes |
| `03_variant_consequences.png` | Most frequent variant consequence types |
| `04_duplicates_and_ambiguity.png` | Share of duplicate rows and of variant-drug pairs with several effects |
| `05_score_after_each_fix.png` | Test accuracy from the first version to the final model |
| `06_confusion_matrix_final.png` | Confusion matrix of the final tuned SVM |
| `07_per_class_f1.png` | F1 per class with the test support |

## tables/
| File | Content |
|---|---|
| `classification_report_tuned_svm.csv` | Precision, recall, F1 and support per class (final model) |
| `class_distribution.csv` | Records and share per class |
| `top20_drug.csv`, `top20_gene.csv`, `top20_conequence.csv` | The 20 most frequent values of each column |

Figures 01-04 and the three `top20` tables describe the raw data (all 4,698 rows, duplicates included); figures 05-07 and the classification report describe the final model.
