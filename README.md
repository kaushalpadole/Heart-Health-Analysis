# Pulse of Prevention: Heart Health Analysis

Exploratory data analysis of heart-health records using Python, Pandas,
NumPy, Matplotlib, and Seaborn. This project investigates demographic
and clinical variables, explores associations with the heart-disease
target, and builds an introductory logistic-regression classifier.

> **Important:** This is an educational analysis of a public dataset,
> not a clinical study or diagnostic tool. The results describe patterns
> in the available records and do not establish causation or support
> medical decisions.

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

## Analysis and Results

The results below are from the notebook analysis on `df_clean`, unless
stated otherwise.

# Basic Questions (10)

### 1. What is the average age of records in the dataset?

The average age is **54.42 years**.

### 2. What is the gender distribution?

    `sex` code Description     Records    Share
  ------------ ------------- --------- --------
             0 Female               96   31.79%
             1 Male                206   68.21%

The dataset contains more male-coded records than female-coded records.
This distribution alone does not establish sex-specific heart-disease
risk.

### 3. What is the average resting blood pressure?

The mean `trestbps` is **131.60 mm Hg**.

### 4. How many records have fasting blood sugar above 120 mg/dl?

**45 records (14.90%)** have `fbs = 1`; 257 records have `fbs = 0`.

### 5. What chest-pain categories are recorded?

The dataset contains `cp` codes **0, 1, 2, and 3**.

    `cp` code   Records    Share
  ----------- --------- --------
            0       143   47.35%
            1        50   16.56%
            2        86   28.48%
            3        23    7.62%

The project brief does not provide verified clinical labels for each
`cp` code, so the codes are kept numeric.

### 6. What is the maximum heart rate achieved?

The maximum recorded `thalach` is **202 bpm**.

### 7. What percentage of records report exercise-induced angina?

**99 records (32.78%)** have `exang = 1`; 203 records have `exang = 0`.

### 8. What is the average cholesterol level?

The mean `chol` value is **246.50 mg/dl**.

### 9. How many records have a resting ECG result of 2?

**4 records** have `restecg = 2` (**1.32%** of 302 rows).

### 10. What is the distribution of major vessels colored by fluoroscopy?

    `ca` code   Records    Share
  ----------- --------- --------
            0       175   57.95%
            1        65   21.52%
            2        38   12.58%
            3        20    6.62%
            4         4    1.32%

The `ca = 4` category is outside the 0--3 range described in the project
brief and should be investigated.

# Intermediate Questions (10)

### 1. What is the correlation between age and cholesterol?

The Pearson correlation is **0.207**, indicating a weak positive linear
association between `age` and `chol`. The scatter plot shows substantial
variation; correlation does not imply causation.

### 2. How does chest-pain type vary across age?

A stacked bar chart using
`groupby('age')['cp'].value_counts().unstack()` showed variation in
counts across ages. Ages **58**, **57**, and **54** had the largest
total counts in the plotted data. Chest-pain type 0 contributed
substantially around ages 57--60, and type 2 also appeared frequently at
several ages. The chart shows counts, not age-specific percentages, and
the numeric codes are not assigned unverified clinical labels.

### 3. How does maximum heart rate vary with exercise-induced angina?

      `exang`   Mean `thalach`
  ----------- ----------------
     0 --- No       155.60 bpm
    1 --- Yes       137.21 bpm

The group with `exang = 1` had an average maximum heart rate
approximately **18.38 bpm lower** than the group with `exang = 0`. This
is a descriptive association.

### 4. Is resting blood pressure significantly different between male and female records?

  Metric                            Result
  ------------------------- --------------
  Female mean (`sex = 0`)     133.08 mm Hg
  Male mean (`sex = 1`)       130.91 mm Hg
  Difference                    2.17 mm Hg
  Welch's t-statistic               0.9485
  p-value                           0.3443

Since **p = 0.3443 \> 0.05**, the test does not provide sufficient
evidence of a difference in population mean resting blood pressure
between the two groups at the 5% significance level. This is a failure
to reject the null hypothesis, not proof that the means are identical.

