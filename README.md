# Pulse of Prevention: Heart Health Analysis

Exploratory data analysis of heart-health records using Python, Pandas,
NumPy, Matplotlib, and Seaborn. This project investigates demographic
and clinical variables, explores associations with the heart-disease
target, and builds an introductory logistic-regression classifier.

## Project Overview

The project brief frames the work as an analysis for **HealthPulse
Analytics**, exploring patient characteristics and clinical measurements
associated with the dataset's heart-disease label.

### Objectives

-   Explore demographic and clinical variables.
-   Examine associations between clinical features and the heart-disease
    target.
-   Compare records with `target = 1` and `target = 0`.
-   Train and evaluate a baseline logistic-regression model.
-   Document data-quality issues and limitations.

## Dataset

-   **Dataset:** Heart Disease Dataset
-   **Source:** [Kaggle --- Heart Disease
    Dataset](https://www.kaggle.com/datasets/johnsmith88/heart-disease-dataset)
-   **Original shape:** 1,025 rows × 14 columns
-   **Rows after duplicate-row removal:** 302
-   **Missing values:** None detected in the original dataset
-   **Target:** `1` = heart disease present; `0` = heart disease absent

### Column Dictionary

  -----------------------------------------------------------------------
  Column                              Description
  ----------------------------------- -----------------------------------
  `age`                               Age

  `sex`                               Sex (`1` = male, `0` = female)

  `cp`                                Chest-pain category (coded 0--3 in
                                      the brief)

  `trestbps`                          Resting blood pressure (mm Hg)

  `chol`                              Serum cholesterol (mg/dl)

  `fbs`                               Fasting blood sugar above 120 mg/dl
                                      (`1` = true, `0` = false)

  `restecg`                           Resting electrocardiographic result

  `thalach`                           Maximum heart rate achieved

  `exang`                             Exercise-induced angina (`1` = yes,
                                      `0` = no)

  `oldpeak`                           ST depression induced by exercise
                                      relative to rest

  `slope`                             Slope of the peak exercise ST
                                      segment

  `ca`                                Number of major vessels colored by
                                      fluoroscopy

  `thal`                              Thalassemia category

  `target`                            Heart-disease label (`1` = present,
                                      `0` = absent)
  -----------------------------------------------------------------------

The project brief describes `ca` as ranging from 0--3 and documents
`thal` categories 1--3. The data also contains `ca = 4` and `thal = 0`;
these anomalies are retained and disclosed rather than silently removed.

## Tools and Libraries

Python · Pandas · NumPy · Matplotlib · Seaborn · SciPy · scikit-learn ·
Jupyter Notebook

## Data Cleaning and Preparation

1.  Loaded the source data into a Pandas DataFrame.
2.  Checked missing values; none were found.
3.  Checked duplicate rows: **723** duplicate rows were detected.
4.  Created `df_clean = df.drop_duplicates().copy()`, leaving **302
    rows**.
5.  Inspected column data types and categorical values.
6.  Used the interquartile range (IQR) method to flag possible outliers
    in numeric measurements. Outliers were not automatically deleted.
7.  Retained the `ca = 4` and `thal = 0` records while documenting that
    they fall outside the value ranges described in the project brief.

**Duplicate-row limitation:** The dataset does not provide a patient
identifier or enough provenance to confirm that identical rows are
duplicate observations of the same individual. Therefore, the 302 rows
are described as *remaining unique rows/records*, not verified unique
patients. Deduplication also changes the target distribution, so results
depend on this preprocessing choice.

### IQR Outlier Review

  Variable       Lower bound   Upper bound   Flagged values
  ------------ ------------- ------------- ----------------
  `age`                28.50         80.50                0
  `trestbps`           90.00        170.00                9
  `chol`              115.38        370.38                5
  `thalach`            84.12        215.12                1
  `oldpeak`            -2.40          4.00                5

These are IQR flags, not proof of data errors. The same row can be
flagged in more than one column.


## Limitations and Responsible Interpretation

1.  **Duplicate rows:** Removing duplicates reduced the dataset from
    1,025 rows to 302. Without patient identifiers and data provenance,
    the remaining rows cannot be assumed to represent 302 verified
    unique patients.
2.  **Unexpected category values:** `ca = 4` and `thal = 0` do not match
    the value ranges described in the project brief. These values
    require source-level investigation.
3.  **Dataset composition:** Deduplication changes the target
    distribution and may affect descriptive results and model
    evaluation.
4.  **Associations are not causal:** EDA and correlations do not prove
    that a variable causes, prevents, or independently predicts heart
    disease.
5.  **Small test set:** The model was assessed on 61 test records from a
    single split. Its performance estimate may be unstable.
6.  **Not for clinical use:** The model has not been clinically
    validated and must not be used for diagnosis, triage, or treatment
    decisions.
7.  **Survival analysis unavailable:** The dataset lacks survival
    outcomes and follow-up duration.

## Recommendations for Further Analysis

-   Verify the source and intended meaning of anomalous `ca` and `thal`
    values.
-   Compare the original dataset and deduplicated dataset as a
    sensitivity analysis.
-   Use within-group percentages, confidence intervals, and appropriate
    statistical tests when comparing categories.
-   Evaluate the classifier with cross-validation and a truly
    independent dataset; report class-specific recall and precision, not
    accuracy alone.
-   Consult qualified clinical experts before interpreting feature
    patterns as clinically meaningful.
-   Obtain suitable time-to-event data if survival analysis is required.


```

The dataset is hosted on Kaggle. If you include a local dataset copy in
the repository, confirm its license and distribution terms first.

```

## Conclusion

This project demonstrates an exploratory workflow covering data-quality
checks, descriptive analysis, visual exploration, correlation analysis,
group comparisons, and a baseline classification model. The findings
highlight patterns in the available records, while the documented
data-quality issues and modeling limitations show why healthcare
analytics requires careful validation and restrained interpretation.
