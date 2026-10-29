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

