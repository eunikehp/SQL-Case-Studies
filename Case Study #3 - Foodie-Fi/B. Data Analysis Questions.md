# Case Study #3 - Foodie-Fi

## B. Data Analysis Questions

1. How many customers has Foodie-Fi ever had?
2. What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value
3. What plan ```start_date``` values occur after the year 2020 for our dataset? Show the breakdown by count of events for each ```plan_name```
4. What is the customer count and percentage of customers who have churned rounded to 1 decimal place?
5. How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?
6. What is the number and percentage of customer plans after their initial free trial?
7. What is the customer count and percentage breakdown of all 5 ```plan_name``` values at 2020-12-31?
8. How many customers have upgraded to an annual plan in 2020?
9. How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?
10. Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)
11. How many customers downgraded from a pro monthly to a basic monthly plan in 2020?

## Answers:

**1. How many customers has Foodie-Fi ever had?**

To find the number of customers that Foodie-Fi had, I put ```COUNT``` and followed by ```DISTINCT``` .
```sql
SELECT COUNT(DISTINCT customer_id) AS total_customer
FROM subscriptions;
```
|total_customer|
|---|
|1000|

- Foodie-Fi has 1000 customers with unique ID.

**2. What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value?**

- First, find the detail of ```month``` from ```start_date```
- To be clear, I use ```TO_CHAR``` to show the month name.
- Filter only for that have plan_id = 0 to show only the trial.

```sql
SELECT DATE_PART('month', start_date) AS month_trial, TO_CHAR(start_date, 'Month') AS month_name,
COUNT(customer_id) AS total_customer
FROM subscriptions 
WHERE plan_id = 0
GROUP BY month_trial,month_name
ORDER BY month_trial;
```
| month_trial | month_name | total_customer |
| ----------- | ---------- | -------------- |
| 1           | January    | 88             |
| 2           | February   | 68             |
| 3           | March      | 94             |
| 4           | April      | 81             |
| 5           | May        | 88             |
| 6           | June       | 79             |
| 7           | July       | 89             |
| 8           | August     | 88             |
| 9           | September  | 87             |
| 10          | October    | 79             |
| 11          | November   | 75             |
| 12          | December   | 84             |


**3. What plan ```start_date``` values occur after the year 2020 for our dataset? Show the breakdown by count of events for each ```plan_name```**

- Find the years of each start_date, and count it
- Filter only that have start date after Dec 31 2020
 
```sql
SELECT p.plan_id, p.plan_name, 
COUNT( DATE_PART('year',s.start_date)) AS event_2021
FROM plans AS p
JOIN subscriptions AS s
ON p.plan_id = s.plan_id
WHERE s.start_date > '2020-12-31'
GROUP BY p.plan_id, p.plan_name
ORDER BY p.plan_id;
```
|plan_id	|plan_name	|event_2021|
|---|---|---|
|1|	basic monthly|	8|
|2|	pro monthly|	60|
|3|	pro annual|	63|
|4|	churn	|71|

**4. What is the customer count and percentage of customers who have churned rounded to 1 decimal place?**

- First, I used ```JOIN``` to find the number of customers that have churned the account by using conditional ```WHERE``` , 
- then calculated the total percentage of churn ,also rounded to 1 decimal.

```sql
SELECT COUNT(*) AS churn_count, 
ROUND(COUNT(*)::NUMERIC/(SELECT COUNT(DISTINCT customer_id)FROM subscriptions)*100, 1) AS churn_percentage
FROM plans AS p
JOIN subscriptions AS s
ON p.plan_id = s.plan_id
WHERE p.plan_id = 4;
```
|churn_count	|churn_percentage|
|---|---|
|307	|30.7|

**5. How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?**

```sql
WITH ranking AS(
SELECT s.customer_id, s.plan_id, p.plan_name, 
RANK () over (PARTITION BY s.customer_id ORDER BY p.plan_id) AS rank_plan -- rank the plan_id in order 
FROM plans AS p
JOIN subscriptions AS s
ON p.plan_id = s.plan_id
ORDER BY s.customer_id, s.plan_id)

SELECT COUNT(*), 
ROUND(COUNT(*)::NUMERIC/ (SELECT COUNT(DISTINCT customer_id) FROM subscriptions)*100,0) AS percentage --0 decimal
FROM ranking
WHERE plan_id = 4 AND rank_plan = 2;
```
|count|	percentage|
|---|---|
|92	|9|

**6. What is the number and percentage of customer plans after their initial free trial?**

Here are the steps how to find the total and percentage of customers who converted the plans after finishing the trial period:
- First, I find the plan ranking of each customer.
- Subsequently, calculate the number and percentage of customer with 1 decimal place
- Use the condition which only shows the plan ranking of 2 ,meaning the plan after customers take the initial free trial.

