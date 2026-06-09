# EDA Study Notes

> *"Bad programmers worry about the code. Good programmers worry about the data structures and their relationships."* — Linus Torvalds

---

## 1. What is EDA (Exploratory Data Analysis)?

Before starting to work with a dataset it is important to understand the data itself. That is the purpose of EDA — one of the most important skills in data science: understanding the underlying data structure before making assumptions, building models, or drawing conclusions. It is essentially an investigation that helps you identify issues before they become bigger problems.

EDA is iterative. It is not a one-time step, but something you return to until the process is complete. It starts by generating questions, then using visualization tools and data transformations to answer those questions, then refining or generating new ones using the previous answers.

**EDA's purpose is to:**

**Understand the data:**
- What does the dataset contain? → `df.head()`, `df.sample(5)`
- What story is the data trying to tell?
- How many rows and columns? → `df.shape`
- What does each column represent?
- What data types are present? → `df.info()`

**Assess data quality:**
- Is the data consistent?
- Are there missing values? → `.isna()`
- Are there obvious errors, duplicates, or invalid entries?

**Discover what's interesting:**
- Are there unusual patterns?
- Is there a correlation between columns? → `df.corr()`

**Understand distributions:**
- What values are common? What values are rare?
- Are there outliers?
- Is the data skewed?

**Explore relationships:**
- Do some variables move together?
- Are there meaningful correlations?
- Are some features likely to be important?

Good EDA is driven by curiosity. It is about transforming raw data into insight.

---

## 2. What types of data exist inside an EDA?

Data is generally divided into:

**Quantitative:** Numerical.
- Continuous — floats
- Discrete — integers

**Qualitative:** Descriptive, non-numerical.
- Nominal — categories with no order
- Ordinal — categories with a meaningful order

Other types:

**Boolean:** A special case. Treated as nominal when analysing it as a label, or as discrete when using it as a number.

**Temporal** (date, datetime, time): Represents a point or range in time.

**Unstructured** (text, image, audio, video): Data with no predefined format or schema. Requires specialized libraries.

---

## 3. What is the difference between univariate, bivariate, and multivariate analysis?

**Univariate** focuses on understanding each variable individually. Helps identify missing values, outliers, and patterns that may affect further analysis.
- Qualitative → count plots (compare frequencies), pie charts (visualize percentage distributions)
- Quantitative → histograms (where values lie), KDE / displots (probability density), box plots (summarize data and highlight outliers)

**Bivariate and multivariate** reveal relationships between two or more variables.
- Qualitative vs quantitative → bar plots (average values across categories), box plots and KDE (compare distributions between groups), heatmaps and cluster maps
- Quantitative vs quantitative → scatter plots (identify trends and correlations), pair plots (quick overview), line plots (trends over time)

---

## 4. What are descriptive statistics?

Tools that help understand and summarize data. → `df.describe()`

**Measures of central tendency** — the foundation for understanding data distribution and identifying anomalies.
- **Mean:** Sum of all values divided by the total number of items.
- **Median:** The middle value in a sorted dataset. Better to use when data has outliers.
- **Mode:** The most frequently occurring value in the dataset.

**Measures of variability (dispersion)** — how the data spreads and distributes.
- **Range:** Difference between the largest and smallest values. Sensitive to outliers.
- **Variance:** Average squared deviation from the mean.
- **Standard deviation:** Measures how much values differ from the mean. Square root of variance.

**Measures of frequency distribution** — a summarized way to show how data points are distributed across categories or intervals. Often the first step before applying advanced methods or creating visualizations.
- **Categories** — used for qualitative data
- **Intervals (bins)** — used for quantitative data with many distinct values
- **Frequency counts** — how many times each value or interval appears
- **Relative frequencies** — percentage of the total each category represents
- **Cumulative frequencies**

---

## 5. What is data cleaning and what tasks does it include?

Cleaning data involves finding and correcting errors, inconsistencies, and missing values.

**Handling missing data (null values):**
- Imputation — filling in missing values with predicted or estimated data
- Interpolation — deriving missing values from surrounding data points
- Deletion — removing rows or columns with missing values

**Handling outliers:** Data points that significantly differ from the rest. Can occur due to measurement errors, data entry errors, or genuine extreme observations.
- Remove if they are errors
- Transform to reduce their impact
- Winsorize — replace extreme values with the nearest values within the normal range
- Treat as a separate class

**Removing duplicates:** Duplicates skew predictions, waste storage, and increase processing time.

**Handling irrelevant data:** Data not useful for solving the problem. Identified via PCA, correlation analysis, or domain knowledge, then removed.

**Handling incorrect data:** Via data transformation or removal.

**Handling imbalanced data:** A dataset is imbalanced when one class has significantly fewer data points than another, resulting in a biased model. Techniques include:
- Resampling (oversampling the minority class or undersampling the majority)
- Synthetic data generation
- Cost-sensitive learning
- Ensemble learning

**Normalisation:** Scaling numerical values to a common range so different variables can be compared fairly. Prevents features with larger numbers from having a disproportionate influence.

---

## 6. What role do pandas, matplotlib, and seaborn play in EDA?

With pandas you can explore, clean, and transform the data; matplotlib is used to draw fully custom plots; seaborn is used to visualize patterns and distributions fast.

The natural flow: **pandas first** (load, clean, explore structure) → **seaborn** to spot patterns visually → **matplotlib** to polish or customize anything seaborn doesn't give you.

### pandas 🐼

