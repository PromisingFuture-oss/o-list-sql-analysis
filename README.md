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
WHERE o.order_status = 'delivered'
GROUP BY month
ORDER BY month
```
 
**Key decision / tricky part:**
For the tricky part it was my first time tackling an sql question where I have to deal with dates so I researched what I could use, until I stumble upon date_trunc
 
**Result:**
| total            | month                  |
| ---------------- | ---------------------- |
| 46566.71         | 2016-10-01 00:00:00+00 |
| 19.62            | 2016-12-01 00:00:00+00 |
| 127545.67        | 2017-01-01 00:00:00+00 |
| 271298.649999999 | 2017-02-01 00:00:00+00 |
| 414369.390000001 | 2017-03-01 00:00:00+00 |
| 390952.18        | 2017-04-01 00:00:00+00 |
| 567066.730000001 | 2017-05-01 00:00:00+00 |
| 490225.600000003 | 2017-06-01 00:00:00+00 |
| 566403.930000002 | 2017-07-01 00:00:00+00 |
| 646000.61        | 2017-08-01 00:00:00+00 |
| 701169.989999999 | 2017-09-01 00:00:00+00 |
| 751140.270000001 | 2017-10-01 00:00:00+00 |
| 1153528.05       | 2017-11-01 00:00:00+00 |
| 843199.170000001 | 2017-12-01 00:00:00+00 |
| 1078606.86       | 2018-01-01 00:00:00+00 |
| 966510.880000002 | 2018-02-01 00:00:00+00 |
| 1120677.99999999 | 2018-03-01 00:00:00+00 |
| 1132933.95       | 2018-04-01 00:00:00+00 |
| 1128836.69       | 2018-05-01 00:00:00+00 |
| 1012090.68       | 2018-06-01 00:00:00+00 |
| 1027903.86       | 2018-07-01 00:00:00+00 |
| 985414.279999999 | 2018-08-01 00:00:00+00 |
 
**What I'd do differently / follow-up questions:**
I probably would round up the decimal value for better readability

## Business Question 2: Top product categories
 
**Question I was trying to answer:**
Which product categories generate the most revenue, and which sell the most units?
 
**Why it matters:**
Finding out which categories provide the most revenue and sells the most units allows a company to focus on those items in order not to run out and make the most profit out of it
 
**Approach:**
I created two sql queries for this, the first query focuses on which units sells the most while the second query focuses on which units generates the most revenue
 
```sql
SELECT p.product_category_name, RANK()OVER(ORDER BY COUNT(oi.order_id) DESC) AS RANK
FROM order_items oi
JOIN products p ON p.product_id = oi.product_id
GROUP BY p.product_category_name

SELECT RANK()OVER(ORDER BY SUM(oi.freight_value + oi.price) DESC) AS rank, SUM(oi.freight_value + oi.price) AS total_revenue, p.product_category_name
FROM order_items oi
JOIN products p ON p.product_id = oi.product_id
GROUP BY p.product_category_name
```
 
**Key decision / tricky part:**
I got stuck in this question because I didn't know how to find the true revenue at first but when I research on it you can find the true revenue of a product if you combine freight value (shipping and delivery cost)
 
**Result: Most Sold**
| product_category_name                          | number_of_sold | rank |
| ---------------------------------------------- | -------------- | ---- |
| cama_mesa_banho                                | 11115          | 1    |
| beleza_saude                                   | 9670           | 2    |
| esporte_lazer                                  | 8641           | 3    |
| moveis_decoracao                               | 8334           | 4    |
| informatica_acessorios                         | 7827           | 5    |
| utilidades_domesticas                          | 6964           | 6    |
| relogios_presentes                             | 5991           | 7    |
| telefonia                                      | 4545           | 8    |
| ferramentas_jardim                             | 4347           | 9    |
| automotivo                                     | 4235           | 10   |
| brinquedos                                     | 4117           | 11   |
| cool_stuff                                     | 3796           | 12   |
| perfumaria                                     | 3419           | 13   |
| bebes                                          | 3065           | 14   |
| eletronicos                                    | 2767           | 15   |
| papelaria                                      | 2517           | 16   |
| fashion_bolsas_e_acessorios                    | 2031           | 17   |
| pet_shop                                       | 1947           | 18   |
| moveis_escritorio                              | 1691           | 19   |
| null                                           | 1603           | 20   |
| consoles_games                                 | 1137           | 21   |

 **Result: Generates Most Revenue**
| rank | total_revenue    | product_category_name                          |
| ---- | ---------------- | ---------------------------------------------- |
| 1    | 1441248.07       | beleza_saude                                   |
| 2    | 1305541.61000001 | relogios_presentes                             |
| 3    | 1241681.72       | cama_mesa_banho                                |
| 4    | 1156656.48       | esporte_lazer                                  |
| 5    | 1059272.4        | informatica_acessorios                         |
| 6    | 902511.789999999 | moveis_decoracao                               |
| 7    | 778397.769999998 | utilidades_domesticas                          |
| 8    | 719329.95        | cool_stuff                                     |
| 9    | 685384.319999998 | automotivo                                     |
| 10   | 584219.210000001 | ferramentas_jardim                             |
| 11   | 561372.55        | brinquedos                                     |
| 12   | 480117.999999998 | bebes                                          |
| 13   | 453338.710000004 | perfumaria                                     |
| 14   | 394883.320000006 | telefonia                                      |
| 15   | 342532.650000001 | moveis_escritorio                              |
| 16   | 277741.710000001 | papelaria                                      |
| 17   | 253876.650000001 | pet_shop                                       |
| 18   | 232799.43        | pcs                                            |
| 19   | 210137.37        | instrumentos_musicais                          |
| 20   | 207705.09        | null                                           |
 
 
**What I'd do differently / follow-up questions:**
I'd probably fix the decimal point and further enhance it as I study more

 ## Business Question 2: Customer Retention
 
**Question I was trying to answer:** Find out how many customers are repeat buyers vs one-time buyers.

**Why it matters:** Because companies spends a lot on advertisements they need to know if the advertisement works and if not find out why is it because of the product, delivering, price etc.,
 
**Approach:**
---
 
```sql
WITH count AS(
SELECT c.customer_unique_id, COUNT(o.order_id) AS count_of_orders
FROM customer c
JOIN orders o ON o.customer_id = c.customer_id
GROUP BY customer_unique_id
)
SELECT
CASE
WHEN count_of_orders = 1 THEN 'OTB'
WHEN count_of_orders >= 2 THEN 'RB'
END AS OTBorRB,
COUNT(*)
FROM count
GROUP BY OTBorRB
```
 
**Key decision / tricky part:**
---
 
**Result:**
| otborrb | count |
| ------- | ----- |
| OTB     | 93099 |
| RB      | 2997  |
 
 
**What I'd do differently / follow-up questions:**
I'd probably fix the decimal point and further enhance it as I study more