```sql
WITH ranking AS(
SELECT s.customer_id, s.plan_id, p.plan_name, 
RANK () over (PARTITION BY s.customer_id ORDER BY p.plan_id) AS rank_plan -- rank the plan_id in order 
FROM plans AS p
JOIN subscriptions AS s
ON p.plan_id = s.plan_id
ORDER BY s.customer_id, s.plan_id)

SELECT plan_id, COUNT(*), 
ROUND(COUNT(*)::NUMERIC/ (SELECT COUNT(DISTINCT customer_id) FROM subscriptions)*100,1) AS percentage --1 decimal
FROM ranking
WHERE rank_plan = 2
GROUP BY plan_id
ORDER BY plan_id;
```

|plan_id	|count|	percentage|
|---|---|---|
|1	|546|	54.6|
|2	|325	|32.5|
|3	|37	|3.7|
|4|	92	|9.2|


**7. What is the customer count and percentage breakdown of all 5 ```plan_name``` values at 2020-12-31?**

To find the status at 31 Dec 2020 in terms of the number of customers and the percentage, here are some steps:
1. Create rank based on the start date of each customer in descending order. Through this way,we know the latest status of what plans the customers have.
2. Filter to show only date before and at 31 Dec 2020
3. Count the number of customers and the percentage with condition above.
4. Filter where the status is 1,  which means showing the customer with the latest status.

```sql
WITH status AS(
SELECT s.customer_id, s.plan_id,p.plan_name, s.start_date, 
RANK () over (PARTITION BY s.customer_id ORDER BY s.start_date DESC)  AS current_status -- rank the start date in descending order
FROM plans AS p
JOIN subscriptions AS s
ON p.plan_id = s.plan_id
WHERE s.start_date <= '2020-12-31' 
ORDER BY s.customer_id, s.plan_id)

SELECT plan_id,plan_name, COUNT(*), 
ROUND(COUNT(*)::NUMERIC/ (SELECT COUNT(DISTINCT customer_id) FROM subscriptions)*100,1) AS percentage --1 decimal
FROM status
WHERE current_status = 1
GROUP BY plan_id,plan_name
ORDER BY plan_id;
 ```
|plan_id	|plan_name|	count|	percentage|
|---|---|---|---|
|0|	trial	|19|	1.9|
|1	|basic monthly|	224	|22.4|
|2|	pro monthly	|326	|32.6|
|3|	pro annual	|195	|19.5|
|4|	churn	|236|	23.6|

**8. How many customers have upgraded to an annual plan in 2020?**

```sql
WITH status AS(
SELECT s.customer_id, s.plan_id,p.plan_name, s.start_date, 
RANK () over (PARTITION BY s.customer_id ORDER BY s.start_date DESC)  AS current_status -- rank the start date in descending order
FROM plans AS p
JOIN subscriptions AS s
ON p.plan_id = s.plan_id
WHERE s.start_date <= '2020-12-31' AND s.plan_id = 3 
ORDER BY s.customer_id, s.plan_id)

SELECT COUNT(*) AS proannual_count, 
ROUND(COUNT(*)::NUMERIC/ (SELECT COUNT(DISTINCT customer_id) FROM subscriptions)*100,1) AS percentage --1 decimal
FROM status
WHERE current_status = 1
GROUP BY plan_id,plan_name
ORDER BY plan_id;
```
|proannual_count	|percentage|
|---|---|
|195	|19.5|

**9. How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?**
```sql
WITH free_trial AS(
SELECT s.customer_id, s.start_date AS trial_plan
FROM plans AS p
JOIN subscriptions AS s
ON p.plan_id = s.plan_id
WHERE p.plan_id = 0 ),

annual_subs AS(
SELECT s.customer_id, s.start_date AS annual_plan
FROM plans AS p
JOIN subscriptions AS s
ON p.plan_id = s.plan_id
WHERE p.plan_id = 3 )

SELECT ROUND (AVG(annual_plan - trial_plan),0) AS avg_days_upgrade_to_annual
FROM annual_subs AS ann
JOIN free_trial AS tri
ON ann.customer_id = tri.customer_id;
```
|avg_days_upgrade_to_annual|
|---|
|105|

**10. Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)**

**11. How many customers downgraded from a pro monthly to a basic monthly plan in 2020?**
```sql 
WITH status AS(
SELECT customer_id, plan_id, start_date, 
LEAD (plan_id, 1) over (PARTITION BY customer_id ORDER BY start_date )  AS next_plan
FROM subscriptions 
WHERE start_date <= '2020-12-31' 
ORDER BY customer_id, start_date)

SELECT COUNT (DISTINCT customer_id) AS downgraded
FROM status
WHERE next_plan = 1 AND plan_id = 2
```

|downgraded|
|---|
|0|

Back to [Main Page](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%233%20-%20Foodie-Fi/Main%20Page.md)
