---
layout: post
title: Calculating Customer Retention Rate In SQL
description: In this post, I'm going to explore using SQL to calculate customer retention rate from a grocery store transaction data
image: "/posts/grocery.jpeg"
tags: [SQL, Postgresql, Transaction Data Insights]
---

## introduction
In this post, I'm going to explore using SQL to calculate the customer retention rate using grocery transaction data.  The database I'm using is on Postgres so I'll be using PostgreSQL to analyse the data for this mini-project. 

Transaction data can give you interesting insights into your customers, and help identify the customers who are most valuable to your business.  

One of the metrics you can determine is your customer retention rate.   This measures the percentage of customers that continue to make repeat purchases. A higher retention rate signals greater customer loyalty and value. 

These are the steps we're going to take to identify the customer retention rate from the transaction data we have.

* Identify the period you want to analyze for retention.  In this case, it is the 6 months from April 2020 to September 2020.
* Find the unique customers who made a purchase in the first month of the period. (First_Month_Customers)
* Of these, find the unique customers who also made a purchase in the last month of the period. (Returning_Customers)

> The retention rate for the period is calculated as (Returning_Customers) / (First_Month_Customers)

The retention rate metric lets you analyze customer loyalty over time. It's often calculated monthly to see trends. A higher rate indicates your store is better at getting return business.

Analysing retention by customer segments (demographics, purchase history, etc.) can provide more insights into which customer types are retained best. This helps guide marketing efforts.

---

## inspecting the data

First, we need to take a look at our dataset in SQL to see what data we have.  

```ruby
select *
from grocery_db.transactions
limit 5
;

```
So, we can see the first few columns of data we have.  This gives us the basic data we need to get an idea of our customer retention.

| customer_id | transaction_date | transaction_id | product_area_id | num_items | sales_cost | 
|:---|:---|---:|---:|---:|---:|
|1|2020-04-10|435657533999|3|7|19.16| 
|1|2020-04-10|435657533999|2|5|7.71| 
|1|2020-06-02|436189770685|4|4|26.97| 
|1|2020-06-02|436189770685|1|2|38.52| 
|1|2020-06-10|436265380298|4|4|22.13| 

We want to check for missing data first.

```ruby
select *
from grocery_db.transactions
where transaction_date is null
or num_items is null
or sales_cost is null
or customer_id is null
limit 6
;

```
OK, we didn't get any results from that, so the transaction dataset looks complete with no nulls in the data we are interested in.  Next, we'll take a look at the date range we have transactions for.   Let's find our smallest and largest transaction date.
we'll use the SQL min and max functions to do this.

```ruby
select max(transaction_date) as max_date
, min(transaction_date) as min_date
from grocery_db.transactions
;

```

| max_date | min_date |  
|:---|---:|
|2020-09-30|2020-04-01| 

So, we have 6 months' worth of transaction data to work with, let's see what we can find out about customer retention.

---

## unique customers who made purchases during the first month of the period

First, we'll count the distinct customers who made a purchase during April 2020.  We'll use the SQL count and distinct keywords to do this.  By using count and distinct together like this, SQL will only count each customer once, regardless of how many purchases they may have made in the month.
I could have used the 'between' keyword for the date condition, but I prefer to be explicit about which dates are included in the range, and also, on some databases, the between keyword is less efficient.  Not sure it matters on Postgres, but it's a preference.

```ruby
select count(distinct customer_id)
from grocery_db.transactions
where transaction_date >= '2020-04-01'
and transaction_date <= '2020-04-30'
;

-- we get a result of 
>>> count
>>> 804

```

So, we had 804 distinct customers in April.  Let's see how many of those were still shopping in September.

---
## of those, how many customers also made purchases in the last month of the period?
There are a number of ways you could write this query in SQL, but I'll go with this one.  

We'll start with a query that selects the distinct customer IDs who made purchases in the first month.  Then we'll do an inner join on this set against a query that selects and counts the distinct customer IDs who made purchases in the final month.

The inner join on the customer ID means we'll only have the customer IDs that appear in both sets.

```ruby
with ORIGINAL_MONTH AS
(
select distinct customer_id
from grocery_db.transactions
where transaction_date >= '2020-04-01'
and transaction_date <= '2020-04-30'
)
select count(distinct LATEST_MONTH.customer_id) 
from grocery_db.transactions LATEST_MONTH
inner join ORIGINAL_MONTH on ORIGINAL_MONTH.customer_id = LATEST_MONTH.customer_id
where LATEST_MONTH.transaction_date >= '2020-09-01'
and LATEST_MONTH.transaction_date <= '2020-09-30'
;

-- we get a result of 
>>> count
>>> 740

```

So our customer retention rate = 740 / 804 over the 6-month period from April to September 2020.

That's a retention rate of *92%* which is pretty excellent!

But what if I wanted my SQL to do all that calculation for me in one go, as part of a rolling automated report for example?

Let's use a cross-join with the original month's customer count, so we have the first month's Total Customers available in each row of retained customers, so we can use it for the calculation.  

A cross-join between 2 sets, creates a set with every item of the first set multiplied by every item of the second set.

So if SET 1 contained 2 rows: A, B
and SET 2 contained 2 rows:  1, 2

Then SET 1 cross join SET 2 would result in 4 rows:
* A 1
* B 1
* A 2
* B 2 

Our ORIGINAL_COUNT set will only have one row in it, so it won't change the total number of rows from our retained customer set, but it will give us access to the total customer count for the first month, that we need in our later calculation.  

We have to use an aggregate function on it, but it will be the same value in every row, so I'll just use MAX.

```ruby
with
ORIGINAL_COUNT AS
(
select count(distinct customer_id) As TotalFirstMonth
from grocery_db.transactions
where transaction_date >= '2020-04-01'
and transaction_date <= '2020-04-30'
) 
, ORIGINAL_MONTH AS
(
select distinct customer_id
from grocery_db.transactions
where transaction_date >= '2020-04-01'
and transaction_date <= '2020-04-30'
)
, LATEST_MONTH_RETAINED AS 
(
select distinct LATEST_MONTH_RETAINED.customer_id
from grocery_db.transactions LATEST_MONTH_RETAINED
inner join ORIGINAL_MONTH on ORIGINAL_MONTH.customer_id = LATEST_MONTH_RETAINED.customer_id
where LATEST_MONTH_RETAINED.transaction_date >= '2020-09-01'
and LATEST_MONTH_RETAINED.transaction_date <= '2020-09-30'
)
select count(distinct LATEST_MONTH_RETAINED.customer_id) As Retained_Customers , Max(ORIGINAL_COUNT.TotalFirstMonth) As First_Month_Customers, (count(distinct LATEST_MONTH_RETAINED.customer_id) * 100 / Max(ORIGINAL_COUNT.TotalFirstMonth)) As Percent_Retention_Rate
from LATEST_MONTH 
CROSS JOIN ORIGINAL_COUNT
;

```

Here is our final customer retention summary report:

| retained_customers | first_month_customers | percent_retention_rate| 
|---:|---:|---:|
|740|804|92| 
