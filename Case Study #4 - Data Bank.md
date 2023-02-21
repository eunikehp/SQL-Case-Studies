# Case Study #4 - Data Bank

The Data Bank team have prepared a data model for this case study as well as a few example rows 
from the complete dataset below to get you familiar with their tables.

**Entity Relationship Diagram**

<img width="650" alt="image" src="https://user-images.githubusercontent.com/104567399/189273563-9ce25943-14c7-4db0-a53b-06529a724549.png">


## Case Study Questions

The following case study questions include some general data exploration analysis for the nodes and transactions before diving right into the core business questions and finishes with a challenging final request!

**A. Customer Nodes Exploration**

1. How many unique nodes are there on the Data Bank system?
2. What is the number of nodes per region?
3. How many customers are allocated to each region?
4. How many days on average are customers reallocated to a different node?
5. What is the median, 80th and 95th percentile for this same reallocation days metric for each region?

**B. Customer Transactions**

1. What is the unique count and total amount for each transaction type?
2. What is the average total historical deposit counts and amounts for all customers?
3. For each month - how many Data Bank customers make more than 1 deposit and either 1 purchase or 1 withdrawal in a single month?
4. What is the closing balance for each customer at the end of the month?
5. What is the percentage of customers who increase their closing balance by more than 5%?

**C. Data Allocation Challenge**

To test out a few different hypotheses - the Data Bank team wants to run an experiment where different 
groups of customers would be allocated data using 3 different options:

- Option 1: data is allocated based off the amount of money at the end of the previous month
- Option 2: data is allocated on the average amount of money kept in the account in the previous 30 days
- Option 3: data is updated real-time

For this multi-part challenge question - you have been requested to generate the following data elements 
to help the Data Bank team estimate how much data will need to be provisioned for each option:

- running customer balance column that includes the impact each transaction
- customer balance at the end of each month
- minimum, average and maximum values of the running balance for each customer

Using all of the data available - how much data would have been required for each option on a monthly basis?

## Solution

1. How many unique nodes are there on the Data Bank system?

```sql
SELECT COUNT(DISTINCT node_id) FROM customer_nodes;
```
| count | 
| ----------- | 
| 5           | 

2. What is the number of nodes per region?
```sql
SELECT c.region_id, r.region_name, COUNT (node_id) AS node_count
FROM customer_nodes AS c
JOIN regions AS r
ON c.region_id = r.region_id
GROUP BY c.region_id, r.region_name
ORDER BY c.region_id;
```
|region_id	|region_name	|node_count|
|--|--|--|
|1	|Australia	|770|
|2	|America	|735|
|3	|Africa	|714|
|4	|Asia	|665|
|5|	Europe	|616|

3. How many customers are allocated to each region?
```sql
SELECT c.region_id, r.region_name, COUNT (customer_id) AS customer_count
FROM customer_nodes AS c
JOIN regions AS r
ON c.region_id = r.region_id
GROUP BY c.region_id, r.region_name
ORDER BY c.region_id;
```
|region_id|	region_name	|customer_count|
|-|-|-|
|1	|Australia|	770|
|2	|America|	735|
|3	|Africa|	714|
|4	|Asia	|665|
|5|	Europe	|616|

4. How many days on average are customers reallocated to a different node?
5. What is the median, 80th and 95th percentile for this same reallocation days metric for each region?

**B. Customer Transactions**

1. What is the unique count and total amount for each transaction type?
```sql
SELECT txn_type, COUNT(customer_id), SUM(txn_amount) AS total_amount
FROM customer_transactions
GROUP BY txn_type;
```
|txn_type	|count	|total_amount|
|---|---|---|
|purchase	|1617	|806537|
|deposit	|2671	|1359168|
|withdrawal	|1580|	793003|

2. What is the average total historical deposit counts and amounts for all customers?
```sql
WITH historical_deposit AS
(SELECT customer_id, txn_type, COUNT(*) AS txn_count , AVG (txn_amount) AS avg_amount
FROM customer_transactions
GROUP BY customer_id, txn_type)

  SELECT ROUND(AVG(txn_count),0) AS avg_count,
  ROUND (AVG(avg_amount),2) AS avg_amount
  FROM historical_deposit
  WHERE txn_type = 'deposit';
```
|avg_count	|avg_amount|
|---|---|
|5	|508.61 |

3. For each month - how many Data Bank customers make more than 1 deposit and either 1 purchase or 1 withdrawal in a single month?


5. What is the closing balance for each customer at the end of the month?
6. What is the percentage of customers who increase their closing balance by more than 5%?
