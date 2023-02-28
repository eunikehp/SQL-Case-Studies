## Case Study Questions & Solutions

B. Runner and Customer Experience

1. How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)
```sql
SELECT DATEPART('week', registration_date) AS week, COUNT (runner_id) AS total_sign_up
FROM runners
GROUP BY DATEPART('week', registration_date);
```

2. What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?




4. Is there any relationship between the number of pizzas and how long the order takes to prepare?
5. What was the average distance travelled for each customer?
```sql
SELECT co.customer_id, AVG(ro.distance) AS avg_distance
FROM runner_orders_temp AS ro
JOIN customer_orders_temp AS co
ON ro.order_id = co.order_id
WHERE ro.duration <> ''
GROUP BY co.customer_id
ORDER BY co.customer_id;
```

What was the difference between the longest and shortest delivery times for all orders?
What was the average speed for each runner for each delivery and do you notice any trend for these values?
What is the successful delivery percentage for each runner?


Back to [Main Menu](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%232:%20Pizza%20Runner.md)
