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


Back to [Main Menu](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%232:%20Pizza%20Runner.md)
