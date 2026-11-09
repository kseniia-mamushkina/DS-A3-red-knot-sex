# AI Use and Verification Log

**Assignment:** DS-A3 — Regression Baseline and Diagnostics  
**Author:** Kseniia Mamushkina  
**AI tool:** ChatGPT, GPT-6.1 Sol, OpenAI

## Scope of AI assistance

ChatGPT generated draft Python code for data checks, sample preparation, splitting, baseline models, cross-validation, regularization tuning, diagnostics, and final test evaluation.

AI assistance also included explanations of statistical concepts, preparation of the PDF report, and review of the completed notebook. 

The author ran the analysis blocks in Google Colab, examined the outputs, and discussed methodological choices and interpretations.

## Accepted suggestions

- Retain the earliest recorded capture per bird to obtain one observation per individual.
- Use a stratified 80:20 training/test split and identical cross-validation folds across candidates.
- Place standardization inside each Pipeline and tune regularization within inner cross-validation.
- Report balanced accuracy alongside accuracy and examine recall separately for each sex.
- Retain the unregularized Wing + Bill model because additional predictors and regularization provided negligible observed gains.

## Suggestions revised or rejected

- The initial proposal to examine regularization only with all three predictors was revised.
- Ridge and lasso were evaluated for both Wing + Bill and Wing + Bill + Tarsus.

## Verification performed

1. **Author execution:** The author executed the analysis blocks in Colab and supplied their outputs for comparison.
2. **Separate AI-assisted execution:** The 15 analysis code cells in the reviewed notebook snapshot were executed sequentially in a separate runtime using the same CSV. Only filesystem paths were adapted.
3. **Version comparison:** The notebook recorded scikit-learn 1.6.1; the separate review used 1.8.0. The substantive numerical tables matched at the displayed precision.
4. **Data and leakage checks:** Verification confirmed 2,130 analytical individuals, a 1,704/426 training/test split, and no overlapping ring identifiers.
5. **Final results:** The reproduced test confusion matrix contained 107 correctly classified males and 207 correctly classified females, with 66 and 46 errors respectively. Test accuracy was 73.71% and balanced accuracy was 71.83%.
6. **Provenance:** The description of the adult dataset, DNA-based reference sex, and measurement units was checked against the Dryad metadata: https://doi.org/10.5061/dryad.3n5tb2rqb.

This verification checks execution and reproducibility. It is not an independently written implementation or an external statistical audit.

## Negative findings and data integrity

The report retains the negligible gains from additional complexity, lower test performance than cross-validation estimates, poorer male recall, and confident classification errors.

Observations were not removed merely because predictions disagreed with reference sex. Case deletion was used only for sensitivity analysis. Test results were reported without subsequent model retuning.

The original CSV was preserved unchanged. No synthetic or AI-generated observations were introduced.
