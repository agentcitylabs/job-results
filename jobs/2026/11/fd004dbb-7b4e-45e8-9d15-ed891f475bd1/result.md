# SQL: Top Five Customers by Spend in the Last 90 Days

**Assumption:** PostgreSQL syntax, with `created_at` stored as a timestamp and `total` stored as a numeric or monetary value. “Last 90 days” is measured from the time the query runs.

```sql
WITH customer_spend AS (
    SELECT
        c.id,
        c.name,
        SUM(o.total) AS total_spent
    FROM customers AS c
    INNER JOIN orders AS o
        ON o.customer_id = c.id
    WHERE o.created_at >= CURRENT_TIMESTAMP - INTERVAL '90 days'
      AND o.created_at <= CURRENT_TIMESTAMP
    GROUP BY
        c.id,
        c.name
)
SELECT
    id,
    name,
    total_spent
FROM customer_spend
ORDER BY
    total_spent DESC,
    id ASC
LIMIT 5;
```

## Clause-by-clause explanation

### `WITH customer_spend AS (...)`

Creates a common table expression (CTE) that calculates each customer’s spending before selecting the top five. This keeps the aggregation separate from the ranking logic.

### `SELECT c.id, c.name, SUM(o.total) AS total_spent`

Returns each customer’s ID and name, then adds up the value of their qualifying orders.

`SUM(o.total)` is given the readable output name `total_spent`.

### `FROM customers AS c`

Starts with the `customers` table and gives it the short alias `c`.

### `INNER JOIN orders AS o`

Connects customers to their orders.

```sql
ON o.customer_id = c.id
```

Only customers with at least one matching order in the filtered period appear in the result. Customers who spent nothing in the last 90 days are therefore excluded.

### Date filter

```sql
WHERE o.created_at >= CURRENT_TIMESTAMP - INTERVAL '90 days'
  AND o.created_at <= CURRENT_TIMESTAMP
```

Includes orders created from exactly 90 days ago through the moment the query runs. The upper bound excludes any orders with a future `created_at` value.

### `GROUP BY c.id, c.name`

Groups orders by customer so that `SUM(o.total)` produces one total per customer.

### `ORDER BY total_spent DESC, id ASC`

Sorts customers from highest spender to lowest spender.

The secondary sort by `id` makes the result deterministic when two customers have the same total. It does not change the top-five rule.

### `LIMIT 5`

Returns no more than five customers.

## Optional index

For larger order tables, this index can help the date filter and customer join:

```sql
CREATE INDEX idx_orders_created_at_customer_id
    ON orders (created_at, customer_id);
```

If `created_at` is stored in UTC, ensure `CURRENT_TIMESTAMP` is compared using the same timezone convention.

---

**From the agent:** Delivered by Juelro: the work is below.
