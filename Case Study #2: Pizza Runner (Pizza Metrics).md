
## Case Study Questions & Solutions

A. Pizza Metrics
1. How many pizzas were ordered?

```sql
SELECT COUNT (pizza_id) AS total_pizza
FROM customer_orders_temp;
```
|total_pizza|
|---|
|14|

2. How many unique customer orders were made?

```sql
SELECT COUNT (DISTINCT order_id) AS total_order
FROM customer_orders_temp;
```
|total_order|
|---|
|10|

3. How many successful orders were delivered by each runner?
```sql
SELECT runner_id, COUNT (order_id) AS total_order
FROM runner_orders_temp
WHERE distance <> ''
GROUP BY runner_id
ORDER BY runner_id;
```

|runner_id	|total_order|
|---|---|
|1|	4|
|2|	3|
|3|	1|

4. How many of each type of pizza was delivered?
```sql
SELECT pn.pizza_name, COUNT (co.pizza_id) AS total_pizza
FROM pizza_names AS pn
JOIN customer_orders_temp AS co
ON pn.pizza_id = co.pizza_id
JOIN runner_orders_temp AS ro
ON co.order_id = ro.order_id
WHERE distance <> ''
GROUP BY pn.pizza_name;
```

|pizza_name |	total_pizza |
|---|---|
|Meatlovers |	9 |
|Vegetarian |	3 |


5. How many Vegetarian and Meatlovers were ordered by each customer?
```sql
SELECT co.customer_id, pn.pizza_name, COUNT(pn.pizza_name) AS total_pizza
FROM customer_orders_temp AS co
JOIN pizza_names AS pn
ON co.pizza_id = pn.pizza_id
GROUP BY co.customer_id, pn.pizza_name
ORDER BY co.customer_id;
```

|customer_id	|pizza_name|	total_pizza|
|---|---|---|
|101	|Meatlovers|	2|
|101|	Vegetarian|	1|
|102|	Meatlovers|	2|
|102|	Vegetarian|	1|
|103|	Meatlovers|	3|
|103|	Vegetarian|	1|
|104|	Meatlovers|	3|
|105|	Vegetarian|	1|

6. What was the maximum number of pizzas delivered in a single order?

```sql
WITH order_per_id AS(
  SELECT co.order_id, COUNT (co.pizza_id) AS total_order
FROM customer_orders_temp AS co
JOIN runner_orders_temp AS ro
ON co.order_id = ro.order_id
WHERE distance <> ''
GROUP BY co.order_id
ORDER BY co.order_id)

SELECT MAX(total_order) AS max_order_per_id
FROM order_per_id;
```
|order_id|	total_order|
|---|---|
|1|	1|
|2|	1|
|3|	2|
|4|	3|
|5|	1|
|7|	1|
|8|	1|
|10|	2|

|max_order_per_id|
|---|
|3|

7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
```sql
SELECT co.customer_id, 
SUM(CASE WHEN co.exclusions <> '' OR co.extras <> '' THEN 1 ELSE 0 END) AS change,
SUM(CASE WHEN co.exclusions = '' AND co.extras = '' THEN 1 ELSE 0 END) AS no_change
FROM customer_orders_temp AS co
JOIN runner_orders_temp AS ro
ON co.order_id = ro.order_id
WHERE distance <> ''
GROUP BY co.customer_id
ORDER BY co.customer_id;
```
|customer_id|	change|	no_change|
|---|---|---|
|101|	0|	2|
|102|	0|	3|
|103|	3|	0|
|104|	2|	1|
|105|	1|	0|


8.How many pizzas were delivered that had both exclusions and extras?
```sql
SELECT co.customer_id, 
SUM(CASE WHEN co.exclusions <> '' AND co.extras <> '' THEN 1 ELSE 0 END) AS change
FROM customer_orders_temp AS co
JOIN runner_orders_temp AS ro
ON co.order_id = ro.order_id
WHERE distance <> ''
GROUP BY co.customer_id
ORDER BY co.customer_id;
```
|customer_id	|change|
|---|---|
|101|	0|
|102|	0|
|103|	0|
|104|	1|
|105|	0|

9. What was the total volume of pizzas ordered for each hour of the day?
```sql
SELECT DATE_PART ('hour', order_time) AS time, SUM (pizza_id) AS total_pizza
FROM customer_orders_temp
GROUP BY time
ORDER BY time;
```

|time|	total_pizza|
|---|---|
|11|	1|
|13|	4|
|18|	3|
|19|	1|
|21|	5|
|23|	4|

10. What was the volume of orders for each day of the week?
```sql
SELECT FORMAT(DATEADD('day',2,order_time),'dddd') AS day_per_week,
COUNT (pizza_id) AS total_pizza_a_day
FROM customer_orders_temp
GROUP BY day_per_week;
```

Back to [Main Menu](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%232:%20Pizza%20Runner.md) 
