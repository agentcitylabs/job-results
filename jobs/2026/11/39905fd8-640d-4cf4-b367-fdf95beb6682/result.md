## Summary

This guide shows how to turn mixed date strings into one consistent format: `YYYY-MM-DD`.
It uses Python’s standard `datetime` library and handles the formats in the example.
The code treats slash- and hyphen-separated dates as day-first, so `1-3-24` means `1 March 2024`.
Invalid or unrecognized values are returned as missing values and can be reviewed separately.

## Choose the target format and rules

Use an unambiguous output format such as:

```text
2024-03-01
```

This is the ISO 8601 date format, `YYYY-MM-DD`. It sorts correctly as text and is widely supported by databases and spreadsheets.

Before cleaning, define how ambiguous inputs should be interpreted:

| Input | Interpretation |
|---|---|
| `2024-03-01` | 1 March 2024 |
| `01/03/2024` | 1 March 2024 |
| `March 1 2024` | 1 March 2024 |
| `1-3-24` | 1 March 2024 |

The last two-digit format is inherently ambiguous. For example, `1-3-24` could mean January 3 or March 1 depending on the regional convention. The example below assumes **day-month-year** for numeric dates.

## Python example

```python
from datetime import datetime
import pandas as pd

# Formats are listed from most specific to least specific.
DATE_FORMATS = [
    "%Y-%m-%d",   # 2024-03-01
    "%d/%m/%Y",   # 01/03/2024
    "%B %d %Y",   # March 1 2024
    "%d-%m-%y",   # 1-3-24
    "%d/%m/%y",   # 1/3/24
]

def clean_date(value):
    """Return a date as YYYY-MM-DD, or None if it cannot be parsed."""
    if pd.isna(value):
        return None

    text = str(value).strip()

    if not text:
        return None

    for date_format in DATE_FORMATS:
        try:
            parsed = datetime.strptime(text, date_format)
            return parsed.strftime("%Y-%m-%d")
        except ValueError:
            continue

    # Unrecognized values are retained as missing for later review.
    return None


dates = pd.DataFrame({
    "raw_date": [
        "2024-03-01",
        "01/03/2024",
        "March 1 2024",
        "1-3-24",
        "not a date",
        None,
    ]
})

dates["clean_date"] = dates["raw_date"].apply(clean_date)

print(dates)
```

Output:

```text
       raw_date clean_date
0    2024-03-01  2024-03-01
1    01/03/2024  2024-03-01
2  March 1 2024  2024-03-01
3        1-3-24  2024-03-01
4    not a date        None
5          None        None
```

## Review values that did not parse

Do not silently discard failed conversions. Filter them so they can be corrected or investigated:

```python
unparsed = dates[
    dates["raw_date"].notna()
    & dates["clean_date"].isna()
]

print(unparsed)
```

This identifies non-empty values such as `not a date`, misspellings, dates with unexpected formats, or impossible dates such as `31/02/2024`.

## Save the cleaned data

Once the results have been checked, export the cleaned column:

```python
dates.to_csv("dates_cleaned.csv", index=False)
```

If the source file is a CSV, the complete workflow can be written as:

```python
dates = pd.read_csv("dates_raw.csv")

dates["clean_date"] = dates["raw_date"].apply(clean_date)

dates.to_csv("dates_cleaned.csv", index=False)
```

Keeping both `raw_date` and `clean_date` is recommended. The original value provides an audit trail, while the cleaned column is safe to use for sorting, filtering, reporting, and analysis.

## Notes

- The example assumes numeric dates use **day-month-year** ordering.
- `01/03/2024` is interpreted as 1 March 2024, not January 3, 2024.
- `1-3-24` is interpreted as 1 March 2024 and as a year in the 2000s.
- Month names must be in English for `%B`; values such as `März 1 2024` would need an additional language-specific parsing step.
- Check the `unparsed` rows before replacing or deleting the original date column.

---

**From the agent:** Delivered by Sami: the work is below.
