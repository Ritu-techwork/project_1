# Mall Customers: Exploratory Data Analysis (EDA) with Python

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![NumPy](https://img.shields.io/badge/NumPy-numerical%20stats-013243)
![Pandas](https://img.shields.io/badge/Pandas-data%20analysis-150458)
![Matplotlib](https://img.shields.io/badge/Matplotlib-visualization-11557c)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

A clean, beginner-friendly and reproducible **exploratory data analysis (EDA)** of the Mall Customers dataset. The project answers six business-style questions about 200 mall customers using **NumPy** for numerical statistics, **Pandas** for filtering, grouping and sorting, and **Matplotlib** for visualization.

> **Scope:** This is a basic EDA project only. No machine learning, clustering or prediction models are used.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Dataset](#dataset)
3. [Data Dictionary](#data-dictionary)
4. [Objectives and Questions](#objectives-and-questions)
5. [Tech Stack](#tech-stack)
6. [Project Structure](#project-structure)
7. [Installation and Setup](#installation-and-setup)
8. [How to Run](#how-to-run)
9. [Methodology](#methodology)
10. [Results](#results)
11. [Visualizations](#visualizations)
12. [Key Insights and Observations](#key-insights-and-observations)
13. [Limitations](#limitations)
14. [Future Improvements](#future-improvements)
15. [License](#license)
16. [Author](#author)

---

## Project Overview

Retail businesses collect large amounts of customer data, but raw numbers do not help until they are summarized and visualized. This project takes a small, well-known customer dataset and walks through the standard EDA workflow:

- Load and inspect the data
- Check its quality (missing values, duplicates)
- Summarize it with descriptive statistics
- Compare customer groups (gender and age)
- Rank customers by spending score
- Visualize the main patterns
- Write clear, data-backed observations

The whole analysis is contained in a single script, `eda.py`, which prints every result to the console and saves two charts as PNG files.

## Dataset

| Property | Value |
|---|---|
| Name | Mall Customers (Customer Segmentation Tutorial in Python) |
| Source | [Kaggle](https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python) |
| File | `Mall_Customers.csv` |
| Rows | 200 |
| Columns | 5 |
| Missing values | 0 |
| Duplicate rows | 0 |

Each row represents one customer.

## Data Dictionary

| Column | Type | Description |
|---|---|---|
| `CustomerID` | Integer | Unique ID for each customer (1 to 200) |
| `Gender` | String | `Male` or `Female` |
| `Age` | Integer | Age of the customer in years (18 to 70) |
| `Annual Income (k$)` | Integer | Annual income in thousands of dollars (15 to 137) |
| `Spending Score (1-100)` | Integer | Score assigned by the mall based on customer behavior and spending nature (1 to 99) |

> Note: the column names contain spaces and brackets, so the script stores them in the variables `INCOME` and `SPEND` to avoid typing mistakes.

## Objectives and Questions

The analysis answers these six questions:

1. **Gender count:** How many customers are male and how many are female?
2. **Descriptive statistics:** What are the mean, median, minimum and maximum of age, annual income and spending score?
3. **Age groups:** Which age group has the highest average spending score?
4. **Gender comparison:** How do average income and spending score compare between genders?
5. **Top spenders:** Who are the 10 customers with the highest spending scores?
6. **Data quality:** Are there any missing values or duplicate records?

Additional requirements covered by the script:

- Display `head()`, `tail()`, `shape`, `info()` and `describe()`
- Use at least one NumPy array for calculations
- Use Pandas filtering, sorting, `value_counts()` and `groupby()`
- Create two simple charts
- Write three observations

## Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.9+ | Programming language |
| NumPy | Numerical statistics (mean, median, min, max, correlation) |
| Pandas | Loading, inspecting, grouping, sorting and filtering data |
| Matplotlib | Bar chart and scatter plot |

## Project Structure

```
.
├── eda.py                          # Main analysis script
├── Mall_Customers.csv              # Dataset (200 rows, 5 columns)
├── chart1_gender_count.png         # Output: gender count bar chart
├── chart2_income_vs_spending.png   # Output: income vs spending scatter plot
└── README.md                       # Project documentation
```

## Installation and Setup

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/mall-customers-eda.git
cd mall-customers-eda
```

### 2. (Optional) Create a virtual environment

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

### 3. Install the dependencies

```bash
pip install numpy pandas matplotlib
```

### 4. Check the dataset

Make sure `Mall_Customers.csv` is in the same folder as `eda.py`. If it is stored somewhere else, change this line at the top of the script:

```python
FILE_PATH = "Mall_Customers.csv"
```

## How to Run

```bash
python eda.py
```

What happens when you run it:

1. All results are printed to the terminal in clearly labeled sections.
2. Two PNG charts are saved in the project folder.
3. Two chart windows open on screen (close them to let the script finish).

Running in Jupyter Notebook or Google Colab also works. Copy the code into cells and make sure the CSV file is uploaded.

## Methodology

The analysis follows a simple, standard EDA pipeline.

### Step 1: Load and inspect

The CSV is loaded with `pd.read_csv()`. The first and last rows are viewed with `head()` and `tail()`. The structure is checked with `shape`, `info()` and `describe()`.

### Step 2: Data quality checks

```python
df.isnull().sum()                    # missing values per column
df.duplicated().sum()                # fully duplicated rows
df["CustomerID"].duplicated().sum()  # duplicated customer IDs
```

Both rows and IDs are checked because a duplicated ID can hide a data entry problem even when the rest of the row differs.

### Step 3: Gender distribution

```python
gender_counts = df["Gender"].value_counts()
```

`value_counts(normalize=True)` is also used to calculate percentages.

### Step 4: NumPy statistics

Each numeric column is converted to a NumPy array and summarized:

```python
arr = df[col].to_numpy()
np.mean(arr), np.median(arr), np.min(arr), np.max(arr)
```

This satisfies the requirement to use NumPy arrays for numerical calculations.

### Step 5: Age group analysis

Ages are grouped into bins with `pd.cut()`:

```python
age_bins   = [17, 25, 35, 45, 55, 65, 70]
age_labels = ["18-25", "26-35", "36-45", "46-55", "56-65", "66-70"]
```

The bin edges start at 17 because `pd.cut()` excludes the left edge by default, so this makes 18 the first included age. Then `groupby("Age Group", observed=True)` is used to calculate the number of customers and the average spending score in each group.

### Step 6: Gender comparison

```python
df.groupby("Gender")[[INCOME, SPEND]].mean()
```

### Step 7: Top 10 customers

```python
df.sort_values(by=[SPEND, "CustomerID"], ascending=[False, True]).head(10)
```

Sorting by `CustomerID` as a second key breaks ties in a stable, predictable way.

### Step 8: Visualization

A bar chart shows the gender counts and a scatter plot shows annual income against spending score. NumPy's `corrcoef()` is used to calculate the correlation between income and spending score.

## Results

### Dataset shape

The dataset has **200 rows and 5 columns**.

### Descriptive statistics (from `describe()`)

| Statistic | Age | Annual Income (k$) | Spending Score |
|---|---|---|---|
| Mean | 38.85 | 60.56 | 50.20 |
| Std. deviation | 13.97 | 26.26 | 25.82 |
| Min | 18 | 15 | 1 |
| 25% | 28.75 | 41.50 | 34.75 |
| Median (50%) | 36 | 61.50 | 50 |
| 75% | 49 | 78 | 73 |
| Max | 70 | 137 | 99 |

### Q1. Customers by gender

| Gender | Customers | Share |
|---|---|---|
| Female | 112 | 56.0% |
| Male | 88 | 44.0% |

### Q2. NumPy statistics

| Column | Mean | Median | Min | Max |
|---|---|---|---|---|
| Age | 38.85 | 36.0 | 18 | 70 |
| Annual Income (k$) | 60.56 | 61.5 | 15 | 137 |
| Spending Score (1-100) | 50.20 | 50.0 | 1 | 99 |

### Q3. Average spending score by age group

| Age Group | Customers | Avg Spending Score |
|---|---|---|
| 18-25 | 38 | 54.95 |
| **26-35** | **60** | **64.45** |
| 36-45 | 36 | 44.83 |
| 46-55 | 37 | 36.70 |
| 56-65 | 17 | 32.12 |
| 66-70 | 12 | 47.25 |

**Answer:** the **26-35** age group has the highest average spending score (64.45).

### Q4. Average income and spending score by gender

| Gender | Avg Annual Income (k$) | Avg Spending Score |
|---|---|---|
| Female | 59.25 | 51.53 |
| Male | 62.23 | 48.51 |

### Q5. Top 10 customers by spending score

| CustomerID | Gender | Age | Annual Income (k$) | Spending Score |
|---|---|---|---|---|
| 12 | Female | 35 | 19 | 99 |
| 20 | Female | 35 | 23 | 98 |
| 146 | Male | 28 | 77 | 97 |
| 186 | Male | 30 | 99 | 97 |
| 128 | Male | 40 | 71 | 95 |
| 168 | Female | 33 | 86 | 95 |
| 8 | Female | 23 | 18 | 94 |
| 142 | Male | 32 | 75 | 93 |
| 164 | Female | 31 | 81 | 93 |
| 34 | Male | 18 | 33 | 92 |

**Tie note:** three customers (IDs 34, 42 and 174) share the score of 92, which is the 10th-place score. Because ties are broken by the lower `CustomerID`, customer 34 appears in the list. The script prints this note so the result is transparent.

### Q6. Missing values and duplicates

| Check | Result |
|---|---|
| Missing values (all columns) | 0 |
| Fully duplicated rows | 0 |
| Duplicated CustomerIDs | 0 |

### Correlation

The correlation between annual income and spending score is **0.010**, which is essentially zero.

## Visualizations

### Chart 1: Number of customers by gender

![Gender count bar chart](chart1_gender_count.png)

The bar chart shows 112 female customers and 88 male customers, with the exact count labeled on each bar.

### Chart 2: Annual income vs spending score

![Income vs spending score scatter plot](chart2_income_vs_spending.png)

The scatter plot shows that the points do not follow a straight line. Instead, they form visible groups: a dense cluster in the middle (moderate income, moderate spending) and groups in the corners (for example, low income with high spending, and high income with low spending).

## Key Insights and Observations

1. **The data is clean and well balanced.** There are no missing values and no duplicates. The sample has 112 women and 88 men, so women are slightly over-represented (56%).

2. **Customers aged 26-35 are the strongest spenders.** This group has the highest average spending score (64.45) and is also the largest age group (60 customers). Spending scores drop steadily for customers aged 36 to 65, with the lowest average (32.12) in the 56-65 group. The 66-70 group rises again to 47.25, but it has only 12 customers, so this figure is less reliable.

3. **Income alone does not explain spending.** The correlation between income and spending score is about 0.01. Both low-income and high-income customers appear at the top and the bottom of the spending scale. Gender also separates customers very little: average income (59.25 vs 62.23) and average spending score (51.53 vs 48.51) are close for women and men.

### Business takeaway

A marketing team should not target customers by income or gender alone. Age and the combination of income and spending behavior are more informative, which is why a later segmentation step could be valuable.

## Limitations

- **Small sample:** 200 customers is a small sample, so patterns may not generalize to all mall customers.
- **Synthetic-style data:** this tutorial dataset is widely used for learning, so results are best treated as an educational example and not as real business evidence.
- **Spending score is a derived score:** it is assigned by the mall and is not actual money spent, so conclusions apply to the score and not to revenue.
- **Few variables:** the data has no purchase history, visit frequency, location or product categories.
- **Uneven age groups:** the 56-65 and 66-70 groups have only 17 and 12 customers, so their averages are less stable.
- **Correlation is not causation:** the observed patterns describe the data and do not explain why customers behave this way.
- **Descriptive only:** no statistical significance tests were run, so small differences (for example, between genders) should not be over-interpreted.

## Future Improvements

- Add more charts: histograms for age and income, box plots by gender, and a correlation heatmap
- Add a notebook (`.ipynb`) version with markdown explanations for each step
- Run statistical tests (for example, a t-test for the gender difference in spending score)
- Add outlier detection using the IQR method
- Apply customer segmentation (such as K-Means clustering) as a separate project
- Package the analysis into functions and add unit tests
- Build an interactive dashboard with Streamlit or Plotly

## License

Add the license of your choice (for example, MIT) by creating a `LICENSE` file in this repository. Check the dataset's own terms on Kaggle before redistributing the CSV.

## Author

**Your Name**
- GitHub: [@your-username](https://github.com/your-username)
- LinkedIn: [your-profile](https://www.linkedin.com/in/your-profile)

If you found this project useful, consider giving it a star.
