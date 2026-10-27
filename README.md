# DS-A3 — Regression Baseline and Diagnostics

## Predicting adult Red Knot sex from morphometric data

**Author:** Kseniia Mamushkina  
**Student ID:** 2026A8010351001  
**Assignment:** DS-A3 — Regression Baseline and Diagnostics

## Research question

How accurately can the sex of adult Red Knots (*Calidris canutus canutus*) be predicted from wing, bill, and tarsus measurements?

The analysis compares individual measurements and their combinations, evaluates whether regularization improves prediction, and examines the limitations of the selected model. Because the response is binary, logistic regression is used.

## Dataset

The analysis uses measurements of adult Red Knots captured in Banc d’Arguin, Mauritania.

**Source:** Oortwijn et al. (2025), available through Dryad:  
https://doi.org/10.5061/dryad.3n5tb2rqb

**Source file:** `14._adults_Mauritania.csv`

Variables:

| Variable | Description |
|---|---|
| `Ring` | Individual identifier |
| `Sex` | DNA-based reference sex: M or F |
| `Wing` | Wing length, mm |
| `Bill` | Bill length, mm |
| `Tarsus` | Tarsus length, mm |
| `Year`, `Month` | Sampling year and month |

The original CSV contains 2,165 records. Both records for one individual with conflicting sex labels were excluded. For the remaining repeated individuals, only the earliest recorded capture was retained.

The analytical sample contains **2,130 unique birds**: 1,263 females and 867 males.

## Repository contents

| File or folder | Contents |
|---|---|
| `DS_A3_red_knot_sex.ipynb` | Analysis code, outputs, and interpretation |
| `DS_A3_Report.pdf` | Written report |
| `14._adults_Mauritania.csv` | Original source data |
| `tables/` | Exported comparisons, predictions, diagnostics, and split manifest |
| `figures/` | Residual diagnostic and learning curve figures |
| `requirements.txt` | Recorded Python package versions |
| `AI_USE_LOG.md` | Disclosure of AI assistance and verification |
| `README.md` | Project overview and reproduction instructions |

## Analysis design

### Sample splitting and leakage prevention

A stratified 80:20 split was created using `random_state=42`:

- **Training set:** 1,704 birds.
- **Test set:** 426 birds.
- **Overlapping individual identifiers:** 0.

Model comparison used five-fold stratified cross-validation within the training set. The same folds were used across candidates.

Standardization was performed inside each model pipeline. Regularization strength was selected using three-fold inner cross-validation within each outer training fold.

The test set was reserved for the final evaluation and was not used to select features, regularization strength, or the classification threshold.

### Candidate models

- A majority-class baseline.
- Unregularized logistic regression using each measurement separately and all combinations of measurements.
- Ridge and lasso logistic regression using `Wing + Bill` and `Wing + Bill + Tarsus`.

Balanced accuracy was the primary comparison metric. Accuracy and recall for each sex were also reported.

### Diagnostics

The selected model was examined using:

- Standardized and original-unit coefficients.
- Out-of-fold predictions and a confusion matrix.
- Residual patterns and probability-group summaries.
- Observations with the largest classification errors.
- Delete-one-observation influence analysis.
- Training and validation learning curves.

For a binary response, residual variance is expected to depend on predicted probability. This was interpreted using the Bernoulli variance relationship, \(p(1-p)\).

## Main results

The unregularized **Wing + Bill** model was retained. Adding tarsus length or regularization produced negligible observed improvements.

Its mean cross-validation accuracy was **77.35%**, and mean balanced accuracy was **75.86%**.

Final test results:

| Model | Accuracy | Balanced accuracy |
|---|---:|---:|
| Majority baseline | 59.39% | 50.00% |
| Wing + Bill | 73.71% | 71.83% |

Test confusion matrix:

| Reference sex | Predicted M | Predicted F |
|---|---:|---:|
| M | 107 | 66 |
| F | 46 | 207 |

Recall was **61.85% for males** and **81.82% for females**. The model therefore performed better for females, and its test performance was lower than the cross-validation estimates.

Individual observation deletion caused limited changes in predictions and coefficients. Learning curves showed a small training–validation gap, with little improvement over the examined range of training sample sizes.

## Interpretation and limitations

Wing and bill measurements contain useful information about reference sex, but substantial overlap between the sexes limits classification accuracy. Morphometric predictions should not be treated as a replacement for DNA-based sex determination.

The coefficients describe conditional associations and are not automatically causal effects.

The evaluation concerns adult birds from the sampled population and sampling context. It does not establish performance for juveniles, other populations or subspecies, or new locations and time periods.

## Reproduction instructions

The notebook was developed in Google Colab.

1. Download the notebook, original CSV, and `requirements.txt` from this repository.
2. Open `DS_A3_red_knot_sex.ipynb` in Google Colab.
3. Upload `14._adults_Mauritania.csv` through Colab’s **Files** panel.
4. Confirm that its runtime path is `/content/14._adults_Mauritania.csv`.
5. Run the notebook cells in order from top to bottom.

The CSV must be uploaded again when a new Colab runtime is started. It is not embedded in the notebook.

The notebook reads the source CSV from runtime storage. The `data/` folder in this repository preserves the source file for download.

### Environment

The recorded Colab environment used **Python 3.13.16**. Package versions are listed in `requirements.txt`.

To install the listed packages in a compatible Python environment:

```bash
pip install -r requirements.txt
```

The Python version comment in `requirements.txt` documents the interpreter version; it does not install Python.

The notebook contains Colab-specific paths and download commands. Running it outside Colab requires adapting these paths and commands.

### Data integrity

SHA-256 of the original CSV:

```text
6da0a6b44266f448393c92a293f24f145d803ee95b6246804a0185bbfd9869f6
```

The source CSV was preserved unchanged. Cleaning and individual selection were performed in code.

The saved split manifest is available at `results/tables/split_manifest.csv`.

## AI assistance

ChatGPT assisted with code drafting, report preparation, and notebook review. The scope of assistance, revised suggestions, and verification procedures are documented in `AI_USE_LOG.md`.
