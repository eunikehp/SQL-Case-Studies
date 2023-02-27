# Case Study #2: Pizza Runner

## Problem Statement

Danny has a plan to launch Pizza Runner. He started by recruiting “runners” to deliver fresh pizza from Pizza Runner Headquarters (otherwise known as Danny’s house) and also maxed out his credit card to pay freelance developers to build a mobile app to accept orders from customers. 

Because Danny had a few years of experience as a data scientist - he was very aware that data collection was going to be critical for his business’ growth.He has prepared for us an entity relationship diagram of his database design but requires further assistance to clean his data and apply some basic calculations so he can better direct his runners and optimise Pizza Runner’s operations.

## Entity Relationship Diagram

<img width="650" alt="image" src="https://user-images.githubusercontent.com/104567399/220662950-bf47793c-e3e5-43ee-bccc-051c2a12da7a.png">

**Table 1 :** ```runners```

The ```runners``` table shows the ```registration_date``` for each new runner

|runner_id	|registration_date|
|---|---|
|1|	2021-01-01|
|2	|2021-01-03|
|3	|2021-01-08|
|4	|2021-01-15|

**Table 2 :** ```customer_orders```

Customer pizza orders are captured in the ```customer_orders``` table
the ```exclusions``` are the ingredient_id values which should be removed from the pizza and the ```extras``` are the ingredient_id values which need to be added to the pizza.

|order_id|	customer_id|	pizza_id	|exclusions	|extras|	order_time|
|----|---|----|---|---|---|
|1	|101|	1	 |	| |	2021-01-01 18:05:02|
|2	|101|	1	| |	 	|2021-01-01 19:00:52|
|3|	102	|1	| |	 	|2021-01-02 23:51:23|
|3	|102	|2	| 	|null	|2021-01-02 23:51:23|
|4|	103|	1|	4	| 	|2021-01-04 13:23:46|
|4	|103|	1|	4	| |	2021-01-04 13:23:46|
|4|	103|	2	|4|	 |	2021-01-04 13:23:46|
|5|	104|	1|	null	|1	|2021-01-08 21:00:29|
|6|	101|	2|	null	|null	|2021-01-08 21:03:13|
|7	|105|	2	|null	|1	|2021-01-08 21:20:29|
|8|	102|	1|	null|	null|	2021-01-09 23:54:33|
|9|	103|	1	|4|	1, 5|	2021-01-10 11:22:59|
|10	|104|	1|	null	|null	|2021-01-11 18:34:49|
|10|	104|	1	|2, 6|	1, 4	|2021-01-11 18:34:49|

**Table 3 :** ```runner_orders```

After each orders are received through the system - they are assigned to a runner - however not all orders are fully completed and can be cancelled by the restaurant or the customer.

|order_id|	runner_id	|pickup_time|	distance	|duration|	cancellation|
|---|---|---|---|---|---|
|1|	1	|2021-01-01 18:15:34|	20km|	32 minutes	 | |
|2|	1	|2021-01-01 19:10:54|	20km	|27 minutes|	 |
|3|	1	|2021-01-03 00:12:37|	13.4km	|20 mins|	null|
|4|	2	|2021-01-04 13:53:03	|23.4|	40	|null|
|5|	3	|2021-01-08 21:10:57	|10|	15|	null|
|6|	3	|null	|null|	null|	Restaurant Cancellation|
|7|	2	|2020-01-08 21:30:45	|25km	|25mins	|null|
|8	|2	|2020-01-10 00:15:02|	23.4 km	|15 minute|	null|
|9|	2	|null|	null	|null|	Customer Cancellation|
|10|	1|	2020-01-11 18:50:20	|10km	|10minutes	|null|

**Table 4 :** ```pizza_names```

At the moment - Pizza Runner only has 2 pizzas available the Meat Lovers or Vegetarian!

|pizza_id|	pizza_name|
|---|---|
|1	|Meat Lovers|
|2	|Vegetarian|

**Table 5 :** ```pizza_recipes```

Each pizza_id has a standard set of toppings which are used as part of the pizza recipe.

|pizza_id	|toppings|
|---|---|
|1	|1, 2, 3, 4, 5, 6, 8, 10|
|2	|4, 6, 7, 9, 11, 12|

**Table 5 :** ```pizza_toppings```

This table contains all of the topping_name values with their corresponding topping_id value.

|topping_id|	topping_name|
|---|---|
|1	|Bacon|
|2|	BBQ Sauce|
|3	|Beef|
|4	|Cheese|
|5	|Chicken|
|6	|Mushrooms|
|7	|Onions|
|8	|Pepperoni|
|9	|Peppers|
|10	|Salami|
|11	|Tomatoes|
|12	|Tomato Sauce|

## Cleaning the database

If we are looking at the provided table, there is a lot of ```null``` values and missing spaces which leads to unclear explanation of some columns, for example,```exclusions``` & ```extras ``` columns in **Table 2** .
First thing to do before answer the case study question, we need to clean up the data. Therefore, it is necessary to create temporary table.

**Table 2A - customer_orders_temp**

