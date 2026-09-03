# o-list-sql-analysis

## Business Question 1: Revenue by Month
 
**Question I was trying to answer:**
Find total revenue for each month?
 
**Why it matters:**
This shows how much revenue or money a business earns by selling goods or services before any expenses are subtracted
 
**Approach:**
I then use date_trunc so that it get filtered down to months and finally filter it down to delivered so it only shows the items that have been paid for
 
```sql
SELECT SUM(payment_value) AS total, date_trunc('month', o.order_purchase_timestamp) AS month
FROM orders o
JOIN order_payments op ON op.order_id = o.order_id
WHERE o.order_status IN ('shipped', 'delivered')
GROUP BY month
ORDER BY month
```
 
**Key decision / tricky part:**
[Anything non-obvious you had to figure out — e.g. "Had to use COUNT(DISTINCT order_id) instead of COUNT(order_id) because joining to order_items caused duplicate rows per order."]
 
**Result:**
[The actual output numbers/table, or a short summary of what it showed.]
 
**What I'd do differently / follow-up questions:**
[Optional — anything you'd explore next if you had more time.]

## Business Question 1: Revenue by Month
 
**Question I was trying to answer:**
Find total revenue for each month?
 
**Why it matters:**
This shows how much revenue or money a business earns by selling goods or services before any expenses are subtracted
 
**Approach:**
I then use date_trunc so that it get filtered down to months and finally filter it down to delivered so it only shows the items that have been paid for
 
```sql
SELECT SUM(payment_value) AS total, date_trunc('month', o.order_purchase_timestamp) AS month
FROM orders o
JOIN order_payments op ON op.order_id = o.order_id
WHERE o.order_status IN ('shipped', 'delivered')
GROUP BY month
ORDER BY month
```
 
**Key decision / tricky part:**
[Anything non-obvious you had to figure out — e.g. "Had to use COUNT(DISTINCT order_id) instead of COUNT(order_id) because joining to order_items caused duplicate rows per order."]
 
**Result:**
[The actual output numbers/table, or a short summary of what it showed.]
 
**What I'd do differently / follow-up questions:**
[Optional — anything you'd explore next if you had more time.]

 
