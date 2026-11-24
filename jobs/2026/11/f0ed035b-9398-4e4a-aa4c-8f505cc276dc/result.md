# Cleaning a Mixed-Format Date Column

**Deliverable for: "Clean a messy list of dates"**
Target output: one column, ISO 8601 (`YYYY-MM-DD`), plus a true date dtype and an audit trail of what couldn't be parsed.

---

## 1. The one decision you have to make first

Everything else is mechanical. The real problem in your sample is this row:

```
01/03/2024
```

That is either **1 March 2024** (day-first, common in the UK/EU) or **3 January 2024** (month-first, US). No parser can tell you which. Guessing per-row is the single most common way date cleaning goes silently wrong — you get a file that looks clean and is wrong for roughly a third of its rows.

So: **pick a convention for the whole source, write it down, and parse explicitly.**

Two ways to pick it with evidence rather than vibes:

- **Impossible-value test.** Split every `a/b/yyyy` value on the separator and look at the first component. If you see any value `> 12`, the source cannot be month-first. If the second component ever exceeds 12 and the first never does, it's month-first.
- **Provenance.** Ask where the column came from. A form filled in by one region's staff, or an export from one system, is almost always internally consistent.

If the two tests disagree, or both components stay ≤ 12 everywhere, you have a genuinely ambiguous source — flag it to the data owner rather than deciding for them. I'd rather hand back 40 quarantined rows than 40 confidently wrong dates.

**Assumption for this deliverable:** the source is **day-first**, so `01/03/2024` → 2024-03-01 and `1-3-24` → 2024-03-01. Change the `FORMATS` list below if yours is month-first; it's a one-line edit.

Second small decision: **two-digit years.** Python's `%y` maps 00–68 → 2000s and 69–99 → 1900s. That's fine for recent transactional data and dangerous for birth dates. If your data can contain pre-1969 dates, strip two-digit years out and quarantine them instead.

---

## 2. The method: parse in waves, don't guess per row

The instinct is `pd.to_datetime(col)` and hope. Don't. Pandas will either throw, or (in older versions) infer a format from the first row and misapply it, or fall back to row-by-row `dateutil` guessing which is both slow and inconsistent.

Instead: run a **cascade**. Try format #1 against everything, keep the hits, try format #2 against only the remaining misses, and so on. Record which pattern matched each row. Whatever survives all passes is a genuine anomaly you can look at by hand.

This gives you three things a one-liner never does: speed (vectorised C parsing per format), determinism (no per-row guessing), and a diagnostic breakdown of what your messy column actually contains.

---

## 3. Python example

```python
import pandas as pd

# --- sample messy input -------------------------------------------------
raw = pd.Series([
    "2024-03-01",
    "01/03/2024",
    "March 1 2024",
    "1-3-24",
    " 2024-03-01 ",          # stray whitespace
    "1 Mar 2024",
    "March 1, 2024",
    "2024/03/01",
    "",                      # empty
    None,                    # missing
    "n/a",                   # sentinel junk
    "31/02/2024",            # calendar-impossible
    "next Tuesday",          # free text
], name="date_raw")

# --- step 1: normalise the text ----------------------------------------
NULL_TOKENS = {"", "n/a", "na", "null", "none", "-", "unknown", "tbd", "?"}

s = (raw.astype("string")
        .str.strip()
        .str.replace(r"\s+", " ", regex=True)        # collapse inner runs
        .str.replace(",", "", regex=False))          # "March 1, 2024" -> "March 1 2024"

s = s.mask(s.str.lower().isin(NULL_TOKENS))          # junk sentinels -> <NA>

# --- step 2: the format cascade (DAY-FIRST convention) ------------------
FORMATS = [
    "%Y-%m-%d",     # 2024-03-01
    "%Y/%m/%d",     # 2024/03/01
    "%d/%m/%Y",     # 01/03/2024
    "%d-%m-%Y",     # 01-03-2024
    "%d.%m.%Y",     # 01.03.2024
    "%d/%m/%y",     # 01/03/24
    "%d-%m-%y",     # 1-3-24
    "%B %d %Y",     # March 1 2024
    "%b %d %Y",     # Mar 1 2024
    "%d %B %Y",     # 1 March 2024
    "%d %b %Y",     # 1 Mar 2024
]

parsed  = pd.Series(pd.NaT, index=s.index, dtype="datetime64[ns]")
matched = pd.Series(pd.NA,  index=s.index, dtype="string")

for fmt in FORMATS:
    todo = parsed.isna() & s.notna()
    if not todo.any():
        break
    attempt = pd.to_datetime(s[todo], format=fmt, errors="coerce")
    hits = attempt.notna()
    parsed.loc[attempt.index[hits]]  = attempt[hits]
    matched.loc[attempt.index[hits]] = fmt

# --- step 3: assemble the output ---------------------------------------
out = pd.DataFrame({
    "date_raw":    raw,
    "date_clean":  parsed.dt.date.astype("string"),   # 'YYYY-MM-DD' text
    "date_dt":     parsed,                            # real datetime dtype
    "format_used": matched,
})
out["status"] = (
    pd.Series("parsed", index=out.index)
      .mask(s.isna(), "missing")
      .mask(parsed.isna() & s.notna(), "unparsed")
)

print(out[["date_raw", "date_clean", "format_used", "status"]].to_string())
print("\nFormat counts:\n", matched.value_counts(dropna=False))
print("\nQuarantine:\n", out.loc[out.status == "unparsed", "date_raw"].tolist())
```

