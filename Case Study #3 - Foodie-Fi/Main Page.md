# Case Study #3 - Foodie-Fi

## Problem Statement
Danni wanted to create a new streaming service that only had food related content - something like Netflix but with only cooking shows!
He finds a few smart friends to launch his new startup Foodie-Fi in 2020 and started selling monthly and annual subscriptions, 
giving their customers unlimited on-demand access to exclusive food videos from around the world!

Danny created Foodie-Fi with a data driven mindset and wanted to ensure all future investment decisions and new features were decided using data. 
This case study focuses on using subscription style digital data to answer important business questions.

## Entity Relationship Diagram

<img width="550" alt="image" src="https://user-images.githubusercontent.com/104567399/221893318-5988e3ba-f2f5-4026-9443-54804d15f182.png">

**Table 1:** ```plans```

|plan_id|	plan_name|	price|
|---|---|---|
|0|	trial|	0|
|1	|basic monthly|	9.90|
|2	|pro monthly	|19.90|
|3|	pro annual|	199|
|4|	churn	|null|

Customers can choose which plans to join Foodie-Fi when they first sign up. 

- **Trial:** Customers can sign up to an initial 7 day free trial will automatically continue with the pro monthly subscription plan 
unless they cancel, downgrade to basic or upgrade to an annual pro plan at any point during the trial.
- **Basic plan:** customers have limited access and can only stream their videos and is only available monthly at $9.90.
- **Pro plan:** customers have no watch time limits and are able to download videos for offline viewing. 
They start at $19.90 a **month** or $199 for an **annual subscription**.
- **Churn plan:** When customers cancel their Foodie-Fi service - they will have a record with a null price 
but their plan will continue until the end of the billing period.

**Table 2:** ```subscriptions```

|customer_id	|plan_id	|start_date|
|---|---|---|
|1|	0|	2020-08-01|
|1	|1	|2020-08-08|
|2	|0	|2020-09-20|
|2	|3|	2020-09-27|
|11|	0	|2020-11-19|
|11	|4	|2020-11-26|
|13	|0	|2020-12-15|
|13	|1	|2020-12-22|
|13	|2	|2021-03-29|
|15	|0	|2020-03-17|
|15	|2	|2020-03-24|
|15	|4	|2020-04-29|
|16	|0	|2020-05-31|
|16	|1	|2020-06-07|
|16	|3	|2020-10-21|
|18	|0	|2020-07-06|
|18	|2	|2020-07-13|
|19	|0	|2020-06-22|
|19	|2	|2020-06-29|
|19	|3|	2020-08-29|

Customer subscriptions show the exact date where their specific plan_id starts. 
If customers downgrade from a pro plan or cancel their subscription - the higher plan will remain in place until the period is over - 
the start_date in the subscriptions table will reflect the date that the actual plan changes.

When customers upgrade their account from a basic plan to a pro or annual pro plan - the higher plan will take effect straightaway.
When customers churn - they will keep their access until the end of their current billing period but the start_date will be technically the day they decided to cancel their service.


## Case Study Questions

**A. Customer Journey**

Based off the 8 sample customers provided in the sample from the subscriptions table, 
write a brief description about each customer’s onboarding journey.

- First, I combine those 2 provided tables for getting a clear idea to create customer's onboarding journey. 

```sql
SELECT s.customer_id, s.plan_id, p.plan_name, s.start_date
FROM plans AS p
JOIN subscriptions AS s
ON p.plan_id = s.plan_id
WHERE s.customer_id IN (1,2,11,13,15,16,18,19)
ORDER BY s.customer_id, s.plan_id;
```

|customer_id	|plan_id|	plan_name|	start_date|
|---|---|---|---|
|1	|0	|trial	|2020-08-01|
|1|	1	|basic monthly	|2020-08-08|
|2	|0	|trial	|2020-09-20|
|2|	3	|pro annual	|2020-09-27|
|11	|0|	trial	|2020-11-19|
|11	|4	|churn	|2020-11-26|
|13	|0	|trial	|2020-12-15|
|13	|1	|basic monthly	|2020-12-22|
|13	|2	|pro monthly	|2021-03-29|
|15	|0	|trial	|2020-03-17|
|15	|2	|pro monthly	|2020-03-24|
|15	|4	|churn|	2020-04-29|
|16	|0	|trial	|2020-05-31|
|16	|1	|basic monthly	|2020-06-07|
|16	|3	|pro annual	|2020-10-21|
|18	|0	|trial|	2020-07-06|
|18	|2	|pro monthly|	2020-07-13|
|19	|0	|trial	|2020-06-22|
|19	|2|	pro monthly|	2020-06-29|
|19	|3|	pro annual	|2020-08-29|

From the table above, I can conclude the onboarding journey of each customer: 
- Customer 1 signed up and followed free trial on 1 August 2020 and then the next 7 days, subscribed the monthly basic plan.
- Customer 2 started the free trial on 20 Sept 2020 and subsequently started annual subscription after the trial period has ended.
- Customer 11 followed trial plan on 19 Nov 2020, however did not upgrade the plan afterwards & chose churn plan.
- Customer 13 chose free trial on 15 Dec 2020. The next 7 days, subscribed monthly basic plan. About 3 months, upgraded the plan into pro plan with monthly payment.
- Customer 15 started the trial plan on 17 Mar 2020, then subscribed monthly pro plan after trial period ends. About a month, terminated the plan. 
- Customer 16 signed up and chose trial plan on 31 Mei 2020. After trial period ends, followed the basic plan with monthly payment and upgraded the plan to pro with annual subscription in the next 4 months.
- Customer 18 followed trial plan on 6 Jul 2020 and afterwards subscribed pro plan with monthly subscription. 
- Customer 19 started the free trial on 22 Jun 2020, then subscribed pro plan with monthly payment. About 2 months, changed the payment system from monthly to annual.

Next, there are some questions related the case & solutions that I made:

[**B. Data Analysis Questions**](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%233%20-%20Foodie-Fi/B.%20Data%20Analysis%20Questions.md)

[**C. Challenge Payment Questions**](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%233%20-%20Foodie-Fi/C.%20Challenge%20Payment%20Question.md)


