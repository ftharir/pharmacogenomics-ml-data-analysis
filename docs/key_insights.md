# Key insights from the data analysis

All numbers use the 3,295 unique rows (exact duplicates removed). Tables are in [`../reports/tables`](../reports/tables), plots in [`../reports/figures`](../reports/figures).

1. **Most records are about efficacy.** Efficacy 55 %, PK 27 %, Dosage 14 %; Toxicity (2 %), Other (2 %) and PD (0.4 %) are rare. Overall 60 % of the effects are `increased` and 40 % `decreased`.

2. **Some drugs and genes almost fix the kind of effect** (`08_effect_kind_by_gene_and_drug.png`).
   * Warfarin: 88 % of its records are about dosage; the same holds for acenocoumarol (93 %) and phenprocoumon (90 %), and for the gene VKORC1 (85 %), all part of the anticoagulant story.
   * Duloxetine: 100 % efficacy (54 records); docetaxel, risperidone and gemcitabine about 80 % efficacy.
   * Tacrolimus (64 %) and efavirenz (70 %) are mostly about pharmacokinetics.

3. **Toxicity is rare and concentrated.** The highest toxicity share among drugs with at least 20 records is nevirapine (10 of 29); among genes it is DPYD (4 of 23) and TYMS (4 of 25). See `top_toxicity_share_drugs_genes.csv`.

4. **Reports of the same variant and drug often disagree.** 347 of 2,775 variant-drug pairs (12.5 %) carry more than one effect, and in 220 of them the same kind of effect is reported in both directions; the biggest case is `decreased-Efficacy` vs `increased-Efficacy` (104 pairs) (`09_conflicting_effect_pairs.png`). This is label noise that no model can resolve.

5. **Drug and gene carry the signal, the variant consequence almost none.** Cramér's V with the kind of effect is 0.51 for the drug, 0.42 for the gene and only 0.08 for the consequence (`10_association_with_effect.png`). The direction of the effect is harder to link to any single column (0.28 / 0.37 / 0.10), and the share of `increased` varies only between 54 % and 67 % across consequences (`11_direction_by_consequence.png`).

**What this means for the model:** predicting the *kind* of effect is feasible from drug and gene, predicting the *direction* is not, which matches the model's most frequent mistake (efficacy up vs down) and explains why accuracy stays around 42 %.

*Caveat:* Cramér's V is bias-corrected but drugs and genes have many categories with few rows, so the values are approximate.