A spreadsheet controlled with code. The mental model is a table — rows are observations, columns are variables. This table is called a **DataFrame**; individual columns are called **Series** (a one-dimensional array with an index).

It exists because NumPy is fast precisely because it works on arrays of uniform type — every element the same size in bytes, stored in a contiguous block of memory. That model breaks with real data (mixed types, missing values, labelled columns, dates). pandas solves this by making the column the unit of storage: each column is its own NumPy array, so each column is internally uniform and fast. The DataFrame is essentially a dictionary of arrays that all share the same index (row labels).

### matplotlib

A canvas you paint on with instructions. A **figure** contains one or more **axes** (the plot area with X/Y coordinates). You tell the library exactly what to draw, where, in what color, with which labels. Nothing appears automatically.

Two interfaces:
- **pyplot** (`plt.plot()`, `plt.xlabel()`) — tracks the "current" figure implicitly
- **Object-oriented** (`fig, ax = plt.subplots()`) — explicit, unambiguous, composable. What professionals use.

`fig, ax = plt.subplots()` gives one axes. `plt.subplots(2, 3)` gives a 2×3 grid. You then call `ax.plot()`, `ax.scatter()`, `ax.hist()`, `ax.set_xlabel()`, `ax.set_title()` etc. on each axes object. `plt.show()` or `plt.savefig()` renders it. Everything is manual.

### seaborn

A library for statistical visualizations with less code. You declare which column is x, which is y, and optionally a grouping variable — it handles the rest. It exists because matplotlib is expressive but verbose.

Underneath, every seaborn plot is a matplotlib figure. You can always access the underlying axes object and customize further. Seaborn expects **tidy data**: one observation per row, one variable per column.

Functions divide into three categories:
- **Relational plots** — relationships between two numerical variables
- **Distribution plots** — the shape of one or more variables
- **Categorical plots** — compare groups

The `hue` parameter is the killer feature: `sns.scatterplot(data=df, x="weight", y="cans_raided", hue="mood")` automatically splits the plot by the mood column with a legend and no extra code.

---

## 7. What is a correlation matrix?

It shows how every numerical variable in the dataset relates to every other one. Each cell contains a number between -1 and 1.

- Close to **1** → directly proportional (both go up together)
- Close to **-1** → inversely proportional (one goes up, the other goes down)
- Close to **0** → no relationship

In EDA it is used to spot which variables move together (potential redundancy in a model), which relate to the target variable, and which are independent of everything else.

---

## 8. What are outliers?

Outliers are data points that deviate significantly from the majority of other points. They can distort parameter estimates and reduce model effectiveness. There is a library called `pyod` specifically for outlier detection.

**Detection methods:**
- **Visual** — boxplots and scatter plots make them immediately obvious
- **IQR rule** — anything below Q1 − 1.5×IQR or above Q3 + 1.5×IQR is flagged
- **Z-score** — flags points more than 3 standard deviations from the mean

**Treatment options:**
- Remove if they are errors
- Cap to a max/min value (winsorizing)
- Transform the column — log scale compresses extreme values
- Keep them and document — sometimes outliers are the most interesting part of the data

---

## 9. What is hypothesis testing?

You see a pattern in your data. But is it real, or did it appear by chance because you only have a sample? Hypothesis testing answers that question formally.

You assume the pattern is not real (null hypothesis H₀). Then you calculate: if there truly were nothing going on, how likely would it be to see data like mine? That probability is the p-value.

- p < 0.05 → the data would be very unlikely if nothing were happening. The pattern is probably real. Reject the null.
- p is large → the data is perfectly plausible even if nothing is happening. Not enough evidence. Keep the null.

**Example:** Colony A meerkats average 620g, colony B averages 580g. Is that difference real or just a sampling fluke? A t-test gives you a p-value. If p = 0.003, the difference is real. If p = 0.4, it could easily be noise.

In EDA, hypothesis testing is used to confirm what your visualizations suggest before you act on them.

---

## Interesting tools to research

🌵Pandera → Library for validating DataFrames.
*FastAPI uses Pydantic to validate the shape of JSON coming into your API. Pandera does the same thing but for DataFrames. Both catch bad data early and give you a clear error instead of letting bugs propagate silently through the code.
Pydantic → validates JSON/dicts row by row.
Pandera → validates DataFrames (the whole table at once).
Same philosophy, different data shape.
🌵polars → It's a pandas replacement written in Rust, dramatically faster on large datasets, and has a cleaner API. Growing fast and increasingly appears in job descriptions. Worth being aware of.

## Sources

- https://sudhamsr.medium.com/python-for-data-science-what-exploratory-data-analysis-eda-actually-means-b2e3edd4c9b4
- https://python.plainenglish.io/powerful-eda-techniques-that-separate-beginners-from-experts-428f6300a7e
- https://lhamu.medium.com/eda-step-of-the-data-science-process-7ef4e6d96b3f
- https://medium.com/@sayedebad.777/mastering-eda-a-complete-guide-for-every-data-type-tabular-text-image-time-series-audio-b69ecf9113ac
- https://www.geeksforgeeks.org/data-science/descriptive-statistic/
- https://medium.com/geekculture/data-preparation-for-machine-learning-a-step-by-step-guide-102eed66a5ee
- https://medium.com/@AryanBeast/exploring-your-data-like-a-pro-the-eda-guide-for-ml-engieers-fa5ea357cc3d
- https://medium.com/@libertihub/correlation-vs-causation-outliers-and-class-distribution-practical-and-theoretical-guide-to-eda-f34a6065372e#6804
