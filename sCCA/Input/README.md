# Input File Format for sCCA

This directory contains the two CSV input files used by the sCCA analysis. `brain_aligned.csv` provides brain structural features, and `CBCL_aligned.csv` provides CBCL behavioral symptom scores and the covariates required by the analysis. The values in the current files are randomly generated examples for illustrating the file format; they do not represent real participants or analysis results. Replace them with real data before conducting an analysis.

## General Requirements

- Each row represents one participant, and the first row contains the column names. The files are comma-separated.
- Both files must contain the same participants, with `participant_id` values in exactly the same row order. The analysis pairs rows by position and does not automatically sort or merge records by ID.
- `participant_id` values should be non-empty and unique. Except for the ID column, features and scores used in the analysis should be numeric. Do not include units, explanatory text, or inconsistently formatted missing values in these columns.
- Use consistent column names in both files. Do not use participant IDs as analysis features.

## `brain_aligned.csv`

- The first column is `participant_id`.
- All subsequent columns are numeric brain structural features. Each column represents a brain region or structural measure, such as the example column `L_bankssts_thickavg`. Column names must be unique, and rows must correspond one-to-one with those in the CBCL file.
- `sCCA.Rmd` uses every column except `participant_id` as a brain feature in the model. Do not include non-brain features or non-numeric fields in these columns.

Example:

```csv
participant_id,L_bankssts_thickavg,...
participant001,numeric_value,...
participant002,numeric_value,...
```

## `CBCL_aligned.csv`

- `participant_id`: Participant identifier. It must match the identifier in the brain structure file.
- `p_factor_mcelroy_harmonized_all_samples`: Numeric p-factor score used to stratify the train/test splits and bootstrap resampling. This column is not used as a CBCL symptom feature in sCCA.
- `CBCL_...`: CBCL item score columns, such as `CBCL_01` and `CBCL_56A`. Each column represents one item and must be numeric. The example retains the item names used in the supplied file; use the item columns appropriate for your data version rather than renaming columns simply to fill gaps in the numbering.
- `sex` and `age`: Covariate columns required to run the current `sCCA.Rmd`. `sex` is treated as a categorical variable, and `age` should be numeric. The script uses them to regress covariate effects out of each CBCL item.

**Note:** The current randomly generated example does not include `sex` or `age`, but `sCCA.Rmd` reads both columns. Running the analysis directly with this example will therefore fail during covariate preparation. Add these columns to the real CBCL input file and ensure their rows follow the same participant order as the brain structure file.

Example:

```csv
participant_id,p_factor_mcelroy_harmonized_all_samples,sex,age,CBCL_01,...
participant001,numeric_value,category,numeric_value,score,...
participant002,numeric_value,category,numeric_value,score,...
```

## Pre-run Checklist

1. Confirm that both CSV files contain the same number of participants and that `participant_id` values appear in exactly the same order.
2. Confirm that the CBCL file contains the p-factor, `sex`, `age`, and the required CBCL item columns.
3. Confirm that all analysis columns other than the ID can be read as numeric where appropriate, and that there are no unhandled missing values.
4. Place the files in this directory and run `sCCA.Rmd` from the project root.
