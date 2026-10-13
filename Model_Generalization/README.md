# sCCA Model Generalization Validation on an Independent Dataset

This project validates the generalizability of sparse canonical correlation analysis (sCCA) weights learned in a training phase by applying them to an independent dataset: it computes brain and behavior (CBCL) canonical scores and tests their correlation per dimension, including Benjamini-Hochberg (BH) FDR correction.

## Analysis pipeline

1. **Load the independent validation data**: brain structural features (`brain_aligned.csv`) and CBCL behavioral data (`CBCL_aligned.csv`). The order of brain regions and CBCL items must match the sequence in the provided files.
2. **Load training-phase model parameters**: standardization parameters (means/SDs), CBCL regression models, bootstrap stable features, and the sCCA candidate features.
3. **Remove covariate effects**: predict CBCL with the trained regression models and take residuals, then standardize.
4. **Extract loadings**: take the sCCA `u` (brain) / `v` (behavior) weights, zero out all features that are not bootstrap-stable, and align signs.
5. **Compute canonical scores**: brain score = standardized brain features × brain loadings; behavior score = standardized residuals × behavior loadings.
6. **Correlation tests per dimension**: Pearson correlation via `cor.test`, extracting `r`, 95% CI, and raw `p`, then BH-FDR correction, summarized as `res_df`.

## Directory layout

```
Model_Generalization/
├── Model_Generalization.Rmd   # main analysis script (R Markdown)
├── README.md
├── Independent_Data_Input/    # independent validation data (CSV)
│   ├── brain_aligned.csv
│   └── CBCL_aligned.csv
└── Model_Parameters/          # training-phase model parameters (RData)
    ├── my_center.RData / my_scale.RData        # brain standardization params
    ├── cbcl_center.RData / cbcl_scale.RData    # CBCL standardization params
    ├── CBCL_trained_models.RData               # CBCL regression models
    ├── u_boot_plot2000.RData / v_boot_plot2000.RData  # bootstrap stable features
    └── scca_cand.RData                         # sCCA candidate result (u/v)
```

## Usage

1. Open `Model_Generalization.Rmd`.
2. **No path editing needed**: all paths are resolved relative to the Rmd file's own directory (the project root), so the whole repository can be cloned or moved anywhere and run as-is.
3. Knit in RStudio (produces HTML), or from the command line:

   ```r
   rmarkdown::render("Model_Generalization.Rmd")
   ```

After a successful run, the console/document prints the result table `res_df` (dimension, correlation, CI, raw and FDR-corrected p-values).



