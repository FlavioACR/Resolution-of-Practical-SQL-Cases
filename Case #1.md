# Case #1 Danny’s Dineer

Created time: January 25, 2023 8:09 AM
Last edited time: January 25, 2023 9:13 AM
Tags: Data Study Cases

# **Introduction**

Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen. Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.

# **Problem Statement**

Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers. He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

Danny has shared with you 3 key datasets for this case study:

- `sales`
- `menu`
- `members`

You can inspect the entity relationship diagram and example data below.

# **Example Datasets**

All datasets exist within the `dannys_diner` database schema - be sure to include this reference within your SQL scripts as you start exploring the data and answering the case study questions.

### **Table 1: sales**

The `sales` table captures all `customer_id` level purchases with an corresponding `order_date` and `product_id` information for when and what menu items were ordered.

| customer_id | order_date | product_id |
| --- | --- | --- |
| A | 2021-01-01 | 1 |
| A | 2021-01-01 | 2 |
| A | 2021-01-07 | 2 |
| A | 2021-01-10 | 3 |
| A | 2021-01-11 | 3 |
| A | 2021-01-11 | 3 |
| B | 2021-01-01 | 2 |
| B | 2021-01-02 | 2 |
| B | 2021-01-04 | 1 |
| B | 2021-01-11 | 1 |
| B | 2021-01-16 | 3 |
| B | 2021-02-01 | 3 |
| C | 2021-01-01 | 3 |
| C | 2021-01-01 | 3 |
| C | 2021-01-07 | 3 |

### **Table 2: menu**

The `menu` table maps the `product_id` to the actual `product_name` and `price` of each menu item.

| product_id | product_name | price |
| --- | --- | --- |
| 1 | sushi | 10 |
| 2 | curry | 15 |
| 3 | ramen | 12 |

### **Table 3: members**

The final `members` table captures the `join_date` when a `customer_id` joined the beta version of the Danny’s Diner loyalty program.

| customer_id | join_date |
| --- | --- |
| A | 2021-01-07 |
| B | 2021-01-09 |

# Creating the schema and tables:

```sql
CREATE SCHEMA dannys_diner;
SET search_path = dannys_diner;

CREATE TABLE sales (
  "customer_id" VARCHAR(1),
  "order_date" DATE,
  "product_id" INTEGER
);

INSERT INTO sales
  ("customer_id", "order_date", "product_id")
VALUES
  ('A', '2021-01-01', '1'),
  ('A', '2021-01-01', '2'),
  ('A', '2021-01-07', '2'),
  ('A', '2021-01-10', '3'),
  ('A', '2021-01-11', '3'),
  ('A', '2021-01-11', '3'),
  ('B', '2021-01-01', '2'),
  ('B', '2021-01-02', '2'),
  ('B', '2021-01-04', '1'),
  ('B', '2021-01-11', '1'),
  ('B', '2021-01-16', '3'),
  ('B', '2021-02-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-07', '3');
 

CREATE TABLE menu (
  "product_id" INTEGER,
  "product_name" VARCHAR(5),
  "price" INTEGER
);

INSERT INTO menu
  ("product_id", "product_name", "price")
VALUES
  ('1', 'sushi', '10'),
  ('2', 'curry', '15'),
  ('3', 'ramen', '12');
  

CREATE TABLE members (
  "customer_id" VARCHAR(1),
  "join_date" DATE
);

INSERT INTO members
  ("customer_id", "join_date")
VALUES
  ('A', '2021-01-07'),
  ('B', '2021-01-09');
```

# **Case Study Questions & Solutions:**

Each of the following case study questions can be answered using a single SQL statement and each question includethe code query and the output of the query.

## What is the total amount each customer spent at the restaurant?

**Solution:**

```sql
SELECT
	customer_id,
  sum(dannys_diner.menu.price) AS "total_spent_amount"
FROM dannys_diner.sales
INNER JOIN dannys_diner.menu
	ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
GROUP BY customer_id
ORDER BY customer_id
;
```

**Output:**

| customer_id | total_spent_amount |
| --- | --- |
| A | 76 |
| B | 74 |
| C | 36 |

## How many days has each customer visited the restaurant?

**Solution:**

```sql
SELECT
	customer_id,
  count(customer_id) as "visited_days"
FROM dannys_diner.sales
GROUP BY customer_id
ORDER BY customer_id
;
```

**Output:**

| visited_days | visited_days |
| --- | --- |
| A | 6 |
| B | 6 |
| C | 3 |

## What was the first item from the menu purchased by each customer?

**Solutions:**

```sql
SELECT
	dannys_diner.sales.customer_id,
  dannys_diner.menu.product_name
FROM dannys_diner.sales
JOIN dannys_diner.menu
	ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
WHERE dannys_diner.sales.order_date = (SELECT MIN(order_date) FROM dannys_diner.sales)
GROUP BY dannys_diner.sales.customer_id, dannys_diner.menu.product_name
ORDER BY dannys_diner.sales.customer_id
;
```

**Output:**

| customer_id | product_name |
| --- | --- |
| A | curry |
| A | sushi |
| B | curry |
| C | ramen |

