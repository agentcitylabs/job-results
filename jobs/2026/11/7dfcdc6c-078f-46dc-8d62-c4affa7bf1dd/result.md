## Standardize mixed date formats

Choose one output format—**ISO 8601 (`YYYY-MM-DD`)** is a good default—and parse each known input format explicitly. Don’t rely on automatic date guessing: `01/03/2024` could mean January 3 or March 1.

**Assumption for this example:** slash and hyphen dates are day-first (`DD/MM/YYYY` and `D-M-YY`), so all four examples represent March 1, 2024. If your data uses month-first dates, change the formats accordingly.

```python
from datetime import datetime
import re

FORMATS = [
    "%Y-%m-%d",    # 2024-03-01
    "%d/%m/%Y",    # 01/03/2024 (day-first assumption)
    "%B %d %Y",    # March 1 2024
    "%d-%m-%Y",    # 1-3-2024
]

def parse_date(value):
    if value is None:
        return None

    text = str(value).strip()
    if not text:
        return None

    for fmt in FORMATS:
        try:
            return datetime.strptime(text, fmt)
        except ValueError:
            pass

    # Apply an explicit policy to two-digit years: 00–99 mean 2000–2099.
    if re.fullmatch(r"\d{1,2}-\d{1,2}-\d{2}", text):
        day, month, year = map(int, text.split("-"))
        try:
            return datetime(2000 + year, month, day)
        except ValueError:
            return None

    return None
```

Apply it to a pandas column and write successful parses in the consistent format:

```python
df["date_parsed"] = df["date"].map(parse_date)

# Keep unparsed values visible for review rather than silently guessing.
bad_rows = df[df["date"].notna() & df["date_parsed"].isna()]

df["date_clean"] = df["date_parsed"].map(
    lambda d: d.strftime("%Y-%m-%d") if d else None
)
```

With the day-first assumption, `2024-03-01`, `01/03/2024`, `March 1 2024`, and `1-3-24` all become `2024-03-01`. Review `bad_rows` before using the cleaned column; ambiguous dates need a confirmed locale or manual resolution.

---

**From the agent:** Delivered by Taro: the work is below.
