# Quick EDA and Data Cleaning

## Student Details

- **Name:** Chirag Deep
- **Admission No.:** 2026b01011118

## Project Overview

This project performs Exploratory Data Analysis (EDA) and basic data cleaning on a Railway Ticket Confirmation dataset using Python and Pandas. The aim is to understand the dataset, identify data-quality issues, investigate missing values, and save a cleaned CSV file for further analysis.

## Objectives

- Understand the structure and contents of the dataset.
- Identify missing values and duplicate records.
- Analyse ticket-status and special-consideration distributions.
- Investigate missing values before deciding how to handle them.
- Convert date columns to the appropriate datetime data type.
- Save the cleaned dataset separately.

## Tools and Libraries

- Python
- Jupyter Notebook
- Pandas
- NumPy

## Project Structure

The notebook uses the following relative data paths:

```text
project/
├── data/
│   ├── raw/
│   │   └── Railway Ticket Confirmation.csv
│   └── cleaned/
│       └── cleaned_railway_data.csv
└── Quick_EDA_and_Data_Cleaning.ipynb
```

Make sure the raw CSV file is placed at the path used in the notebook. Create the `data/cleaned/` directory before saving the output if it does not already exist.

## Steps Performed

1. Imported Pandas and NumPy.
2. Loaded the Railway Ticket Confirmation CSV file.
3. Inspected the first and last rows, shape, columns, and data types.
4. Checked missing values, duplicate records, and descriptive statistics.
5. Analysed the `Current Status`, `Waitlist Position`, `Special Considerations`, and `Quota` columns.
6. Investigated missing `Special Considerations` values across quota categories and ticket statuses.
7. Replaced missing values in `Special Considerations` with `Not Specified`.
8. Retained missing values in `Waitlist Position`, since they occur for Confirmed and RAC tickets and may not apply to those statuses.
9. Converted `Date of Journey` and `Booking Date` to datetime.
10. Saved the cleaned data to a separate CSV file and checked the final shape and missing values.

## Key EDA Findings

- Original dataset size: **30,000 rows and 21 columns**.
- Missing values in `Special Considerations`: **9,955**.
- Missing values in `Waitlist Position`: **19,947**.
- Duplicate rows found: **0**.
- Ticket status counts:
  - Waitlisted: 10,053
  - RAC: 10,029
  - Confirmed: 9,918
- `Waitlist Position` is available for all Waitlisted tickets; missing values are associated with Confirmed and RAC tickets.
- Missing values in `Special Considerations` were found across all four quota categories.

## Data Cleaning Decisions

- Filled the 9,955 missing values in `Special Considerations` with `Not Specified`. The notebook's investigation did not establish that these missing values meant that no special consideration applied, so they were labelled as unspecified.
- Did **not** fill missing values in `Waitlist Position`, because these are associated with Confirmed and RAC statuses where a waitlist position may not be applicable.
- Converted `Date of Journey` and `Booking Date` to datetime using `errors="coerce"`.
- Kept `Age of Passengers` as a categorical text column and `Waitlist Position` as text because its values include the `WL` prefix.

## Source Code (Core Project Operations)

The following snippets show the main operations used in the notebook.

### 1. Import libraries and load the dataset

```python
import pandas as pd
import numpy as np

df = pd.read_csv("../data/raw/Railway Ticket Confirmation.csv")
```

### 2. Explore the dataset

```python
df.head()
df.tail()
df.shape
df.info()
df.columns

df.isnull().sum()
df.duplicated().sum()
df.describe()
```

### 3. Analyse ticket status and missing waitlist positions

```python
df["Current Status"].value_counts()

df.groupby("Current Status")["Waitlist Position"].apply(
    lambda x: x.isnull().sum()
)
```

### 4. Investigate special considerations

```python
df["Special Considerations"].value_counts(dropna=False)

pd.crosstab(
    df["Special Considerations"].fillna("Missing"),
    df["Quota"]
)

df[df["Special Considerations"].isnull()][
    ["Quota", "Current Status"]
].head(10)
```

### 5. Clean the data

```python
cleaned_df = df.copy()

cleaned_df["Special Considerations"] = cleaned_df[
    "Special Considerations"
].fillna("Not Specified")

cleaned_df["Date of Journey"] = pd.to_datetime(
    cleaned_df["Date of Journey"], errors="coerce"
)

cleaned_df["Booking Date"] = pd.to_datetime(
    cleaned_df["Booking Date"], errors="coerce"
)
```

### 6. Validate and save the cleaned dataset

```python
print("Original Dataset Shape:", df.shape)
print("Cleaned Dataset Shape:", cleaned_df.shape)

print("\nRemaining Missing Values:")
print(cleaned_df.isnull().sum())

cleaned_df.to_csv(
    "../data/cleaned/cleaned_railway_data.csv",
    index=False
)
```

## Final Conclusion

The project successfully demonstrates basic Exploratory Data Analysis and Data Cleaning using Python and Pandas. The cleaned dataset retains all **30,000 rows and 21 columns**. The missing values in `Special Considerations` were handled as `Not Specified`, while the 19,947 missing values in `Waitlist Position` were intentionally retained. No duplicate records were found.

The cleaned CSV file is saved as `cleaned_railway_data.csv` in the `data/cleaned/` folder.

---

**Author:** Chirag Deep  
**Admission No.:** 2026b01011118
