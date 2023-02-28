# B. Data Analysis Questions

1. How many customers has Foodie-Fi ever had?
2. What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value
3. What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each ```plan_name```
4. What is the customer count and percentage of customers who have churned rounded to 1 decimal place?
5. How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?
6. What is the number and percentage of customer plans after their initial free trial?
7. What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?
8. How many customers have upgraded to an annual plan in 2020?
9. How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?
10. Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)
11. How many customers downgraded from a pro monthly to a basic monthly plan in 2020?

**Answers:**

1. How many customers has Foodie-Fi ever had?
```sql
SELECT COUNT(DISTINCT customer_id) AS total_customer
FROM subscriptions;
```
|total_customer|
|---|
|1000|

2. What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value?

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


3. What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each ```plan_name```

4. What is the customer count and percentage of customers who have churned rounded to 1 decimal place?
5. How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?
6. What is the number and percentage of customer plans after their initial free trial?
7. What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?
8. How many customers have upgraded to an annual plan in 2020?
9. How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?
10. Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)
11. How many customers downgraded from a pro monthly to a basic monthly plan in 2020?

Back to [Main Page](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%233%20-%20Foodie-Fi/Main%20Page.md)
