# `csv_revenue.py`

```python
#!/usr/bin/env python3

import argparse
import csv
import sys
from collections import defaultdict
from decimal import Decimal, InvalidOperation


REQUIRED_COLUMNS = {"date", "product", "quantity", "price"}


def calculate_revenue(csv_path):
    revenue_by_product = defaultdict(Decimal)

    with open(csv_path, "r", newline="", encoding="utf-8") as csv_file:
        reader = csv.DictReader(csv_file)

        if reader.fieldnames is None:
            raise ValueError("The CSV file has no header row.")

        missing_columns = REQUIRED_COLUMNS - set(reader.fieldnames)
        if missing_columns:
            missing = ", ".join(sorted(missing_columns))
            raise ValueError(f"Missing required column(s): {missing}")

        for line_number, row in enumerate(reader, start=2):
            if not row or all(value in (None, "") for value in row.values()):
                continue

            product = (row["product"] or "").strip()
            if not product:
                raise ValueError(f"Line {line_number}: product is empty.")

            try:
                quantity = Decimal((row["quantity"] or "").strip())
                price = Decimal((row["price"] or "").strip())
            except (InvalidOperation, AttributeError):
                raise ValueError(
                    f"Line {line_number}: quantity and price must be numbers."
                )

            revenue_by_product[product] += quantity * price

    return revenue_by_product


def main():
    parser = argparse.ArgumentParser(
        description="Print revenue per product from a CSV file."
    )
    parser.add_argument("csv_file", help="Path to the CSV file")
    args = parser.parse_args()

    try:
        revenue_by_product = calculate_revenue(args.csv_file)
    except (OSError, ValueError) as error:
        print(f"Error: {error}", file=sys.stderr)
        return 1

    for product, revenue in sorted(
        revenue_by_product.items(),
        key=lambda item: item[1],
        reverse=True,
    ):
        print(f"{product}: {revenue:.2f}")

    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

## Usage

Given a file such as:

```csv
date,product,quantity,price
2024-01-01,Notebook,3,4.50
2024-01-02,Pen,10,1.25
2024-01-03,Notebook,2,4.50
```

Run:

```bash
python csv_revenue.py sales.csv
```

Output:

```text
Notebook: 22.50
Pen: 12.50
```

---

**From the agent:** Delivered by Juelro: the work is below.
