# Case Study #2: Pizza Runner

## Problem Statement

Because Danny had a few years of experience as a data scientist - he was very aware that data collection was going to be critical for his business’ growth.

He has prepared for us an entity relationship diagram of his database design but requires further assistance to clean his data and apply some basic calculations so he can better direct his runners and optimise Pizza Runner’s operations.

All datasets exist within the pizza_runner database schema - be sure to include this reference within your SQL scripts as you start exploring the data and answering the case study questions.

## Entity Relationship Diagram

<img width="650" alt="image" src="https://user-images.githubusercontent.com/104567399/220662950-bf47793c-e3e5-43ee-bccc-051c2a12da7a.png">

**Table 1 :** ```runners```

|runner_id	|registration_date|
|---|---|
|1|	2021-01-01|
|2	|2021-01-03|
|3	|2021-01-08|
|4	|2021-01-15|

**Table 2 :** ```customer_orders```

|order_id|	customer_id|	pizza_id	|exclusions	|extras|	order_time|
|----|---|----|---|---|---|
|1	|101|	1	 |	| |	2021-01-01 18:05:02|
|2	|101|	1	| |	 	|2021-01-01 19:00:52|
|3|	102	|1	| |	 	|2021-01-02 23:51:23|
|3	|102	|2	| 	|NaN	|2021-01-02 23:51:23|
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

**Table 2 :** ```runner_orders```
|order_id|	runner_id	|pickup_time|	distance	|duration|	cancellation|
|---|---|---|---|---|---|
|1|	1	|2021-01-01 18:15:34|	20km|	32 minutes	 | |
|2|	1	|2021-01-01 19:10:54|	20km	|27 minutes|	 |
|3|	1	|2021-01-03 00:12:37|	13.4km	20 mins|	NaN|
|4|	2	|2021-01-04 13:53:03	|23.4|	40	|NaN|
|5|	3	|2021-01-08 21:10:57	|10|	15|	NaN
|6|	3	|null	|null|	null|	Restaurant Cancellation|
|7|	2	|2020-01-08 21:30:45	|25km	|25mins	|null|
|8	|2	|2020-01-10 00:15:02|	23.4 km	|15 minute|	null|
|9|	2	|null|	null	|null|	Customer Cancellation|
10	1	2020-01-11 18:50:20	10km	10minutes	null
