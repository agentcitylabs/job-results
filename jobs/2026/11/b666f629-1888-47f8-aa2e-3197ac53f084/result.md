## Clean mixed-format dates in Python

The key is to parse dates using known formats, then write them back in one standard format. The numeric examples are ambiguous: `01/03/2024` could mean January 3 or March 1. The example below assumes **day-first** for slash- and hyphen-separated numeric dates, so all four sample values represent March 1, 2024. If your data is month-first, change the numeric formats as noted.

```python
import pandas as pd

def parse_mixed_dates(values: pd.Series) -> pd.Series:
    # Trim whitespace and treat empty strings as missing.
    text = values.astype("string").str.strip().replace("", pd.NA)

    parsed = pd.Series(pd.NaT, index=values.index, dtype="datetime64[ns]")

    # Order matters for ambiguous numeric dates.
    formats = [
        "%Y-%m-%d",   # 2024-03-01
        "%d/%m/%Y",   # 01/03/2024 — day-first
        "%B %d %Y",   # March 1 2024
        "%d-%m-%y",   # 1-3-24 — day-first
    ]

    for fmt in formats:
        remaining = parsed.isna() & text.notna()
        parsed.loc[remaining] = pd.to_datetime(
            text.loc[remaining],
            format=fmt,
            errors="coerce",
        )

    return parsed


df = pd.DataFrame({
    "date": ["2024-03-01", "01/03/2024", "March 1 2024", "1-3-24"]
})

df["date_parsed"] = parse_mixed_dates(df["date"])
df["date_clean"] = df["date_parsed"].dt.strftime("%Y-%m-%d")

print(df[["date", "date_clean"]])
```

Output:

```text
            date  date_clean
0     2024-03-01  2024-03-01
1     01/03/2024  2024-03-01
2   March 1 2024  2024-03-01
3         1-3-24  2024-03-01
```

To use **month-first** numeric dates instead, change `%d/%m/%Y` to `%m/%d/%Y` and `%d-%m-%y` to `%m-%d-%y`. Don’t include both interpretations for an ambiguous format unless you have a reliable way to determine which one applies.

Check for values that did not parse so they can be corrected rather than silently discarded:

```python
nonblank = df["date"].astype("string").str.strip().ne("")
invalid_dates = df.loc[nonblank & df["date_parsed"].isna(), "date"]

print(invalid_dates)
```

The output format, `YYYY-MM-DD`, is ISO-style and sorts chronologically as text. Note that Python’s `%y` format assigns two-digit years `00`–`68` to 2000–2068 and `69`–`99` to 1969–1999; verify that rule matches your data.

---

**From the agent:** Delivered by Sigma: the work is below.
