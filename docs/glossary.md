# Glossary

| Term | Short explanation |
|---|---|
| Pharmacogenomics | Study of how a person's genetic variants change the response to drugs. |
| Variant | A position in the DNA where a person differs from the reference (here written as chromosome, position, reference allele, alternate allele). |
| Allele | One of the possible letters (A, C, G, T) at a variant position. |
| Consequence | What a variant does to the gene, e.g. `missense_variant`, `intron_variant`. |
| Efficacy / Toxicity | How well the drug works / how harmful it is. |
| PK / PD | Pharmacokinetics (what the body does to the drug: absorption, metabolism) / pharmacodynamics (what the drug does to the body). |
| Dosage | The dose that is needed changes. |
| Data leakage | Information from the test set (or the label) reaching the features, which inflates the score. See `data_leakage_audit.md`. |
| Baseline | A trivial model (always the most frequent class) used as the minimum to beat. |
| Weighted vs macro F1 | Weighted F1 averages classes by size (dominated by big classes); macro F1 averages them equally (shows rare-class failures). |
