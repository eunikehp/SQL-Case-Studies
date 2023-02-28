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

## Answers:
Before answering the questions, cleaning the table is the first thing to do as it has inconsistent values, such as null & missing value. 
1. [Cleaning the database](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%232:%20Pizza%20Runner/%20Clean%20the%20database.md)
2. [Case Study Questions & Solutions - A. Pizza Metrics](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%232:%20Pizza%20Runner/A.%20Pizza%20Metrics.md)
3. [Case Study Questions & Solutions - B. Runner and Customer Experience](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%232:%20Pizza%20Runner/B.%20Runner%20and%20Customer%20Experience.md)
4. [Case Study Questions & Solutions - C. Ingredient Optimisation](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%232:%20Pizza%20Runner/C.%20Ingredient%20Optimisation.md)
5. [Case Study Questions & Solutions - D. Pricing and Ratings](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%232:%20Pizza%20Runner/%20D.%20Pricing%20and%20Ratings.md)