## What is the most purchased item on the menu and how many times was it purchased by all customers?

**Solutions:**

```sql
SELECT
    dannys_diner.menu.product_name as producto_mas_vendido,
    tbd.numero_de_ventas
FROM (SELECT DISTINCT
	product_id,
    count(product_id) as numero_de_ventas
FROM dannys_diner.sales
GROUP BY product_id
ORDER BY product_id) as tbd
JOIN dannys_diner.menu
	ON tbd.product_id = dannys_diner.menu.product_id
ORDER BY tbd.numero_de_ventas DESC
LIMIT 1
;
```

**Output:**

| producto_mas_vendido | numero_de_ventas |
| --- | --- |
| ramen | 8 |

## Which item was the most popular for each customer?

```sql
SELECT
*
FROM (SELECT
				customer_id,
        product_id,
        ventas,
      	product_name,
        RANK()
            OVER(PARTITION BY customer_id
                 ORDER BY ventas) AS "top_rank"
		  FROM (SELECT DISTINCT
							tmp.customer_id,
	            tmp.product_id,
	            tmp.ventas,
	            dannys_diner.menu.product_name
	          FROM	(SELECT DISTINCT
	                    customer_id,
	                    product_id,
	                    count(product_id) AS "sales"
	                 FROM dannys_diner.sales
	                 GROUP by customer_id, product_id
	                 ORDER BY ventas DESC) AS tmp
					         JOIN dannys_diner.menu
					           ON tmp.product_id = dannys_diner.menu.product_id
			            ORDER BY tmp.ventas DESC) AS tmp) AS tmp
WHERE tmp.toprank = 1
;
```

**Output:**

| customer_id | product_id | sales | product_name | top_rank |
| --- | --- | --- | --- | --- |
| A | 1 | 1 | sushi | 1 |
| B | 1 | 2 | sushi | 1 |
| B | 2 | 2 | curry | 1 |
| B | 3 | 2 | ramen | 1 |
| C | 3 | 3 | ramen | 1 |

## Which item was purchased first by the customer after they became a member?

**Solutions:**

```sql
SELECT
* 
FROM (SELECT
        tmp.customer_id,
        tmp.order_date,
        tmp.join_date,
        tmp.product_id,
        tmp.product_name, 	
        RANK()
            OVER(PARTITION BY customer_id
								 ORDER BY tmp.order_date) AS "buy_after_be_member"
      FROM (SELECT
							dannys_diner.sales.customer_id,
	            dannys_diner.sales.order_date,
	            dannys_diner.members.join_date,
	            dannys_diner.sales.product_id,
	            dannys_diner.menu.product_name
		        FROM dannys_diner.sales
		        JOIN dannys_diner.members
	            ON dannys_diner.sales.customer_id = dannys_diner.members.customer_id
		        JOIN dannys_diner.menu
			        ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
		        WHERE dannys_diner.sales.order_date > dannys_diner.members.join_date
		        ORDER BY 	dannys_diner.sales.customer_id) as tmp) as tmp
WHERE buy_after_be_member = 1
;
```

**Output:**

| stomer_id | order_date | join_date | product_id | product_name | buy_after_be_member |
| --- | --- | --- | --- | --- | --- |
| A | 2021-01-10T00:00:00.000Z | 2021-01-07T00:00:00.000Z | 3 | ramen | 1 |
| B | 2021-01-11T00:00:00.000Z | 2021-01-09T00:00:00.000Z | 1 | sushi | 1 |

## Which item was purchased just before the customer became a member?

**Solutions:**

```sql
SELECT
* 
FROM (SELECT
        tmp.customer_id,
        tmp.order_date,
        tmp.join_date,
        tmp.product_id,
        tmp.product_name, 	
        RANK()
            OVER(PARTITION BY customer_id ORDER BY tmp.order_date) AS "buy_after_be_member"
    FROM (SELECT
            dannys_diner.sales.customer_id,
            dannys_diner.sales.order_date,
            dannys_diner.members.join_date,
            dannys_diner.sales.product_id,
            dannys_diner.menu.product_name
        FROM dannys_diner.sales
        JOIN dannys_diner.members
            ON dannys_diner.sales.customer_id = dannys_diner.members.customer_id
        JOIN dannys_diner.menu
            ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
        WHERE dannys_diner.sales.order_date < dannys_diner.members.join_date
        ORDER BY 	dannys_diner.sales.customer_id) as tmp) as tmp
WHERE buy_after_be_member = 1
;
```

**Output:**

| customer_id | order_date | join_date | product_id | product_name | buy_after_be_member |
| --- | --- | --- | --- | --- | --- |
| A | 2021-01-01T00:00:00.000Z | 2021-01-07T00:00:00.000Z | 1 | sushi | 1 |
| A | 2021-01-01T00:00:00.000Z | 2021-01-07T00:00:00.000Z | 2 | curry | 1 |
| B | 2021-01-01T00:00:00.000Z | 2021-01-09T00:00:00.000Z | 2 | curry | 1 |

