Here’s the fix, written as if you’d just opened the file and found the bug staring back at you.

---

## The buggy query

Someone on the team wrote this and shipped it:

```sql
SELECT c.name, SUM(o.total) AS total_spent
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.created_at >= NOW() - INTERVAL '90 days'
GROUP BY c.name
ORDER BY total_spent DESC
LIMIT 5;
```

It *looks* right. It runs. It even returns five rows most of the time. But it has a real bug hiding in plain sight.

---

## What’s wrong

The problem is in the `GROUP BY`:

```sql
GROUP BY c.name
```

`customers.name` is not unique. Two different customers can have the same name, and when that happens their orders get merged into one group. The query then reports a single row with their combined total, as if they were one person. Worse, if two same-named customers are both big spenders, you can end up with a “top five” list that actually represents six or seven real customers, and one of them gets squeezed out entirely.

The fix is to group by the primary key and only use `name` for display:

```sql
GROUP BY c.id, c.name
```

Now the database treats every customer as a separate entity, even if they share a name.

---

## The corrected query

```sql
SELECT c.name, SUM(o.total) AS total_spent
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.created_at >= NOW() - INTERVAL '90 days'
GROUP BY c.id, c.name
ORDER BY total_spent DESC
LIMIT 5;
```

That’s the whole fix. One line changed, one bug gone.

---

## Clause by clause

Here’s what each part does, in plain terms.

### `SELECT c.name, SUM(o.total) AS total_spent`

We want two things back: the customer’s name and how much they’ve spent. `SUM(o.total)` adds up every order total for that customer. The `AS total_spent` gives the column a readable name so the result doesn’t come back as `sum`.

### `FROM customers c`

We start with the customers table. The `c` is an alias, so we can write `c.name` instead of `customers.name` everywhere.

### `JOIN orders o ON c.id = o.customer_id`

This connects each customer to their orders. For every row in `customers`, the database finds matching rows in `orders` where `orders.customer_id` equals `customers.id`. If a customer has no orders, they don’t appear in the result at all — which is what we want, since someone with zero orders can’t be a top spender.

### `WHERE o.created_at >= NOW() - INTERVAL '90 days'`

This filters orders to only those placed in the last 90 days. `NOW()` is the current timestamp. Subtracting `INTERVAL '90 days'` gives us the cutoff point. Any order older than that is ignored before the sums are calculated.

One thing to note: this uses `created_at`, not a hypothetical `completed_at` or `paid_at`. If the orders table has a status field, you might want to add something like `AND o.status = 'completed'` to avoid counting cancelled orders. But based on the schema given, `created_at` is the right column.

### `GROUP BY c.id, c.name`

This is the fixed line. It tells the database to create one group per unique customer, using the primary key `id` as the real identifier. Including `c.name` alongside `c.id` is allowed because `id` determines `name` — every `id` maps to exactly one `name`, so there’s no ambiguity.

### `ORDER BY total_spent DESC`

Sorts the groups from highest total to lowest. `DESC` means descending, so the biggest spender is first.

### `LIMIT 5`

Keeps only the top five rows after sorting. That gives us exactly the five customers with the highest total spend in the window.

---

## Why grouping by `id` matters

Let’s make it concrete. Say you have:

| id | name   |
|----|--------|
| 1  | Jordan Lee |
| 2  | Jordan Lee |

And orders:

| id | customer_id | total |
|----|-------------|-------|
| 101 | 1 | 200 |
| 102 | 1 | 150 |
| 103 | 2 | 400 |

With the buggy query, both Jordans merge into one group:

```
Jordan Lee | 750
```

With the fixed query, they stay separate:

```
Jordan Lee | 400
Jordan Lee | 350
```

The second version is correct. The first version would report a single customer with a phantom combined total, and if both Jordans were in the top five, one real customer would be missing from the list.

---

## What I’d check before calling this done

- **Indexes**: If `orders` is large, you’ll want an index on `orders.customer_id` and one on `orders.created_at`. Without them, the `JOIN` and the date filter will scan far more rows than necessary.
- **Time zone**: `NOW()` uses the database’s time zone. If orders are stored in UTC but the business reports in local time, the 90-day window might be off by a few hours. Usually fine, but worth knowing.
- **Ties**: If two customers have the exact same total and both sit at the fifth spot, `LIMIT 5` will arbitrarily pick one. If ties matter, consider `ORDER BY total_spent DESC, c.id ASC` to make the result deterministic.

---

That’s the fix and the reasoning. One line changed, but it’s the kind of line that quietly corrupts reports if left alone.

---

**From the agent:** Delivered by Xaro: the work is below.
