## Query — PostgreSQL

Assumption: “last 90 days” means the rolling 90-day period ending at the supplied `as_of` time, including orders exactly at either boundary. Customers with no qualifying orders are omitted. Pass the current time as `$1` in normal use.

```sql
WITH params AS (
    SELECT $1::timestamptz AS as_of
)
SELECT
    c.id,
    c.name,
    SUM(o.total) AS total_spent
FROM customers AS c
JOIN orders AS o
    ON o.customer_id = c.id
CROSS JOIN params AS p
WHERE o.created_at >= p.as_of - INTERVAL '90 days'
  AND o.created_at <= p.as_of
GROUP BY c.id, c.name
ORDER BY total_spent DESC, c.id ASC
LIMIT 5;
```

### Clause-by-clause

- **`WITH params AS (...)`** captures the reference time once, making the query’s window consistent and the query easier to test. Use the current timestamp for `$1`.
- **`SELECT`** returns each customer’s ID, name, and summed order total.
- **`FROM ... JOIN`** matches customers to their orders; customers without orders in the period will not appear.
- **`CROSS JOIN params`** makes the reference time available to the date filter.
- **`WHERE`** keeps orders from the inclusive 90-day window and excludes future-dated orders.
- **`GROUP BY`** collects qualifying orders per customer.
- **`ORDER BY`** ranks customers by spending, with customer ID as a stable tie-breaker.
- **`LIMIT 5`** returns at most five customers. Ties at fifth place are resolved by ID rather than returning extra rows.

## Edge-case tests for this query

Run each case with a fixed `$1` value, such as `2026-11-14 12:00:00+00`.

| Case | Fixture / condition | Expected result |
|---|---|---|
| Lower boundary | An order is timestamped exactly `as_of - 90 days` | It is included in the customer’s total. |
| Just outside window | An order is timestamped just before `as_of - 90 days` | It is excluded. |
| Future order | An order is timestamped after `as_of` | It is excluded. |
| Multiple orders | One customer has several qualifying orders | Their `total_spent` is the sum of those orders only. |
| Fewer than five customers | Only three customers have qualifying orders | Three rows are returned. |
| More than five, tied totals | Several customers tie for fifth; their IDs differ | Exactly five rows are returned, with lower IDs winning the tie. |

---

**From the agent:** Delivered by Orlix: the work is below.
