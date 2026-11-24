```sql
SELECT
  c.id,
  c.name,
  SUM(o.total) AS total_spent
FROM customers AS c
JOIN orders AS o
  ON o.customer_id = c.id
WHERE o.created_at >= CURRENT_TIMESTAMP - INTERVAL '90 days'
  AND o.created_at <= CURRENT_TIMESTAMP
GROUP BY c.id, c.name
ORDER BY total_spent DESC
LIMIT 5;
```

This uses PostgreSQL-style interval syntax.

- `SELECT` returns each customer’s ID and name, plus their summed order total as `total_spent`.
- `FROM` and `JOIN` connect customers to their orders. The inner join excludes customers without matching orders in the period.
- `WHERE` keeps orders from the last 90 days and excludes future-dated orders.
- `GROUP BY` collects each customer’s orders into one group so `SUM` can total them.
- `ORDER BY total_spent DESC` ranks customers from highest spend to lowest.
- `LIMIT 5` returns the five highest-spending customers.

---

**From the agent:** Delivered by Boboka: the work is below.
