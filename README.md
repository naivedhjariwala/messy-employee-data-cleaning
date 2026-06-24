# Messy Employee Data Cleaning

A data cleaning project focused on transforming a realistically messy employee dataset (400+ rows) into an analysis-ready format. Unlike a typical EDA project, this one is entirely about diagnosing and fixing real-world data quality issues — inconsistent formats, mixed types, invalid entries, and duplicates.

## Objective
To take a raw, messy HR-style dataset and apply a systematic cleaning pipeline: standardizing text, fixing data types, handling missing/invalid values, parsing inconsistent date formats, validating emails, and removing duplicates.

## Tools Used
- Python
- Pandas
- NumPy
- Regex (for email validation)

## Data Quality Issues Identified & Fixed

| Column | Issues Found | Fix Applied |
|---|---|---|
| `age` | Mixed formats (`"25"` vs `"25 yrs"`), text-based missing values (`"NA"`, `"N/A"`, `"unknown"`), unrealistic outliers (ages over 100) | Stripped suffixes, standardized missing values to `NaN`, converted to numeric, treated outliers as missing, filled with median |
| `salary` | Currency symbols (`₹`), trailing decimals, negative values, missing values stored as literal text (`"NaN"`) | Removed symbols, converted to numeric, treated negative salaries as invalid/missing, filled with median |
| `city` | Inconsistent casing (`Mumbai`/`MUMBAI`/`mumbai`), extra whitespace, alternate names for the same city (`Bangalore`/`Bengaluru`) | Stripped whitespace, standardized casing, manually mapped duplicate city names |
| `department` | Casing and whitespace inconsistencies | Stripped whitespace, standardized casing |
| `join_date` | At least 5 different date formats mixed in one column (DD-MM-YYYY, MM/DD/YYYY, YYYY-MM-DD, "Mon YYYY", year-only) | Parsed using `pd.to_datetime(errors='coerce')`; rows with unparseable formats documented and treated as missing rather than guessed |
| `email` | Mixed casing, literal `"invalid_email"` placeholder values, blank entries | Standardized casing, flagged invalid entries using regex validation |
| Duplicate rows | Exact duplicate rows, plus near-duplicate rows (same person, different formatting, different ID) | Removed exact duplicates with `drop_duplicates()`; flagged near-duplicates for manual review |

## Key Takeaways
- A single inconsistent format anywhere in a column (e.g. one `"25 yrs"` among numeric `"25"`s) is enough to force the entire column to be stored as text, blocking any numeric analysis until cleaned.
- Outliers and clearly invalid values (negative salaries, 200-year-old employees) should be treated as missing data, not deleted outright — preserves the rest of that row's valid information.
- Mixed date formats in a single column can't always be reliably auto-parsed; being explicit about what couldn't be parsed (and why) is more honest and useful than forcing a guess.
- Exact duplicates are easy to remove automatically; near-duplicates (same entity, different formatting/ID) require manual judgment, not just code.

## Files
- `messy_employee_data.csv` — raw, uncleaned dataset
- `employee_data_cleaning.ipynb` — full cleaning notebook with step-by-step process

## Author
Naivedh 
