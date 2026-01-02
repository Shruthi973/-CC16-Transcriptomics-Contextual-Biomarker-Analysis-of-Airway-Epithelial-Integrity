# -CC16-Transcriptomics-Contextual-Biomarker-Analysis-of-Airway-Epithelial-Integrity
CC16-Transcriptomics is a probe-validated transcriptomic analysis project investigating CC16 (SCGB1A1 / uteroglobin) as a context-dependent airway epithelial biomarker using public bronchial epithelial gene-expression data. 

# ⬛ CC16 (SCGB1A1 / Uteroglobin) Signal Analysis in Bronchial Epithelial Samples


---

## PROJECT NAME
CC16 Signal in Bronchial Epithelial Samples

---

## PROJECT TYPE
Pilot Transcriptomic Analysis  
Bioinformatics + Clinical Phenotype Integration  

---

## DATA SOURCE (EXACT)
NCBI GEO Public Dataset

- GEO Accession: GSE37147
- Platform: Affymetrix Human Gene 1.0 ST Array
- Platform ID: GPL13243
- Tissue: Bronchial epithelial brushings
- Total samples in matrix: 269
- Total probes: 19,793

---

## FILES USED (AS PROVIDED)

- CC16_PROJECT_SCIENTIFIC_ANALYST_ROLE.ipynb  
- CC16_PROJECT_SCIENTIFIC_ANALYST_ROLE_1766972434.html  
- CC16 PART II - SIGNAL IN SAMPLES.html  
- CC16 Expression in Bronchial Epithelial Samples - Report.pdf  

All results, statistics, and plots originate ONLY from these files.

---

## PROJECT OBJECTIVE (EXACT)
To validate CC16 (SCGB1A1 / uteroglobin) probe identity on the Affymetrix platform, extract its transcriptomic signal from bronchial epithelial samples, perform quality control, and evaluate relationships with smoking status, COPD status, and lung-function measures (FEV1%).

---

## STEP-BY-STEP METHODS (NO GAPS)

---

### STEP 1 — Load GEO Series Matrix
- Loaded GSE37147 series matrix
- Confirmed dimensions:
  - Rows: 19,793 probes
  - Columns: 269 samples
- Retained all samples initially (no pre-filtering)

---

### STEP 2 — Platform Annotation Download & Parsing
- Downloaded GPL13243 annotation file
- Parsed annotation table into dataframe
- Did NOT rely on pre-attached gene symbols in matrix

---

### STEP 3 — Probe-to-Gene Validation (CRITICAL STEP)
Performed synonym-based search across annotation table using:

- SCGB1A1
- CC16
- uteroglobin
- secretoglobin

Results:
- SCGB1A1 → 0 direct hits
- CC16 → 0 direct hits
- uteroglobin → 1 hit
- secretoglobin → multiple hits

Filtered annotation rows containing “uteroglobin”.

Confirmed mapping:
- Gene: secretoglobin family 1A member 1 (uteroglobin)
- Probe ID: **7356_at**
- SPOT_ID: 7356

This probe was used for ALL downstream analysis.

---

### STEP 4 — CC16 Expression Extraction
- Filtered expression matrix to probe ID `7356_at`
- Converted expression values to numeric
- Stored as `cc16_expr` vector
- Expression scale: log-transformed microarray signal (as provided by GEO)

---

### STEP 5 — Distributional Quality Control
Computed descriptive statistics across all 269 samples:

- Mean: 12.28
- Median: 12.40
- Standard deviation: 0.39
- Range: 9.68 – 12.65

Distribution characteristics:
- Unimodal
- Right-skewed
- Biologically plausible epithelial expression pattern

Outlier handling:
- IQR method used to flag extremes
- Approximately 30 samples flagged
- **No samples removed**
- Extremes retained intentionally to preserve biological variability

---

### STEP 6 — Metadata Extraction from GEO Header
Extracted sample-level metadata fields:

- smoking_status
- copd_status (raw)
- age_years
- pack_years
- fev1_percent
- fev1_fvc
- delta_fev1

---

### STEP 7 — Metadata Cleaning & Harmonization

Type conversions:
- age_years → numeric
- pack_years → numeric
- fev1_percent → numeric
- fev1_fvc → numeric
- delta_fev1 → numeric

COPD grouping:
- Raw values mapped to:
  - "COPD"
  - "Control"
  - NA retained as NA

Smoking grouping:
- current smoker (CS)
- ex-smoker (EX)
- NA retained

No imputation performed.

---

### STEP 8 — Sample Availability Check

Smoking status:
- Ex-smoker (EX): 139
- Current smoker (CS): 99
- Missing: 31
- Total with smoking data: 238

COPD status:
- Control: 151
- COPD: 87
- Missing: 31

Lung function:
- FEV1% non-missing: 238
- FEV1/FVC non-missing: 238

---

### STEP 9 — Merge Expression + Metadata
Merged CC16 expression with cleaned metadata using sample ID.

Final analysis table included:
- cc16_expr
- smoking_status
- copd_group
- fev1_percent
- fev1_fvc
- age_years
- pack_years

---

## STATISTICAL ANALYSES PERFORMED

---

### ANALYSIS 1 — Smoking Status Comparison

Groups:
- Current smokers (n = 99)
- Ex-smokers (n = 139)

Statistics:
- Mean CC16 (CS): 12.14 (SD 0.54)
- Mean CC16 (EX): 12.39 (SD 0.18)

Test:
- Welch two-sample t-test (unequal variance)

Results:
- p-value: 2.32 × 10⁻⁵
- Cohen’s d: −0.66 (moderate-to-large effect)

Interpretation:
- CC16 expression is significantly suppressed in current smokers.

---

### ANALYSIS 2 — COPD Status Comparison

Groups:
- Control (n = 151)
- COPD (n = 87)

Statistics:
- Mean CC16 (Control): 12.32
- Mean CC16 (COPD): 12.24

Test:
- Welch two-sample t-test

Results:
- p-value: 0.14

Interpretation:
- CC16 expression alone does not strongly discriminate COPD status.

---

### ANALYSIS 3 — Lung Function Association

Variable:
- FEV1% predicted (continuous)

Test:
- Pearson correlation

Results:
- All samples: r = 0.24, p = 1.98 × 10⁻⁴
- Current smokers subgroup: r ≈ 0.37

Interpretation:
- Higher CC16 expression is associated with better lung function.
- Relationship stronger under active smoking exposure.

---

## VISUALIZATIONS GENERATED

- CC16 expression distribution plot
- CC16 vs smoking status group plot
- CC16 vs COPD group boxplot
- CC16 vs FEV1% scatter plot

(All plots generated using matplotlib; no post-processing.)

---

## KEY CONCLUSIONS (ONLY FROM DATA)

- CC16 probe mapping was successfully validated on GPL13243.
- CC16 expression is stable and biologically plausible in bronchial epithelium.
- Active smoking significantly suppresses CC16 transcript levels.
- CC16 shows a positive relationship with lung-function severity.
- CC16 is a **context-dependent airway biomarker**, not a standalone disease classifier.

---

---

## REPRODUCIBILITY

- Entire analysis performed in Jupyter Notebook
- HTML exports provided for audit
- All steps traceable from raw GEO input to final figures

---

## AUTHOR

Shruthi Reddy Vudem  
M.S. Health Data Science  
Saint Louis University  

---

## KEYWORDS
CC16 · SCGB1A1 · Uteroglobin · Transcriptomics · Bronchial Epithelium · Smoking Exposure · COPD · FEV1 · GEO · Affymetrix

---


