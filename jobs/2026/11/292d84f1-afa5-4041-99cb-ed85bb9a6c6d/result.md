## Summary

This guide provides a resilient Python pipeline for normalizing columns containing mixed date representations—such as ISO strings, slash-separated values, written month names, and two-digit shorthand years—into a unified standard format (`YYYY-MM-DD`). It uses modern `pandas` features alongside `python-dateutil` to parse variable strings gracefully while capturing unparseable values. Run the included script directly against your pandas DataFrame or CSV extract to standardize your temporal fields in minutes.

---

## Why Mixed Dates Happen (and How to Tame Them)

In any exploratory run across legacy ledgers or user-submitted forms, date columns rarely follow a single rule. You might see an ISO timestamp (`2024-03-01`), a British or European style entry (`01/03/2024`), long-form text (`March 1 2024`), and condensed shorthand (`1-3-24`) sitting in adjacent rows.

The main obstacle is structural ambiguity. A string like `01/03/2024` can represent March 1 or January 3, depending on the origin district or system locale. To solve this reliably, an effective cleaning pipeline follows three steps:

1. **Establish Locale Conventions**: Decide upfront whether ambiguous numeric splits (`01/03/2024` or `1-3-24`) default to `DD/MM/YYYY` (day first) or `MM/DD/YYYY` (month first).
2. **Apply Mixed Parsing**: Leverage a parser that handles diverse text layouts dynamically without failing on the entire column when encountering an unusual token.
3. **Format and Audit**: Convert valid timestamps into canonical ISO 8601 (`YYYY-MM-DD`) strings and flag invalid rows for human or manual inspection.

---

## The Python Solution

Modern `pandas` (version 2.0 and later) natively supports flexible parsing via the `format="mixed"` parameter in `pd.to_datetime()`. For older installations or bespoke control over ambiguous dates, `dateutil.parser` offers granular fallback logic. 

Here is a self-contained, robust script that handles both:

```python
import pandas as pd
from dateutil import parser

def standardize_date(val, day_first=False):
    """
    Safely parses a mixed-format date string into YYYY-MM-DD.
    Returns None if the value cannot be resolved.
    """
    if pd.isna(val) or str(val).strip() == "":
        return None
    
    # Fast-path string cleanup
    clean_val = str(val).strip()
    
    try:
        # fuzzy=True ignores stray tokens or noise surrounding the date
        parsed_dt = parser.parse(clean_val, dayfirst=day_first, fuzzy=True)
        return parsed_dt.strftime("%Y-%m-%d")
    except (ValueError, TypeError, OverflowError):
        return None


def clean_date_column(df: pd.DataFrame, column_name: str, day_first: bool = False) -> pd.DataFrame:
    """
    Cleans a target column containing mixed dates into ISO 8601 format.
    Adds an audit trail column for validation.
    """
    result_df = df.copy()
    
    # Primary transformation
    result_df[f"{column_name}_clean"] = result_df[column_name].apply(
        lambda x: standardize_date(x, day_first=day_first)
    )
    
    # Audit flag: mark rows that could not be parsed
    result_df[f"{column_name}_is_valid"] = result_df[f"{column_name}_clean"].notna()
    
    return result_df


# --- Demonstration Run ---
if __name__ == "__main__":
    # Sample data reflecting the formats from your brief
    raw_data = {
        "record_id": [101, 102, 103, 104, 105, 106],
        "event_date": [
            "2024-03-01",     # ISO standard
            "01/03/2024",     # Slash format (ambiguous without dayfirst flag)
            "March 1 2024",   # Full text month
            "1-3-24",         # Shorthand two-digit year
            "Invalid Date",   # Noise / Bad data
            None              # Missing entry
        ]
    }

    df = pd.DataFrame(raw_data)
    
    # Run the cleaning pipeline (assuming day-first convention: 01/03/2024 -> March 1, 2024)
    cleaned_df = clean_date_column(df, column_name="event_date", day_first=True)
    
    print("--- Cleaned DataFrame Output ---")
    print(cleaned_df[["record_id", "event_date", "event_date_clean", "event_date_is_valid"]])
```

---

## Execution Walkthrough & Expected Output

When running the pipeline with `day_first=True`, the parser maps each entry systematically:

| record_id | event_date     | event_date_clean | event_date_is_valid | Interpretation |
| :--- | :--- | :--- | :--- | :--- |
| `101` | `2024-03-01`   | `2024-03-01`     | `True`              | Direct ISO mapping |
| `102` | `01/03/2024`   | `2024-03-01`     | `True`              | Interpreted as 1st of March |
| `103` | `March 1 2024` | `2024-03-01`     | `True`              | Word-boundary parsed correctly |
| `104` | `1-3-24`       | `2024-03-01`     | `True`              | Expanded 2-digit year (2024) |
| `105` | `Invalid Date` | `None`           | `False`             | Gracefully isolated |
| `106` | `None`         | `None`           | `False`             | Handled as missing value |

If your source records originate from a US-dominant locale (where `01/03/2024` means January 3rd), simply toggle `day_first=False`.

---

## Notes

- **Ambiguity Convention**: In this script, `day_first=True` was used under the assumption that `01/03/2024` corresponds to the same date as `2024-03-01` and `March 1 2024` in your sample list. Verify whether your source records follow American (`MM/DD/YYYY`) or International (`DD/MM/YYYY`) patterns before deploying at scale.
- **Two-Digit Century Pivot**: The parser interprets two-digit years (such as `-24`) relative to modern decades. If your records span historical data from the 1900s, configure a custom pivot threshold or explicitly add century prefixes before parsing.
- **Performance at Scale**: For tables containing more than 500,000 rows, use `pd.to_datetime(df['date'], format='mixed', errors='coerce')` in pandas 2.0+ as a vectorised alternative, using this custom parsing loop primarily for non-standard fallbacks.

---

**From the agent:** Delivered by Orion: the work is below.
