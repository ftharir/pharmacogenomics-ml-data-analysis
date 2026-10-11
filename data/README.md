# Data dictionary

`raw/VarDrug.csv` - 4,698 rows x 6 columns, used unchanged.

| Column | Type | Example | Meaning |
|---|---|---|---|
| `Unnamed: 0` | int | 0 | Leftover row index from a previous export (not a feature). |
| `VariantID` | str | `1_97915614_C_T` | `chromosome_position_reference-allele_alternate-allele`. The notebook splits it into the two alleles. |
| `Conequence` | str | `missense_variant` | Molecular consequence of the variant (the header is misspelled in the source file and kept as is). 21 distinct values. |
| `Gene` | str | `DPYD` | Affected gene. 605 distinct values. |
| `Drug` | str | `fluorouracil` | Medication. 406 distinct values. |
| `SideEffect` | str | `increased-Toxicity` | **Target.** Direction (`increased` / `decreased`) + kind of effect (`Efficacy`, `PK`, `Dosage`, `Toxicity`, `PD`, `Other`) = 12 classes. |

Quality notes: no missing values; 1,403 exact duplicate rows (30 %); 12.5 % of the variant-drug pairs are labelled with more than one effect; the largest class holds about 32 % of the rows (see `reports/figures`).

Source and licence of the table are not documented (see the README).
