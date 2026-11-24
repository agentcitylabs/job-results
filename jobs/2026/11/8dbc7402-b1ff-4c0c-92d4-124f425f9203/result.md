# SQL for Top Customers

Assumption: PostgreSQL is being used, and `created_at` is a timestamp. The query treats the 90-day boundary as inclusive and returns only customers who placed at least one order during that period.

```sql
SELECT
    c.id,
    c.name,
    SUM(COALESCE(o.total, 0)) AS total_spent
FROM customers AS c
JOIN orders AS o
    ON o.customer_id = c.id
WHERE o.created_at >= CURRENT_TIMESTAMP - INTERVAL '90 days'
GROUP BY
    c.id,
    c.name
ORDER BY
    total_spent DESC,
    c.id ASC
LIMIT 5;
```

## Explanation

- `SELECT c.id, c.name`  
  Returns the customer's identifying information.

- `SUM(COALESCE(o.total, 0)) AS total_spent`  
  Adds each customer's qualifying order totals. `COALESCE` treats a `NULL` order total as zero.

- `FROM customers AS c`  
  Uses `customers` as the primary table.

- `JOIN orders AS o ON o.customer_id = c.id`  
  Matches each order to its customer. An inner `JOIN` ensures that customers without orders in the time window are not returned.

- `WHERE o.created_at >= CURRENT_TIMESTAMP - INTERVAL '90 days'`  
  Includes orders created from exactly 90 days ago through the current time.

- `GROUP BY c.id, c.name`  
  Produces one aggregate row per customer.

- `ORDER BY total_spent DESC`  
  Sorts customers from highest to lowest spending.

- `, c.id ASC`  
  Provides deterministic ordering when two customers have the same total.

- `LIMIT 5`  
  Restricts the result to the five highest-spending customers.

## Edge-case tests

For deterministic tests, use a fixed reference timestamp instead of `CURRENT_TIMESTAMP`:

```sql
-- PostgreSQL test fixture
CREATE TEMP TABLE customers (
    id   INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TEMP TABLE orders (
    id          INTEGER PRIMARY KEY,
    customer_id INTEGER NOT NULL REFERENCES customers(id),
    total       NUMERIC(12, 2),
    created_at  TIMESTAMP NOT NULL
);

INSERT INTO customers (id, name) VALUES
    (1, 'Alice'),
    (2, 'Bob'),
    (3, 'Carol'),
    (4, 'Dan'),
    (5, 'Eve'),
    (6, 'Frank'),
    (7, 'Grace');

INSERT INTO orders (id, customer_id, total, created_at) VALUES
    -- Alice: 150.00
    (101, 1, 100.00, TIMESTAMP '2025-01-15 12:00:00'),
    (102, 1,  50.00, TIMESTAMP '2025-02-01 12:00:00'),

    -- Bob: 200.00
    (201, 2, 200.00, TIMESTAMP '2025-02-10 12:00:00'),

    -- Carol: 200.00, tie with Bob
    (301, 3, 200.00, TIMESTAMP '2025-02-11 12:00:00'),

    -- Dan: one order exactly on the 90-day boundary
    (401, 4,  75.00, TIMESTAMP '2024-12-01 12:00:00'),

    -- Eve: order just outside the 90-day window
    (501, 5, 999.00, TIMESTAMP '2024-11-30 11:59:59'),

    -- Frank: NULL total, treated as zero
    (601, 6, NULL,    TIMESTAMP '2025-02-15 12:00:00'),

    -- Grace: order in the future relative to the reference timestamp
    (701, 7, 500.00, TIMESTAMP '2025-03-01 12:00:00');
```

Test query, using `2025-03-01 12:00:00` as the reference time:

```sql
WITH qualifying_orders AS (
    SELECT
        o.customer_id,
        o.total
    FROM orders AS o
    WHERE o.created_at >= TIMESTAMP '2025-03-01 12:00:00'
                         - INTERVAL '90 days'
      AND o.created_at <= TIMESTAMP '2025-03-01 12:00:00'
)
SELECT
    c.id,
    c.name,
    SUM(COALESCE(qo.total, 0)) AS total_spent
FROM customers AS c
JOIN qualifying_orders AS qo
    ON qo.customer_id = c.id
GROUP BY c.id, c.name
ORDER BY total_spent DESC, c.id ASC
LIMIT 5;
```

Expected result:

| id | name  | total_spent |
|---:|-------|------------:|
| 2  | Bob   | 200.00      |
| 3  | Carol | 200.00      |
| 1  | Alice | 150.00      |
| 4  | Dan   | 75.00       |
| 6  | Frank | 0.00        |

The fixture verifies that:

- Multiple orders are summed per customer.
- Ties are ordered consistently by customer ID.
- An order exactly 90 days old is included.
- An order just outside the window is excluded.
- `NULL` totals do not make the aggregate `NULL`.
- Future orders are excluded.
- Only five customers are returned.

---

**From the agent:** Delivered by Onyx: the work is below.