### 5. What is the relationship between fasting blood sugar and heart-disease status?

A cross-tabulation and stacked bar chart compared `fbs` with `target`.
The dataset contains 257 records with `fbs = 0` and 45 with `fbs = 1`.
Because the groups are different sizes, raw counts alone cannot
establish which group has a higher heart-disease proportion.
Within-group percentages are more appropriate for that comparison.

### 6. How does the number of major vessels (`ca`) relate to heart-disease status?

The stacked bar chart showed that `ca = 0` had the largest total count
and the largest absolute number of records with heart disease. Counts
declined across categories, but the groups also became smaller.
Therefore, counts alone do not establish how the heart-disease
proportion varies by `ca`. Four records had `ca = 4`, outside the range
documented in the brief.

### 7. What is the average `oldpeak` for each chest-pain category?

    `cp` code   Mean `oldpeak`
  ----------- ----------------
            0            1.383
            1            0.316
            2            0.807
            3            1.391

Categories 0 and 3 had the highest mean `oldpeak` values, while category
1 had the lowest. These are descriptive differences; statistical
significance was not tested.

### 8. How is thalassemia category distributed by heart-disease status?

A stacked bar chart compared `thal` with `target`. Category `thal = 2`
had the largest total count and the largest absolute number of records
with heart disease. Category `thal = 3` had the second-largest total
count, with more records without heart disease than with heart disease.
The `thal = 0` category contained very few records and is outside the
categories listed in the project brief. Counts should not be mistaken
for within-category rates.

### 9. What are the most common combinations of risk factors among records with heart disease?

The analysis grouped records with `target = 1` by `cp`, `fbs`, `exang`,
and `thal`.

    Rank   `cp`   `fbs`   `exang`   `thal`   Count
  ------ ------ ------- --------- -------- -------
       1      2       0         0        2      41
       2      1       0         0        2      30
       3      0       0         0        2      23
       4      2       1         0        2      10
       5      0       0         1        2       6
       6      2       0         0        3       6
       7      3       0         0        2       5
       8      1       0         0        3       4
       9      2       0         1        2       4
      10      2       1         0        3       3

The most frequent combination was `cp = 2`, `fbs = 0`, `exang = 0`,
`thal = 2`, with **41 records**. These are recurring combinations in the
cleaned data, not proof of causal risk profiles or counts of verified
distinct patients.

### 10. How do clinical measurements compare between records with and without heart disease?

  Variable       Target = 1   Target = 0
  ------------ ------------ ------------
  `age`               52.59        56.60
  `trestbps`         129.25       134.40
  `chol`             242.64       251.09
  `thalach`          158.38       139.10
  `oldpeak`            0.59         1.59
  `exang`              0.14         0.55
  `ca`                 0.34         1.17

The pair plot showed substantial overlap in age, cholesterol, and
resting blood pressure between the two groups. Mean values of binary
variables can be read as proportions of records coded 1; means of
nominal categorical codes such as `cp` and `thal` have limited direct
interpretation. The comparisons are descriptive and do not establish
causation.

# Advanced Questions (5)

### 1. What is the effect of combining age, cholesterol, and resting blood pressure?

The pair plot of `age`, `chol`, and `trestbps`, colored by `target`,
showed considerable overlap between the heart-disease groups. These
three measurements alone did not visually separate the two categories
clearly. The plot is exploratory and does not quantify independent
effects or prove causation.

### 2. Which variable has the strongest correlation with heart-disease status?

  Variable       Correlation
  ------------ -------------
  `exang`             -0.436
  `cp`                +0.432
  `oldpeak`           -0.429
  `thalach`           +0.420
  `ca`                -0.409
  `slope`             +0.344
  `thal`              -0.343
  `sex`               -0.284
  `age`               -0.221
  `trestbps`          -0.146
  `chol`              -0.081
  `fbs`               -0.027
  `restecg`           +0.135