Output on the sample: the first eight values parse to `2024-03-01`; the empty string, `None` and `"n/a"` come back as `missing`; `"31/02/2024"` and `"next Tuesday"` land in the quarantine list as `unparsed`.

That `31/02/2024` row is the quiet win here. `%d/%m/%Y` matches the *shape* but February has no 31st, so `errors="coerce"` returns `NaT` rather than silently rolling to 2 March. Shape-valid, calendar-invalid dates are exactly the kind of thing that survives a lazier clean.

---

## 4. Handling the leftovers

For a one-off cleanup, read the quarantine list and fix by hand — it's usually a handful of rows and three recurring patterns.

For a recurring pipeline, add formats to the list until the quarantine rate is acceptable, then set a threshold: if more than, say, 1% of rows fail, the job fails loudly rather than shipping a half-clean table. Silent partial success is the enemy.

If you want a last-resort catch-all, append one after the cascade — but keep it *after*, so the explicit formats always win:

```python
from dateutil import parser as du

def loose(v):
    try:
        return pd.Timestamp(du.parse(v, dayfirst=True, fuzzy=False))
    except Exception:
        return pd.NaT

todo = parsed.isna() & s.notna()
parsed.loc[todo]  = s[todo].map(loose)
matched.loc[todo & parsed.notna()] = "dateutil"
```

Tag those rows (`format_used == "dateutil"`) so a reviewer can spot-check them. They're your highest-risk rows, not your cleanest.

---

## 5. Checks before you call it done

Four I run every time, in about a minute:

1. **Row count conservation.** `len(out) == len(raw)` — a cascade should never drop a row.
2. **Range sanity.** `parsed.min()` and `parsed.max()`. Dates in 1900 or 2087 usually mean a two-digit-year misread or a typo'd year.
3. **Day-of-month distribution.** `parsed.dt.day.value_counts()`. If days 1–12 are wildly over-represented relative to 13–31, you have a day/month swap hiding in part of the file.
4. **Round-trip spot check.** Sample 10 parsed rows, print `date_raw` next to `date_clean`, and read them. Eyes catch what assertions don't.

---

## 6. Short version

1. Decide day-first vs month-first from evidence, and record the decision next to the data.
2. Normalise whitespace, punctuation and null-ish tokens to real `<NA>`.
3. Parse in waves with **explicit** formats, `errors="coerce"`, recording which format matched.
4. Store ISO `YYYY-MM-DD` *and* a true date dtype; never ship a date as an ambiguous string alone.
5. Quarantine and review the residue — never let it quietly become `NaT` in production.

The pattern generalises: whenever a column mixes formats, parse explicitly in priority order and keep a column that says *how* each value was interpreted. That provenance column is what lets you defend the numbers three weeks later.

— Sigma

---

**From the agent:** Delivered by Sigma: the work is below.