```sql
CREATE TEMP TABLE customer_orders_temp AS
  SELECT order_id, customer_id, pizza_id,
	CASE WHEN exclusions IS null OR exclusions LIKE 'null' THEN '' ELSE exclusions END AS exclusions,
  	CASE WHEN extras IS null OR extras LIKE 'null' THEN '' ELSE extras END AS extras,order_time
FROM customer_orders;
```

|order_id|	customer_id|	pizza_id	|exclusions	|extras|	order_time|
|----|---|----|---|---|---|
|1	|101|	1	 |	| |	2021-01-01 18:05:02|
|2	|101|	1	| |	 	|2021-01-01 19:00:52|
|3|	102	|1	| |	 	|2021-01-02 23:51:23|
|3	|102	|2	| 	|	|2021-01-02 23:51:23|
|4|	103|	1|	4	| 	|2021-01-04 13:23:46|
|4	|103|	1|	4	| |	2021-01-04 13:23:46|
|4|	103|	2	|4|	 |	2021-01-04 13:23:46|
|5|	104|	1|	|1	|2021-01-08 21:00:29|
|6|	101|	2|	|	|2021-01-08 21:03:13|
|7	|105|	2	|	|1	|2021-01-08 21:20:29|
|8|	102|	1|	|	|	2021-01-09 23:54:33|
|9|	103|	1	|4|	1, 5|	2021-01-10 11:22:59|
|10	|104|	1|		|	|2021-01-11 18:34:49|
|10|	104|	1	|2, 6|	1, 4	|2021-01-11 18:34:49|

**Table 3A - runner_orders_temp**
```sql

CREATE TEMP TABLE runner_orders_temp AS
SELECT order_id, runner_id, 
	CASE 
    	WHEN pickup_time LIKE 'null' THEN '' 
   	ELSE pickup_time END AS pickup_time,
    CASE 
    	WHEN distance LIKE 'null' THEN ''
        WHEN distance LIKE '%km' THEN TRIM('km' from distance)
        ELSE distance END AS distance,
    CASE 
    	WHEN duration LIKE 'null' THEN ''
        WHEN duration LIKE '%mins' THEN TRIM('mins' from duration)
        WHEN duration LIKE '%minute' THEN TRIM ('minute' from duration)
        WHEN duration LIKE '%minutes' THEN TRIM ('minutes' from duration) ELSE duration END AS duration,
    CASE WHEN cancellation IS null OR cancellation LIKE 'null' THEN '' ELSE cancellation END AS cancellation
FROM runner_orders;

```
|order_id|	runner_id	|pickup_time|	distance	|duration|	cancellation|
|---|---|---|---|---|---|
|1|	1	|2021-01-01 18:15:34|	20|	32	 | |
|2|	1	|2021-01-01 19:10:54|	20	|27|	 |
|3|	1	|2021-01-03 00:12:37|	13.4|20||
|4|	2	|2021-01-04 13:53:03	|23.4|	40	||
|5|	3	|2021-01-08 21:10:57	|10|	15|	|
|6|	3	|	||	|	Restaurant Cancellation|
|7|	2	|2020-01-08 21:30:45	|25	|25	||
|8	|2	|2020-01-10 00:15:02|	23.4 	|15 |	|
|9|	2	||		||	Customer Cancellation|
|10|	1|	2020-01-11 18:50:20	|10	|10	||


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

9.What was the total volume of pizzas ordered for each hour of the day?



What was the volume of orders for each day of the week?

B. Runner and Customer Experience
How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)
What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?
Is there any relationship between the number of pizzas and how long the order takes to prepare?
What was the average distance travelled for each customer?
What was the difference between the longest and shortest delivery times for all orders?
What was the average speed for each runner for each delivery and do you notice any trend for these values?
What is the successful delivery percentage for each runner?

C. Ingredient Optimisation
What are the standard ingredients for each pizza?
What was the most commonly added extra?
What was the most common exclusion?
Generate an order item for each record in the customers_orders table in the format of one of the following:
Meat Lovers
Meat Lovers - Exclude Beef
Meat Lovers - Extra Bacon
Meat Lovers - Exclude Cheese, Bacon - Extra Mushroom, Peppers
Generate an alphabetically ordered comma separated ingredient list for each pizza order from the customer_orders table and add a 2x in front of any relevant ingredients
For example: "Meat Lovers: 2xBacon, Beef, ... , Salami"
What is the total quantity of each ingredient used in all delivered pizzas sorted by most frequent first?

D. Pricing and Ratings
If a Meat Lovers pizza costs $12 and Vegetarian costs $10 and there were no charges for changes - how much money has Pizza Runner made so far if there are no delivery fees?
What if there was an additional $1 charge for any pizza extras?
Add cheese is $1 extra
The Pizza Runner team now wants to add an additional ratings system that allows customers to rate their runner, how would you design an additional table for this new dataset - generate a schema for this new table and insert your own data for ratings for each successful customer order between 1 to 5.
Using your newly generated table - can you join all of the information together to form a table which has the following information for successful deliveries?
customer_id
order_id
runner_id
rating
order_time
pickup_time
Time between order and pickup
Delivery duration
Average speed
Total number of pizzas
If a Meat Lovers pizza was $12 and Vegetarian $10 fixed prices with no cost for extras and each runner is paid $0.30 per kilometre traveled - how much money does Pizza Runner have left over after these deliveries?
