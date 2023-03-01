# Case Study #5 - Data Mart

## Problem Statement

Data Mart is Danny’s latest venture and after running international operations for his online supermarket that specialises in fresh produce.
Danny is asking for your support to analyse his sales performance.

In June 2020,large scale supply changes were made at Data Mart. 
All Data Mart products now use sustainable packaging methods in every single step from the farm all the way to the customer.

Danny needs your help to quantify the impact of this change on the sales performance for Data Mart and it’s separate business areas.
The key business question he wants you to help him answer are the following:
- What was the quantifiable impact of the changes introduced in June 2020?
- Which platform, region, segment and customer types were the most impacted by this change?
- What can we do about future introduction of similar sustainability updates to the business to minimise impact on sales?

## Entity Relationship Diagram

<img width="250" alt="image" src="https://user-images.githubusercontent.com/104567399/222139020-a1f334ce-a160-4c1e-849a-6fb4f332a198.png">

For this case study there is only a single **table:** ```data_mart.weekly_sales```.

10 random rows are shown in the table output below from ```data_mart.weekly_sales```:

|week_date	|region|	platform|	segment	|customer_type|	transactions|	sales|
|---|---|---|---|---|---|---|
|9/9/20|	OCEANIA	|Shopify	|C3	|    New	     | 610	|110033.89|
|29/7/20|	AFRICA	|Retail|	C1	  |  New	    |  110692	|3053771.19|
|22/7/20	|EUROPE	|Shopify|	C4	  |  Existing|	24|	8101.54|
|13/5/20	|AFRICA	|Shopify	|null	|  Guest	   | 5287|	1003301.37|
|24/7/19	|ASIA	  |Retail|	C1	|    New	    |  127342|	3151780.41|
|10/7/19	|CANADA	|Shopify|	F3	|    New	    |  51	|8844.93|
|26/6/19	|OCEANIA|	Retail|	C3	|    New	    |  152921	|5551385.36|
|29/5/19	|SOUTH   AMERICA	|Shopify|	null	  |  New|	53	|10056.2|
|22/8/18	|AFRICA	|Retail|	null|	  Existing	|31721	|1718863.58|
|25/7/18	|SOUTH  AMERICA|	Retail	|null	  |  New	|2136	|81757.91|

## Case Study Questions

The following case study questions require some data cleaning steps before we start to unpack Danny’s key business questions in more depth.


2. Data Exploration
What day of the week is used for each week_date value?
What range of week numbers are missing from the dataset?
How many total transactions were there for each year in the dataset?
What is the total sales for each region for each month?
What is the total count of transactions for each platform
What is the percentage of sales for Retail vs Shopify for each month?
What is the percentage of sales by demographic for each year in the dataset?
Which age_band and demographic values contribute the most to Retail sales?
Can we use the avg_transaction column to find the average transaction size for each year for Retail vs Shopify? If not - how would you calculate it instead?
3. Before & After Analysis
This technique is usually used when we inspect an important event and want to inspect the impact before and after a certain point in time.

Taking the week_date value of 2020-06-15 as the baseline week where the Data Mart sustainable packaging changes came into effect.

We would include all week_date values for 2020-06-15 as the start of the period after the change and the previous week_date values would be before

Using this analysis approach - answer the following questions:

What is the total sales for the 4 weeks before and after 2020-06-15? What is the growth or reduction rate in actual values and percentage of sales?
What about the entire 12 weeks before and after?
How do the sale metrics for these 2 periods before and after compare with the previous years in 2018 and 2019?
