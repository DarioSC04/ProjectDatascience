# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Data Science course project (DHBW 6th Semester) analyzing the **Adult Income Dataset** (`adult.csv`). The project explores income patterns based on demographic and employment factors using exploratory data analysis and statistical hypothesis testing.

## Project Structure

```
Project/
├── adult.csv          # Adult Income Dataset (Census data)
├── Code.ipynb         # Main analysis notebook (Jupyter)
├── .venv/             # Python virtual environment
└── .gitignore         # Git ignore rules
```

## Dataset: adult.csv

The Adult Income Dataset contains census data with the following key variables:

**Demographics:**
- `age`: Age of person
- `gender`: Gender (Male/Female)
- `race`: Ethnicity
- `native-country`: Country of origin

**Employment & Education:**
- `workclass`: Employment type (Private, Self-employed, Government, etc.)
- `occupation`: Job category
- `education`: Education level (categorical)
- `educational-num`: Education level (numeric 1-16)
- `hours-per-week`: Weekly work hours

**Financial:**
- `capital-gain`: Capital gains
- `capital-loss`: Capital losses
- `fnlwgt`: Final weight (statistical weighting factor - can usually be ignored)

**Target Variable:**
- `income`: Binary classification (<=50K or >50K)

### Dataset Characteristics
- **Outliers**: `capital-gain` and `capital-loss` are heavily right-skewed (most values are 0)
- **Income encoding**: Values are `'<=50K'` and `'>50K'` (no leading spaces)
- The `fnlwgt` variable is a demographic weighting factor and typically not used for ML modeling

## Development Environment

### Setup
```bash
# Activate virtual environment
.venv\Scripts\activate  # Windows
source .venv/bin/activate  # Unix/macOS

# Install dependencies (if needed)
pip install pandas numpy matplotlib jupyter
```

### Running the Analysis
```bash
# Start Jupyter Notebook
jupyter notebook Code.ipynb

# Or use Jupyter Lab
jupyter lab
```

The notebook is designed to be run sequentially from top to bottom. Each code cell depends on previous cells (especially the data loading in the first cell).

## Notebook Structure

**Aufgabe 1: Datenexploration**
- Data loading and initial inspection
- Descriptive statistics with `df.describe()`
- Outlier analysis using IQR method

**Aufgabe 2: Fragestellungen und Hypothesen**
- Five hypotheses tested with filtering and grouping:
  1. Education → Income correlation
  2. Work hours → Income relationship
  3. Gender pay gap analysis
  4. Age → Income patterns
  5. Occupation → Income distribution
- Includes combined analyses (e.g., Gender × Education)

## Working with the Notebook

### Key DataFrame Conventions
- Main dataframe: `df` (loaded from `adult.csv`)
- Income filtering: Use `df['income'] == '>50K'` (no leading space)
- Age groups are created with `pd.cut()` as `df['age_group']`

### Common Operations
```python
# Percentage of high earners by group
df.groupby('category')['income'].apply(
    lambda x: (x == '>50K').sum() / len(x) * 100
)

# Multiple grouping
df.groupby(['var1', 'var2'])['income'].apply(...)

# Filtering
df[df['hours-per-week'] > 40]
```

### Visualization Style
- Matplotlib is used for all visualizations
- Common pattern: `fig, ax = plt.subplots(figsize=(...))` then `plot()` on ax
- Reference lines at 50% are added with `ax.axhline()` or `ax.axvline()`

## Course Context (DHBW Data Science)

This project follows a structured assignment format:
- **Aufgabe 1**: Initial data exploration (shape, columns, describe(), outliers)
- **Aufgabe 2**: Hypothesis formulation and testing using pandas filtering/grouping
- Future tasks likely include: data cleaning, visualization, modeling, evaluation

When adding new analysis sections, follow the existing pattern:
1. Markdown cell with hypothesis/question
2. Code cell(s) with analysis
3. Visualization code cell
4. Markdown cell with interpretation

## Notes

- The notebook uses German for headers and descriptions (course language)
- All code and comments can be in English or German
- The dataset is already cleaned and ready for analysis
- Missing value handling may be needed for some categorical variables (check with `df.isnull().sum()`)