## What is the total items and amount spent for each member before they became a member?

**Solutions:**

```sql
SELECT 
	customer_id,
    sum(buys) as total_items, 
    sum(amount) as total_amount
FROM (SELECT 
        tbd.customer_id,
        tbd.buys,
        (tbd.buys * dannys_diner.menu.price) AS amount
	    FROM (
				SELECT
			    customer_id,
	        product_id,
					count(product_id) as buys
		    FROM (
					SELECT
				    dannys_diner.sales.customer_id,
			      dannys_diner.sales.product_id
			    FROM dannys_diner.sales
			    JOIN dannys_diner.members
				    ON dannys_diner.sales.customer_id = dannys_diner.members.customer_id
			    WHERE dannys_diner.sales.order_date < dannys_diner.members.join_date) AS tbd
			    GROUP by customer_id, product_id) as tbd
JOIN dannys_diner.menu
	ON tbd.product_id = dannys_diner.menu.product_id
ORDER BY tbd.customer_id) AS tbd
GROUP BY customer_id
;
```

**Output:**

| customer_id | total_items | total_amount |
| --- | --- | --- |
| A | 2 | 25 |
| B | 3 | 40 |

## If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

### ********************************************************I propose this first solution but the case() function:********************************************************

1. First we made a query to obtain the sushi points by each customer:

```sql
SELECT DISTINCT
	dannys_diner.sales.customer_id,
  ((dannys_diner.menu.price * 10) * 2) AS points 
FROM dannys_diner.sales
JOIN dannys_diner.menu
	ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
WHERE dannys_diner.menu.product_name = 'sushi'
ORDER BY dannys_diner.sales.customer_id
;
```

**Output:**

| customer_id | points |
| --- | --- |
| A | 200 |
| B | 200 |
1. First we made a query to obtain the others products points by each customer:

```sql
SELECT DISTINCT
	dannys_diner.sales.customer_id,
  sum(dannys_diner.menu.price * 10) AS points
FROM dannys_diner.sales
JOIN dannys_diner.menu
	ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
WHERE dannys_diner.menu.product_name != 'sushi'
GROUP BY customer_id
ORDER BY dannys_diner.sales.customer_id
;
```

**Output:**

| customer_id | points |
| --- | --- |
| A | 660 |
| B | 540 |
| C | 360 |
1. Now, we use the both querys to mix it and obtain the data to response the question:

```sql
WITH other_p AS (
  	SELECT DISTINCT
          dannys_diner.sales.customer_id,
          sum(dannys_diner.menu.price * 10) AS points
      FROM dannys_diner.sales
      JOIN dannys_diner.menu
          ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
      WHERE dannys_diner.menu.product_name <> 'sushi'
  	  GROUP BY customer_id
      ORDER BY dannys_diner.sales.customer_id)
SELECT
	other_p.customer_id,
    (other_p.points + sushi.points) as total_points 
FROM other_p
JOIN (SELECT
      *
      FROM (SELECT DISTINCT
          dannys_diner.sales.customer_id,
          ((dannys_diner.menu.price * 10) * 2) AS points 
      FROM dannys_diner.sales
      JOIN dannys_diner.menu
          ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
      WHERE dannys_diner.menu.product_name = 'sushi'
      ORDER BY dannys_diner.sales.customer_id) as sushi) as sushi
ON other_p.customer_id = sushi.customer_id
;
```

**********Output:**********

| ustomer_id | total_points |
| --- | --- |
| A | 860 |
| B | 740 |

How we can see the query is complicated and not efficient and hard to read. For this we will try to use the case() function.

********************************************************I propose this second solution witha case() function:********************************************************

```sql
SELECT
	dannys_diner.sales.customer_id,
  SUM(CASE WHEN dannys_diner.menu.product_name = 'sushi'
					 THEN dannys_diner.menu.price * 20
			     ELSE dannys_diner.menu.price * 10 END) AS Total
FROM dannys_diner.sales 
JOIN dannys_diner.menu
	ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
GROUP BY dannys_diner.sales.customer_id
ORDER BY dannys_diner.sales.customer_id
;
```

**************Output:**************

| customer_id | total |
| --- | --- |
| B | 940 |
| C | 360 |
| A | 860 |

Using the case function, the query is cleaner, more exact, and more efficient.

## In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January

```sql
SELECT
	dannys_diner.sales.customer_id,
	SUM(CASE WHEN dannys_diner.menu.product_name = 'sushi'
					 THEN dannys_diner.menu.price * 20
			     ELSE dannys_diner.menu.price * 10 END) AS Total
FROM dannys_diner.sales 
JOIN dannys_diner.menu
	ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
JOIN dannys_diner.members
	ON dannys_diner.sales.customer_id = dannys_diner.members.customer_id
WHERE dannys_diner.sales.customer_id <> 'C'
	AND dannys_diner.sales.order_date >= dannys_diner.members.join_date
GROUP BY dannys_diner.sales.customer_id
ORDER BY dannys_diner.sales.customer_id
;
```

**Output:**

| customer_id | total |
| --- | --- |
| A | 510 |
| B | 440 |