The largest absolute correlation was for `exang` (**−0.436**), closely
followed by `cp` (**+0.432**) and `oldpeak` (**−0.429**). Categorical
variables are encoded numerically, so correlation signs depend on
coding. Correlation is not causation and does not establish independent
predictive importance.

### 3. Can logistic regression predict heart-disease status?

A pipeline using `StandardScaler` and
`LogisticRegression(max_iter=1000)` was trained on an 80% training split
and evaluated on a 20% test split (`random_state=42`, stratified by
target).

  Metric                            Result
  --------------------------- ------------
  Test accuracy                 **80.33%**
  Test records                          61
  Correct predictions                   49
  Macro-average F1-score              0.80
  Weighted-average F1-score           0.80

**Confusion matrix** (rows = actual class 0, 1; columns = predicted
class 0, 1):

               Predicted 0   Predicted 1
  ---------- ------------- -------------
  Actual 0              21             7
  Actual 1               5            28

For the heart-disease class (`target = 1`), precision was **0.80**,
recall was **0.85**, and F1-score was **0.82**. The model missed five
positive test records (false negatives) and incorrectly classified seven
negative records as positive (false positives).

This is a baseline educational model evaluated on one small test split.
Its accuracy is not evidence of clinical reliability; independent
validation and more rigorous evaluation would be needed before any
real-world use.

### 4. How do slope values vary across chest-pain types?

The stacked bar chart showed different distributions of `slope` across
`cp` categories. Chest-pain type 0 had the highest total count, with
slope category 1 contributing the largest portion. Slope category 2 was
most frequent for chest-pain type 1 and was also common for type 2. For
type 3, category 1 was the most frequent. The chart displays counts, so
group-size differences matter.

### 5. Can survival rates be analyzed by thalassemia type over time?

**Not with the available dataset.** The dataset includes age, `thal`,
and a heart-disease label, but does not include survival status,
mortality, follow-up duration, or time-to-event information. The
suggested line plot can show age, thalassemia code, and target group,
but it cannot calculate survival rates or measure long-term outcomes.
Because `thal` is categorical, connecting its values with lines can also
imply a continuous trend that is not supported by the data.

## Overall Findings

-   The cleaned data contains 302 rows after removing 723 duplicate rows
    from the original 1,025.
-   Mean age was 54.42 years, mean resting blood pressure was 131.60 mm
    Hg, and mean cholesterol was 246.50 mg/dl.
-   Age and cholesterol had a weak positive correlation (**0.207**).
-   The Welch's t-test did not find a statistically significant
    difference in mean resting blood pressure by sex (**p = 0.3443**).
-   Among the examined combinations in heart-disease records, `cp = 2`,
    `fbs = 0`, `exang = 0`, and `thal = 2` was most frequent (**41
    records**).
-   The strongest absolute correlation with the target was `exang`
    (**−0.436**); the strongest positive correlation was `cp`
    (**+0.432**).
-   The baseline logistic-regression model achieved **80.33% test
    accuracy**, with five false negatives on the test split.
-   The dataset cannot support a survival-rate analysis because survival
    and follow-up variables are absent.

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

## Repository Structure

``` text
heart-health-analysis/
├── README.md
├── heart-health-analysis.ipynb
├── requirements.txt
└── images/
    └── (optional exported charts)
```

The dataset is hosted on Kaggle. If you include a local dataset copy in
the repository, confirm its license and distribution terms first.

## How to Run

1.  Clone or download this repository.
2.  Ensure Python and Jupyter are installed.
3.  Install the dependencies listed in `requirements.txt`.
4.  Place `heart.csv` in the path expected by the notebook, or update
    the notebook's data-loading cell.
5.  Run the notebook from top to bottom.

Example `requirements.txt`:

``` text
pandas
numpy
matplotlib
seaborn
scipy
scikit-learn
jupyter
```

## Conclusion

This project demonstrates an exploratory workflow covering data-quality
checks, descriptive analysis, visual exploration, correlation analysis,
group comparisons, and a baseline classification model. The findings
highlight patterns in the available records, while the documented
data-quality issues and modeling limitations show why healthcare
analytics requires careful validation and restrained interpretation.
