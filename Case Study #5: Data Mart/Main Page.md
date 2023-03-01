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

1. [Data Cleansing Steps](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%235:%20Data%20Mart/1.%20Data%20Cleansing%20Steps.md)
2. [Data Exploration](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%235:%20Data%20Mart/2.%20Data%20Exploration.md)
3. [Before & After Analysis](https://github.com/eunikehp/SQL-Case-Studies/blob/main/Case%20Study%20%235:%20Data%20Mart/3.%20Before%20%26%20After%20Analysis.md)

