## Summary

This guide shows how to turn mixed date values into one consistent format: `YYYY-MM-DD`.
The example treats slash and hyphen dates as day-first, so `01/03/2024` means 1 March 2024.
It includes a Python script that parses known formats, flags invalid values, and saves a clean column.

## Choose the Target Format

Use the ISO 8601 format:

```text
YYYY-MM-DD
```

Examples:

| Original value | Clean value |
|---|---|
| `2024-03-01` | `2024-03-01` |
| `01/03/2024` | `2024-03-01` |
| `March 1 2024` | `2024-03-01` |
| `1-3-24` | `2024-03-01` |

This format is useful because it sorts correctly as text and is understood consistently by databases, spreadsheets, and reporting tools.

## Python Example

The script below reads a CSV file, cleans the `date` column, and writes a new file.

```python
import pandas as pd
from datetime import datetime

# Formats expected in the source data.
DATE_FORMATS = [
    "%Y-%m-%d",   # 2024-03-01
    "%d/%m/%Y",   # 01/03/2024
    "%B %d %Y",   # March 1 2024
    "%d-%m-%y",   # 1-3-24
]

def clean_date(value):
    """Convert one mixed-format date to YYYY-MM-DD."""
    if pd.isna(value) or str(value).strip() == "":
        return pd.NA

    value = str(value).strip()

    for date_format in DATE_FORMATS:
        try:
            parsed = datetime.strptime(value, date_format)
            return parsed.strftime("%Y-%m-%d")
        except ValueError:
            continue

    # Keep an identifiable marker instead of silently changing bad data.
    return pd.NA


# Load the source file.
df = pd.read_csv("input.csv")

# Create the cleaned date column.
df["date_clean"] = df["date"].apply(clean_date)

# Review values that could not be parsed.
unparsed = df[df["date"].notna() & df["date_clean"].isna()]

if not unparsed.empty:
    print("These date values need review:")
    print(unparsed["date"].drop_duplicates().to_list())

# Save the result.
df.to_csv("output_cleaned.csv", index=False)
```

If the input file contains:

```csv
date
2024-03-01
01/03/2024
March 1 2024
1-3-24
```

The resulting `date_clean` column will contain:

```text
2024-03-01
2024-03-01
2024-03-01
2024-03-01
```

## Important Checks

### Confirm the day/month convention

Dates such as `01/03/2024` are ambiguous:

- Day-first: 1 March 2024
- Month-first: January 3, 2024

The example uses day-first parsing because `1-3-24` is interpreted as 1 March 2024. If the source data uses US month-first dates, change the formats to:

```python
DATE_FORMATS = [
    "%Y-%m-%d",   # 2024-03-01
    "%m/%d/%Y",   # 03/01/2024
    "%B %d %Y",   # March 1 2024
    "%m-%d-%y",   # 3-1-24
]
```

### Do not silently guess invalid dates

Values such as `31/02/2024`, `unknown`, or empty cells cannot be safely converted. The script returns missing values for them and prints the original values requiring review.

### Check two-digit years

Python interprets two-digit years using a defined range, but two-digit years are inherently less clear. Where possible, convert source data to four-digit years before cleaning or review dates such as `1-3-24` against the surrounding records.

## Notes

- Assumed the source column is named `date` and the input file is `input.csv`.
- Assumed slash and hyphen dates use the day-month-year convention.
- The output format is `YYYY-MM-DD`.
- Review the printed unparsed values before using the cleaned file in a dashboard or database.

---

**From the agent:** Delivered by Vetaon: the work is below.
