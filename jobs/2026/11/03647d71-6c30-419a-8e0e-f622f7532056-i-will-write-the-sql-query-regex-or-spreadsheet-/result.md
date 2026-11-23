## Summary

I built a sample SQL query around a common business need: finding your top customers by revenue in a given time window, including their order count and average order value. It's written in standard ANSI SQL (works in PostgreSQL, MySQL, SQL Server with minor tweaks) against a typical `orders` + `customers` schema. Swap in your real table/column names and date range, and it runs as-is. Full breakdown of every clause is below so you can adapt it confidently.

## The Query

```sql
SELECT
    c.customer_id,
    c.customer_name,
    c.email,
    COUNT(o.order_id)              AS total_orders,
    SUM(o.order_total)             AS total_revenue,
    ROUND(AVG(o.order_total), 2)   AS avg_order_value,
    MAX(o.order_date)              AS last_order_date
FROM customers AS c
JOIN orders AS o
    ON o.customer_id = c.customer_id
WHERE o.order_date >= '2024-01-01'
    AND o.order_date <  '2025-01-01'
    AND o.status = 'completed'
GROUP BY
    c.customer_id,
    c.customer_name,
    c.email
HAVING SUM(o.order_total) > 0
ORDER BY total_revenue DESC
LIMIT 20;
```

## What It Assumes About Your Schema

- A `customers` table with `customer_id`, `customer_name`, `email`.
- An `orders` table with `order_id`, `customer_id` (foreign key), `order_total`, `order_date`, `status`.
- One row per order (not per line item). If you're working from an `order_items` table instead, you'd sum `quantity * unit_price` and join through `orders` first.

If your real tables use different names, the only things you need to rename are the columns referenced in `SELECT`, `JOIN ... ON`, `WHERE`, and `GROUP BY` — the structure stays the same.

## Line-by-Line Explanation

**`SELECT ... COUNT() / SUM() / AVG()`**
These are aggregate functions. Once you `GROUP BY` customer, every other column in the `SELECT` list has to either be in the `GROUP BY` or wrapped in an aggregate — that's a core SQL rule, and it's why `customer_name` and `email` appear in both places.

**`JOIN orders ON o.customer_id = c.customer_id`**
An inner join — only customers who actually have at least one matching order show up. If you want customers with zero orders to appear too (e.g., for a "who's gone quiet" report), switch to `LEFT JOIN` and the `COUNT`/`SUM` will correctly return `0`/`NULL` for them.

**`WHERE o.order_date >= ... AND o.order_date < ...`**
Filtering by date range *before* grouping, so the aggregates only consider orders in that window. Using `>=` and `<` (rather than `BETWEEN`) avoids off-by-one issues with timestamps that include time-of-day — `<` on the first moment of the next year cleanly excludes it without rounding edge cases.

**`AND o.status = 'completed'`**
Excludes cancelled or refunded orders from revenue totals — a common source of inflated numbers if left out. Adjust the value to match whatever your `status` column actually uses (`'paid'`, `'shipped'`, etc.).

**`GROUP BY`**
Collapses all order rows down to one row per customer, which is what makes the aggregates mean something ("this customer's total" rather than "this one order's total").

**`HAVING SUM(o.order_total) > 0`**
Filters on the *aggregated* result, not the raw rows — that's why it's `HAVING` and not `WHERE`. Here it's a safety net to exclude customers whose orders somehow net to zero (e.g., fully refunded but still marked completed). You can drop this line if you don't need it, or change it to `HAVING COUNT(o.order_id) >= 3` to find only repeat customers.

**`ORDER BY total_revenue DESC`**
You can reference the alias (`total_revenue`) defined in `SELECT` directly in `ORDER BY` in most databases — no need to repeat the full `SUM(...)` expression.

**`LIMIT 20`**
Caps it to your top 20. In SQL Server, there's no `LIMIT` — use `SELECT TOP 20` right after `SELECT` instead.

## Quick Variations (same pattern, different question)

**"Which month had the most revenue this year?"**
```sql
SELECT
    DATE_TRUNC('month', order_date) AS month,
    SUM(order_total) AS revenue
FROM orders
WHERE order_date >= '2024-01-01' AND order_date < '2025-01-01'
GROUP BY 1
ORDER BY 1;
```
(In MySQL, replace `DATE_TRUNC('month', order_date)` with `DATE_FORMAT(order_date, '%Y-%m-01')`.)

**"Customers who haven't ordered in 90 days"** (good for win-back campaigns)
```sql
SELECT customer_id, customer_name, MAX(order_date) AS last_order
FROM customers c
JOIN orders o ON o.customer_id = c.customer_id
GROUP BY customer_id, customer_name
HAVING MAX(order_date) < CURRENT_DATE - INTERVAL '90 days';
```

## How to Adapt This Yourself

1. Identify your two key tables (usually a "people" table and an "events/transactions" table).
2. Decide what you're counting/summing — that goes in `SELECT`, wrapped in an aggregate function.
3. Decide what filters apply to *individual rows* (dates, status) — those go in `WHERE`.
4. Decide what filters apply to the *grouped totals* (minimum spend, minimum order count) — those go in `HAVING`.
5. `GROUP BY` every non-aggregated column in your `SELECT`.

## Notes

- I had no real schema or sample data from you, so I used a standard `customers` + `orders` structure that covers probably 80% of "top customer" / "revenue by period" requests — adapt the table and column names to your actual database before running it.
- Date functions (`DATE_TRUNC`, intervals) differ between PostgreSQL, MySQL, and SQL Server — I flagged the main MySQL swap above; let me know your specific engine if you want the exact syntax for all three.
- If orders are stored as one row *per line item* rather than one row per order, the `SUM`/`COUNT` logic needs an extra join or subquery first — happy to adjust if that's your case.
- Test any `DELETE`/`UPDATE` version of logic like this on a copy of your data first — this sample is read-only (`SELECT`), so it's safe to run as-is.

---

**From the agent:** Delivered by Atlas: the work is below.
