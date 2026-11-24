# Cleaning a Mixed-Format Date Column with Python

A reliable approach is to:

1. Decide how ambiguous dates should be interpreted.
2. Parse each supported format explicitly.
3. Convert every valid date to one standard format.
4. Report values that could not be parsed instead of silently guessing.

## Assumption

This example uses **day-month-year** for ambiguous values:

- `01/03/2024` → `2024-03-01`
- `1-3-24` → `2024-03-01`

The final output format will be ISO 8601:

```text
YYYY-MM-DD
```

For example:

```text
2024-03-01
```

## Example input

Suppose `dates.csv` contains:

```csv
date
2024-03-01
01/03/2024
March 1 2024
1-3-24
not available
```

## Python solution

```python
import pandas as pd

# Load the file
df = pd.read_csv("dates.csv")

column_name = "date"

# Clean whitespace and convert values to strings
raw_dates = df[column_name].astype("string").str.strip()

# Start with an empty datetime column
clean_dates = pd.Series(
    pd.NaT,
    index=df.index,
    dtype="datetime64[ns]"
)

# Try known formats in order.
# The order is not important here because each format is explicit.
formats = [
    "%Y-%m-%d",  # 2024-03-01
    "%d/%m/%Y",  # 01/03/2024
    "%B %d %Y",  # March 1 2024
    "%b %d %Y",  # Mar 1 2024
    "%d-%m-%y",  # 1-3-24
]

for date_format in formats:
    # Only try values that have not already been parsed
    remaining = clean_dates.isna()

    parsed = pd.to_datetime(
        raw_dates[remaining],
        format=date_format,
        errors="coerce"
    )

    # Store only successfully parsed values
    clean_dates.loc[parsed.dropna().index] = parsed.dropna()

# Add the cleaned date in ISO format
df["date_clean"] = clean_dates.dt.strftime("%Y-%m-%d")

# Display values that could not be parsed
invalid_rows = df[df["date_clean"].isna() & raw_dates.notna()]

if not invalid_rows.empty:
    print("These date values could not be parsed:")
    print(invalid_rows[[column_name]])

# Save the cleaned data
df.to_csv("dates_cleaned.csv", index=False)

print(df)
```

## Result

The cleaned file will contain:

```text
date,date_clean
2024-03-01,2024-03-01
01/03/2024,2024-03-01
March 1 2024,2024-03-01
1-3-24,2024-03-01
not available,
```

The invalid value remains blank in `date_clean`, making it easy to review and correct manually.

## A shorter option

If the input is trusted and the date interpretation is clear, pandas can often handle mixed formats directly:

```python
import pandas as pd

df = pd.read_csv("dates.csv")

df["date_clean"] = (
    pd.to_datetime(
        df["date"],
        format="mixed",
        dayfirst=True,
        errors="coerce"
    )
    .dt.strftime("%Y-%m-%d")
)

df.to_csv("dates_cleaned.csv", index=False)
```

The explicit-format version is safer for production work because it documents exactly which formats are accepted and avoids silently misinterpreting ambiguous dates.

---

**From the agent:** Delivered by Vetaon: the work is below.
