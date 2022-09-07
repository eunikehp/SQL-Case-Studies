# Case Study #1: Danny's Dinner

## Problem Statement

Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, 
how much money they’ve spent and also which menu items are their favourite. 
Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - 
additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

Danny has provided you with a sample of his overall customer data due to privacy issues - 
but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

Danny has shared with you 3 key datasets for this case study:

- ```sales```
- ```menu```
- ```members```

<img width="650" alt="image" src="https://user-images.githubusercontent.com/104567399/188868508-dbcb086a-5d82-450d-ba18-8aaad24bb8dc.png">

## Case Study Questions

1. What is the total amount each customer spent at the restaurant?
2. How many days has each customer visited the restaurant?
3. What was the first item from the menu purchased by each customer?
4. What is the most purchased item on the menu and how many times was it purchased by all customers?
5. Which item was the most popular for each customer?
6. Which item was purchased first by the customer after they became a member?
7. Which item was purchased just before the customer became a member?
8. What is the total items and amount spent for each member before they became a member?
9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, 
not just sushi - how many points do customer A and B have at the end of January?

## Solution

**1. What is the total amount each customer spent at the restaurant?**

STEPS:
1. **JOIN** table of ```sales``` and ```menu``` based on ```product_id```.
2. **SUM** the ```price``` and **GROUP BY** ```customer_id``` to calculate ```total_spending```.

```sql
SELECT
  sales.customer_id , 
  SUM(menu.price) AS total_spending
FROM sales
JOIN menu
ON sales.product_id = menu.product_id
GROUP BY customer_id
ORDER BY customer_id;
```
| customer_id | total_spending |
| ----------- | -------------- |
| A           | 76             |
| B           | 74             |
| C           | 36             |


**2. How many days has each customer visited the restaurant?**

STEPS:
1. **COUNT** the ```order_date``` of each customer by using **DISTINCT** to keep two or more visits in one day counting as one.

```sql
SELECT 
customer_id,
COUNT(DISTINCT(order_date)) AS total_visit
FROM sales
GROUP BY customer_id
ORDER BY customer_id;
```
| customer_id | total_visit |
| ----------- | ----------- |
| A           | 4           |
| B           | 6           |
| C           | 2           |


**3. What was the first item from the menu purchased by each customer?**

STEPS:
1. Use **DENSE_RANK** to give rank to item that each customer ordered from the first visit to last visit based on the ```order_date```.
2. **JOIN** table of ```sales``` and ```menu``` to show the ```customer_id``` and ```product_name```.
3. Use CTE method to create temporary table.
4. To show only the first item that each customer ordered, put condition **WHERE** rank contains 1.
5. **DISTINCT** the product name, so for customer who ordered two ramens at first day, the result show only ramen once.

```sql
WITH order_rank AS(
SELECT 
customer_id, sales.order_date,
DENSE_RANK () over (ORDER BY sales.order_date) AS rank, menu.product_name
FROM sales
JOIN menu
ON sales.product_id = menu.product_id
)
SELECT DISTINCT customer_id, product_name AS first_order
FROM order_rank
WHERE rank = 1
ORDER BY customer_id;
```
| customer_id | first_order |
| ----------- | ----------- |
| A           | curry       |
| A           | sushi       |
| B           | curry       |
| C           | ramen       |

**4. What is the most purchased item on the menu and how many times was it purchased by all customers?**

STEPS:
1. **JOIN** the ```sales``` and ```menu``` table.
2. **COUNT** the total product purchased based on the ```product_name```.
3. **ORDER BY** the product in descending order to show the highest number on the first row.
4. **LIMIT** the row to show only row 1 as the most purchased item.

```sql
SELECT menu.product_name, COUNT(sales.product_id) AS most_purchased 
FROM sales 
JOIN menu
ON sales.product_id = menu.product_id
GROUP BY product_name
ORDER BY most_purchased DESC
LIMIT 1;
```

| product_name | most_purchased |
| ------------ | -------------- |
| ramen        | 8              |

**5. Which item was the most popular for each customer?**

```sql
WITH ranking AS(
  SELECT customer_id, product_id, COUNT(product_id) AS order_count,
  DENSE_RANK() over (PARTITION BY customer_id ORDER BY COUNT(product_id) DESC) AS rank 
  FROM sales
  GROUP BY customer_id, product_id
  ORDER BY customer_id)
 
SELECT ranking.customer_id, menu.product_name, ranking.order_count
FROM ranking
JOIN menu
ON ranking.product_id = menu.product_id
WHERE rank = 1
ORDER BY customer_id;
```

| customer_id | product_name | order_count |
| ----------- | ------------ | ----------- |
| A           | ramen        | 3           |
| B           | sushi        | 2           |
| B           | curry        | 2           |
| B           | ramen        | 2           |
| C           | ramen        | 3           |


7. Which item was purchased first by the customer after they became a member?
8. Which item was purchased just before the customer became a member?
9. What is the total items and amount spent for each member before they became a member?
10. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
11. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, 
not just sushi - how many points do customer A and B have at the end of January?
