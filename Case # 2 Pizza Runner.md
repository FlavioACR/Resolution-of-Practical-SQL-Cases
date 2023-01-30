# Case #2  Pizza Runner

Created time: January 25, 2023 9:13 AM
Last edited time: January 27, 2023 4:19 PM
Tags: Data Study Cases

# **Introduction**

Did you know that over **115 million kilograms** of pizza is consumed daily worldwide??? (Well according to Wikipedia anyway…) 

Danny was scrolling through his Instagram feed when something really caught his eye - “80s Retro Styling and Pizza Is The Future!” 

Danny was sold on the idea, but he knew that pizza alone was not going to help him get seed funding to expand his new Pizza Empire - so he had one more genius idea to combine with it - he was going to *Uberize* it - and so Pizza Runner was launched! Danny started by recruiting “runners” to deliver fresh pizza from Pizza Runner Headquarters (otherwise known as Danny’s house) and also maxed out his credit card to pay freelance developers to build a mobile app to accept orders from customers.

![https://8weeksqlchallenge.com/images/case-study-designs/2.png](https://8weeksqlchallenge.com/images/case-study-designs/2.png)

# **Available Data**

Because Danny had a few years of experience as a data scientist - he was very aware that data collection was going to be critical for his business’ growth.

He has prepared for us an entity relationship diagram of his database design but requires further assistance to clean his data and apply some basic calculations so he can better direct his runners and optimise Pizza Runner’s operations.

All datasets exist within the `pizza_runner` database schema - be sure to include this reference within your SQL scripts as you start exploring the data and answering the case study questions.

### **Entity Relationship Diagram**

### **Table 1: runners**

The `runners` table shows the `registration_date` for each new runner

| runner_id | registration_date |
| --- | --- |
| 1 | 2021-01-01 |
| 2 | 2021-01-03 |
| 3 | 2021-01-08 |
| 4 | 2021-01-15 |

### **Table 2: customer_orders**

Customer pizza orders are captured in the `customer_orders` table with 1 row for each individual pizza that is part of the order.

The `pizza_id` relates to the type of pizza which was ordered whilst the `exclusions` are the `ingredient_id` values which should be removed from the pizza and the `extras` are the `ingredient_id` values which need to be added to the pizza.

Note that customers can order multiple pizzas in a single order with varying `exclusions` and `extras` values even if the pizza is the same type!

The `exclusions` and `extras` columns will need to be cleaned up before using them in your queries.

| order_id | customer_id | pizza_id | exclusions | extras | order_time |
| --- | --- | --- | --- | --- | --- |
| 1 | 101 | 1 |  |  | 2021-01-01 18:05:02 |
| 2 | 101 | 1 |  |  | 2021-01-01 19:00:52 |
| 3 | 102 | 1 |  |  | 2021-01-02 23:51:23 |
| 3 | 102 | 2 |  | NaN | 2021-01-02 23:51:23 |
| 4 | 103 | 1 | 4 |  | 2021-01-04 13:23:46 |
| 4 | 103 | 1 | 4 |  | 2021-01-04 13:23:46 |
| 4 | 103 | 2 | 4 |  | 2021-01-04 13:23:46 |
| 5 | 104 | 1 | null | 1 | 2021-01-08 21:00:29 |
| 6 | 101 | 2 | null | null | 2021-01-08 21:03:13 |
| 7 | 105 | 2 | null | 1 | 2021-01-08 21:20:29 |
| 8 | 102 | 1 | null | null | 2021-01-09 23:54:33 |
| 9 | 103 | 1 | 4 | 1, 5 | 2021-01-10 11:22:59 |
| 10 | 104 | 1 | null | null | 2021-01-11 18:34:49 |
| 10 | 104 | 1 | 2, 6 | 1, 4 | 2021-01-11 18:34:49 |

### **Table 3: runner_orders**

After each orders are received through the system - they are assigned to a runner - however not all orders are fully completed and can be cancelled by the restaurant or the customer.

The `pickup_time` is the timestamp at which the runner arrives at the Pizza Runner headquarters to pick up the freshly cooked pizzas. The `distance` and `duration` fields are related to how far and long the runner had to travel to deliver the order to the respective customer.

There are some known data issues with this table so be careful when using this in your queries - make sure to check the data types for each column in the schema SQL!

| order_id | runner_id | pickup_time | distance | duration | cancellation |
| --- | --- | --- | --- | --- | --- |
| 1 | 1 | 2021-01-01 18:15:34 | 20km | 32 minutes |  |
| 2 | 1 | 2021-01-01 19:10:54 | 20km | 27 minutes |  |
| 3 | 1 | 2021-01-03 00:12:37 | 13.4km | 20 mins | NaN |
| 4 | 2 | 2021-01-04 13:53:03 | 23.4 | 40 | NaN |
| 5 | 3 | 2021-01-08 21:10:57 | 10 | 15 | NaN |
| 6 | 3 | null | null | null | Restaurant Cancellation |
| 7 | 2 | 2020-01-08 21:30:45 | 25km | 25mins | null |
| 8 | 2 | 2020-01-10 00:15:02 | 23.4 km | 15 minute | null |
| 9 | 2 | null | null | null | Customer Cancellation |
| 10 | 1 | 2020-01-11 18:50:20 | 10km | 10minutes | null |

### **Table 4: pizza_names**

At the moment - Pizza Runner only has 2 pizzas available the Meat Lovers or Vegetarian!

| pizza_id | pizza_name |
| --- | --- |
| 1 | Meat Lovers |
| 2 | Vegetarian |

### **Table 5: pizza_recipes**

Each `pizza_id` has a standard set of `toppings` which are used as part of the pizza recipe.

| pizza_id | toppings |
| --- | --- |
| 1 | 1, 2, 3, 4, 5, 6, 8, 10 |
| 2 | 4, 6, 7, 9, 11, 12 |

### **Table 6: pizza_toppings**

This table contains all of the `topping_name` values with their corresponding `topping_id` value

| topping_id | topping_name |
| --- | --- |
| 1 | Bacon |
| 2 | BBQ Sauce |
| 3 | Beef |
| 4 | Cheese |
| 5 | Chicken |
| 6 | Mushrooms |
| 7 | Onions |
| 8 | Pepperoni |
| 9 | Peppers |
| 10 | Salami |
| 11 | Tomatoes |
| 12 | Tomato Sauce |

# **Interactive SQL Instance**

You can use the embedded DB Fiddle below to easily access these example datasets - this interactive session has everything you need to start solving these questions using SQL.

You can click on the `Edit on DB Fiddle` link on the top right hand corner of the embedded session below and it will take you to a fully functional SQL editor where you can write your own queries to analyse the data.

You can feel free to choose any SQL dialect you’d like to use, the existing Fiddle is using PostgreSQL 13 as default.

Serious SQL students can copy and paste the Schema SQL code below directly into their SQLPad GUI to create permanent tables or they can add a `TEMP` within the table creation steps like we’ve done throughout the entire course to keep our schemas clean and tidy!

# Creating schemas and tables:

```sql
CREATE SCHEMA pizza_runner;
SET search_path = pizza_runner;

DROP TABLE IF EXISTS runners;
CREATE TABLE runners (
  "runner_id" INTEGER,
  "registration_date" DATE
);
INSERT INTO runners
  ("runner_id", "registration_date")
VALUES
  (1, '2021-01-01'),
  (2, '2021-01-03'),
  (3, '2021-01-08'),
  (4, '2021-01-15');

DROP TABLE IF EXISTS customer_orders;
CREATE TABLE customer_orders (
  "order_id" INTEGER,
  "customer_id" INTEGER,
  "pizza_id" INTEGER,
  "exclusions" VARCHAR(4),
  "extras" VARCHAR(4),
  "order_time" TIMESTAMP
);

INSERT INTO customer_orders
  ("order_id", "customer_id", "pizza_id", "exclusions", "extras", "order_time")
VALUES
  ('1', '101', '1', '', '', '2020-01-01 18:05:02'),
  ('2', '101', '1', '', '', '2020-01-01 19:00:52'),
  ('3', '102', '1', '', '', '2020-01-02 23:51:23'),
  ('3', '102', '2', '', NULL, '2020-01-02 23:51:23'),
  ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
  ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
  ('4', '103', '2', '4', '', '2020-01-04 13:23:46'),
  ('5', '104', '1', 'null', '1', '2020-01-08 21:00:29'),
  ('6', '101', '2', 'null', 'null', '2020-01-08 21:03:13'),
  ('7', '105', '2', 'null', '1', '2020-01-08 21:20:29'),
  ('8', '102', '1', 'null', 'null', '2020-01-09 23:54:33'),
  ('9', '103', '1', '4', '1, 5', '2020-01-10 11:22:59'),
  ('10', '104', '1', 'null', 'null', '2020-01-11 18:34:49'),
  ('10', '104', '1', '2, 6', '1, 4', '2020-01-11 18:34:49');

DROP TABLE IF EXISTS runner_orders;
CREATE TABLE runner_orders (
  "order_id" INTEGER,
  "runner_id" INTEGER,
  "pickup_time" VARCHAR(19),
  "distance" VARCHAR(7),
  "duration" VARCHAR(10),
  "cancellation" VARCHAR(23)
);

INSERT INTO runner_orders
  ("order_id", "runner_id", "pickup_time", "distance", "duration", "cancellation")
VALUES
  ('1', '1', '2020-01-01 18:15:34', '20km', '32 minutes', ''),
  ('2', '1', '2020-01-01 19:10:54', '20km', '27 minutes', ''),
  ('3', '1', '2020-01-03 00:12:37', '13.4km', '20 mins', NULL),
  ('4', '2', '2020-01-04 13:53:03', '23.4', '40', NULL),
  ('5', '3', '2020-01-08 21:10:57', '10', '15', NULL),
  ('6', '3', 'null', 'null', 'null', 'Restaurant Cancellation'),
  ('7', '2', '2020-01-08 21:30:45', '25km', '25mins', 'null'),
  ('8', '2', '2020-01-10 00:15:02', '23.4 km', '15 minute', 'null'),
  ('9', '2', 'null', 'null', 'null', 'Customer Cancellation'),
  ('10', '1', '2020-01-11 18:50:20', '10km', '10minutes', 'null');

DROP TABLE IF EXISTS pizza_names;
CREATE TABLE pizza_names (
  "pizza_id" INTEGER,
  "pizza_name" TEXT
);
INSERT INTO pizza_names
  ("pizza_id", "pizza_name")
VALUES
  (1, 'Meatlovers'),
  (2, 'Vegetarian');

DROP TABLE IF EXISTS pizza_recipes;
CREATE TABLE pizza_recipes (
  "pizza_id" INTEGER,
  "toppings" TEXT
);
INSERT INTO pizza_recipes
  ("pizza_id", "toppings")
VALUES
  (1, '1, 2, 3, 4, 5, 6, 8, 10'),
  (2, '4, 6, 7, 9, 11, 12');

DROP TABLE IF EXISTS pizza_toppings;
CREATE TABLE pizza_toppings (
  "topping_id" INTEGER,
  "topping_name" TEXT
);
INSERT INTO pizza_toppings
  ("topping_id", "topping_name")
VALUES
  (1, 'Bacon'),
  (2, 'BBQ Sauce'),
  (3, 'Beef'),
  (4, 'Cheese'),
  (5, 'Chicken'),
	  (6, 'Mushrooms'),
  (7, 'Onions'),
  (8, 'Pepperoni'),
  (9, 'Peppers'),
  (10, 'Salami'),
  (11, 'Tomatoes'),
  (12, 'Tomato Sauce');
```

# **Case Study Questions**

This case study has **LOTS** of questions - they are broken up by area of focus including:

- Pizza Metrics
- Runner and Customer Experience
- Ingredient Optimisation
- Pricing and Ratings
- Bonus DML Challenges (DML = Data Manipulation Language)

Each of the following case study questions can be answered using a single SQL statement.

Again, there are many questions in this case study - please feel free to pick and choose which ones you’d like to try!

Before you start writing your SQL queries however - you might want to investigate the data, you may want to do something with some of those `null` values and data types in the `customer_orders` and `runner_orders` tables!

# Cleaning and Exploring the Data

## Table customer_order:

First we need to see the current information in the table to determinate how work or clean it:

```sql
-- Data types of the schema:
DROP TABLE IF EXISTS customer_orders;
CREATE TABLE customer_orders (
		  "order_id" INTEGER,
		  "customer_id" INTEGER,
		  "pizza_id" INTEGER,
		  "exclusions" VARCHAR(4),
		  "extras" VARCHAR(4),
		  "order_time" TIMESTAMP
);

-- Query to view the table:
SELECT * FROM pizza_runner.customer_orders;
```

| order_id | customer_id | pizza_id | exclusions | extras | order_time |
| --- | --- | --- | --- | --- | --- |
| 1 | 101 | 1 |  |  | 2020-01-01T18:05:02.000Z |
| 2 | 101 | 1 |  |  | 2020-01-01T19:00:52.000Z |
| 3 | 102 | 1 |  |  | 2020-01-02T23:51:23.000Z |
| 3 | 102 | 2 |  | null | 2020-01-02T23:51:23.000Z |
| 4 | 103 | 1 | 4 |  | 2020-01-04T13:23:46.000Z |
| 4 | 103 | 1 | 4 |  | 2020-01-04T13:23:46.000Z |
| 4 | 103 | 2 | 4 |  | 2020-01-04T13:23:46.000Z |
| 5 | 104 | 1 | null | 1 | 2020-01-08T21:00:29.000Z |
| 6 | 101 | 2 | null | null | 2020-01-08T21:03:13.000Z |
| 7 | 105 | 2 | null | 1 | 2020-01-08T21:20:29.000Z |
| 8 | 102 | 1 | null | null | 2020-01-09T23:54:33.000Z |
| 9 | 103 | 1 | 4 | 1, 5 | 2020-01-10T11:22:59.000Z |
| 10 | 104 | 1 | null | null | 2020-01-11T18:34:49.000Z |
| 10 | 104 | 1 | 2, 6 | 1, 4 |  |

So with this view we can see of in the exclusions and extra columns we have some empty values that are mixed with null and integer values. With this we can deeterminate the following steps to clean;

1. Update the values of the exclusion and extra columns, deleting the the wrong values, leaving only the varchar values already recorded in the table.
    1. exclusions column:
        
        ```sql
        UPDATE pizza_runner. customer_orders
         SET exclusions = ''
        WHERE exclusions IS NULL OR exclusions LIKE 'null';
        ```
        
    2. extras column:
        
        ```sql
        UPDATE pizza_runner. customer_orders
         SET extras = ''
        WHERE extras IS NULL OR extras LIKE 'null';
        ```
        
    
    **Output,** this is the output table then updated:
    
    | order_id | customer_id | pizza_id | exclusions | extras | order_time |
    | --- | --- | --- | --- | --- | --- |
    | 1 | 101 | 1 |  |  | 2020-01-01T18:05:02.000Z |
    | 2 | 101 | 1 |  |  | 2020-01-01T19:00:52.000Z |
    | 3 | 102 | 1 |  |  | 2020-01-02T23:51:23.000Z |
    | 4 | 103 | 1 | 4 |  | 2020-01-04T13:23:46.000Z |
    | 4 | 103 | 1 | 4 |  | 2020-01-04T13:23:46.000Z |
    | 4 | 103 | 2 | 4 |  | 2020-01-04T13:23:46.000Z |
    | 9 | 103 | 1 | 4 | 1, 5 | 2020-01-10T11:22:59.000Z |
    | 10 | 104 | 1 | 2, 6 | 1, 4 | 2020-01-11T18:34:49.000Z |
    | 5 | 104 | 1 |  | 1 | 2020-01-08T21:00:29.000Z |
    | 7 | 105 | 2 |  | 1 | 2020-01-08T21:20:29.000Z |
    | 3 | 102 | 2 |  |  | 2020-01-02T23:51:23.000Z |
    | 6 | 101 | 2 |  |  | 2020-01-08T21:03:13.000Z |
    | 8 | 102 | 1 |  |  | 2020-01-09T23:54:33.000Z |
    | 10 | 104 | 1 |  |  |  |

## Table runner_order:

First we need to see the current information in the table to determinate how work or clean it:

```sql
-- Data types of the schema:
DROP TABLE IF EXISTS runner_orders;
CREATE TABLE runner_orders (
	  "order_id" INTEGER,
	  "runner_id" INTEGER,
	  "pickup_time" VARCHAR(19),
	  "distance" VARCHAR(7),
	  "duration" VARCHAR(10),
	  "cancellation" VARCHAR(23)
);

-- Query to view the table:
SELECT * FROM pizza_runner.customer_orders;
-- Curren types values:
VALUES
  ('1', '1', '2020-01-01 18:15:34', '20km', '32 minutes', ''),
  ('2', '1', '2020-01-01 19:10:54', '20km', '27 minutes', ''),
  ('3', '1', '2020-01-03 00:12:37', '13.4km', '20 mins', NULL),
  ('4', '2', '2020-01-04 13:53:03', '23.4', '40', NULL),
  ('5', '3', '2020-01-08 21:10:57', '10', '15', NULL),
  ('6', '3', 'null', 'null', 'null', 'Restaurant Cancellation'),
  ('7', '2', '2020-01-08 21:30:45', '25km', '25mins', 'null'),
  ('8', '2', '2020-01-10 00:15:02', '23.4 km', '15 minute', 'null'),
  ('9', '2', 'null', 'null', 'null', 'Customer Cancellation'),
  ('10', '1', '2020-01-11 18:50:20', '10km', '10minutes', 'null');
```

| order_id | runner_id | pickup_time | distance | duration | cancellation |
| --- | --- | --- | --- | --- | --- |
| 1 | 1 | 2020-01-01 18:15:34 | 20km | 32 minutes |  |
| 2 | 1 | 2020-01-01 19:10:54 | 20km | 27 minutes |  |
| 3 | 1 | 2020-01-03 00:12:37 | 13.4km | 20 mins | null |
| 4 | 2 | 2020-01-04 13:53:03 | 23.4 | 40 | null |
| 5 | 3 | 2020-01-08 21:10:57 | 10 | 15 | null |
| 6 | 3 | null | null | null | Restaurant Cancellation |
| 7 | 2 | 2020-01-08 21:30:45 | 25km | 25mins | null |
| 8 | 2 | 2020-01-10 00:15:02 | 23.4 km | 15 minute | null |
| 9 | 2 | null | null | null | Customer Cancellation |
| 10 | 1 | 2020-01-11 18:50:20 | 10km | 10minutes |  |

*Steps to clean it:*

1. Update the values in each column if is type vachar but containd null like string.
    
    ```sql
    -- pickup_time column:
    UPDATE pizza_runner.runner_orders
    	SET pickup_time = ''
    WHERE pickup_time = 'null'
    OR pickup_time = NULL
    ;
    -- pickup_time column:
    UPDATE pizza_runner.runner_orders
    	SET distance	= ''
    WHERE distance	= 'null'
    OR distance = NULL
    ;
    -- duration	column:
    UPDATE pizza_runner.runner_orders
    	SET duration	= ''
    WHERE duration IS NULL OR duration LIKE 'null'
    ;
    -- cancellation column:
    UPDATE pizza_runner.runner_orders
    	SET cancellation= ''
    WHERE cancellation IS NULL OR cancellation LIKE 'null';
    ;
    ```
    
    **Output:**
    
    | order_id | runner_id | pickup_time | distance | duration | cancellation |
    | --- | --- | --- | --- | --- | --- |
    | 1 | 1 | 2020-01-01 18:15:34 | 20km | 32 minutes |  |
    | 2 | 1 | 2020-01-01 19:10:54 | 20km | 27 minutes |  |
    | 3 | 1 | 2020-01-03 00:12:37 | 13.4km | 20 mins |  |
    | 4 | 2 | 2020-01-04 13:53:03 | 23.4 | 40 |  |
    | 5 | 3 | 2020-01-08 21:10:57 | 10 | 15 |  |
    | 6 | 3 |  |  |  | Restaurant Cancellation |
    | 7 | 2 | 2020-01-08 21:30:45 | 25km | 25mins |  |
    | 8 | 2 | 2020-01-10 00:15:02 | 23.4 km | 15 minute |  |
    | 9 | 2 |  |  |  | Customer Cancellation |
    | 10 | 1 | 2020-01-11 18:50:20 | 10km | 10minutes |  |
2. Update the correct information in distance and duration deleting the km, minutes of each column:
    
    ```sql
    -- cleaning distance column:
    UPDATE pizza_runner.runner_orders
    	SET distance = TRIM('km' FROM distance)
    WHERE distance LIKE '%km'
    ;
    
    -- cleaning duration column:
    UPDATE pizza_runner.runner_orders
    	SET duration = TRIM('minutes' FROM duration)
    WHERE duration LIKE '%minutes'
    OR duration LIKE '%mins'
    OR duration LIKE '%minute'
    ;
    ```
    
    **************Output:**************
    
    | order_id | runner_id | pickup_time | distance | duration | cancellation |
    | --- | --- | --- | --- | --- | --- |
    | 6 | 3 |  |  |  | Restaurant Cancellation |
    | 9 | 2 |  |  |  | Customer Cancellation |
    | 4 | 2 | 2020-01-04 13:53:03 | 23.4 | 40 |  |
    | 5 | 3 | 2020-01-08 21:10:57 | 10 | 15 |  |
    | 1 | 1 | 2020-01-01 18:15:34 | 20 | 32 |  |
    | 2 | 1 | 2020-01-01 19:10:54 | 20 | 27 |  |
    | 3 | 1 | 2020-01-03 00:12:37 | 13.4 | 20 |  |
    | 7 | 2 | 2020-01-08 21:30:45 | 25 | 25 |  |
    | 8 | 2 | 2020-01-10 00:15:02 | 23.4 | 15 |  |
    | 10 | 1 | 2020-01-11 18:50:20 | 10 | 10 |  |

## Totalcode to clean the data:

```sql
-- exclusions column
UPDATE pizza_runner. customer_orders
 SET exclusions = ''
WHERE exclusions IS NULL OR exclusions LIKE 'null';

-- extras column.
UPDATE pizza_runner. customer_orders
 SET extras = ''
WHERE extras IS NULL OR extras LIKE 'null';

-- pickup_time column:
UPDATE pizza_runner.runner_orders
	SET pickup_time = ''
WHERE pickup_time = 'null'
OR pickup_time = NULL
;
-- pickup_time column:
UPDATE pizza_runner.runner_orders
	SET distance	= ''
WHERE distance	= 'null'
OR distance = NULL
;
-- duration	column:
UPDATE pizza_runner.runner_orders
	SET duration	= ''
WHERE duration IS NULL OR duration LIKE 'null'
;
-- cancellation column:
UPDATE pizza_runner.runner_orders
	SET cancellation= ''
WHERE cancellation IS NULL OR cancellation LIKE 'null';
;

-- cleaning distance column:
UPDATE pizza_runner.runner_orders
	SET distance = TRIM('km' FROM distance)
WHERE distance LIKE '%km'
;

-- cleaning duration column:
UPDATE pizza_runner.runner_orders
	SET duration = TRIM('minutes' FROM duration)
WHERE duration LIKE '%minutes'
OR duration LIKE '%mins'
OR duration LIKE '%minute'
;
```

### **A. Pizza Metrics**

1. How many pizzas were ordered?
    
    ```sql
    SELECT
        count(order_id) as total_orders
    FROM pizza_runner.customer_orders
    ;
    ```
    
    Output:
    
    | total_orders |
    | --- |
    | 14 |
2. How many unique customer orders were made?
    
    ```sql
    SELECT DISTINCT
    	customer_id
    FROM pizza_runner.customer_orders
    ;
    ```
    
    Output:
    
    | customer_id |
    | --- |
    | 101 |
    | 103 |
    | 104 |
    | 105 |
    | 102 |
3. How many successful orders were delivered by each runner?
    
    ```sql
    SELECT
    runner_id,
    count( cancellation) as order_delivered_by_runner
    FROM pizza_runner.runner_orders
    WHERE cancellation = ''
    GROUP BY runner_id
    ;
    ```
    
    Output:
    
    | runner_id | order_delivered_by_runner |
    | --- | --- |
    | 1 | 4 |
    | 2 | 3 |
    | 3 | 1 |
4. How many of each type of pizza was delivered?

```sql
SELECT
	pizza_runner.pizza_names.pizza_name, 
    count( cancellation) as order_delivered_by_pizza_type
FROM pizza_runner.runner_orders
JOIN pizza_runner.customer_orders
	ON pizza_runner.runner_orders.order_id = 
       pizza_runner.customer_orders.order_id
JOIN pizza_runner.pizza_names
	ON pizza_runner.customer_orders.pizza_id = pizza_runner.pizza_names.pizza_id
WHERE cancellation = ''
GROUP BY pizza_runner.pizza_names.pizza_name
;
```

Output:

| pizza_name | order_delivered_by_pizza_type |
| --- | --- |
| Meatlovers | 9 |
| Vegetarian | 3 |
1. How many Vegetarian and Meatlovers were ordered by each customer?

```sql
SELECT
	customer_id,
    count(pizza_name = 'Meatlovers') as meatlovers_orders,
    count(pizza_name = 'Vegetarian') as vegetarian_orders
FROM pizza_runner.customer_orders
JOIN pizza_runner.pizza_names
	ON pizza_runner.customer_orders.pizza_id = pizza_runner.pizza_names.pizza_id
GROUP BY customer_id
;
```

Output:

| customer_id | meatlovers_orders | vegetarian_orders |
| --- | --- | --- |
| 101 | 3 | 3 |
| 103 | 4 | 4 |
| 104 | 3 | 3 |
| 105 | 1 | 1 |
| 102 | 3 | 3 |
1. What was the maximum number of pizzas delivered in a single order?
    
    ```sql
    SELECT
    	MAX(tbd.pizzas_delivered_by_order) as max_pizzas_delivered_in_single_order 
    FROM (
      SELECT
    	pizza_runner.customer_orders.order_id,
    	count(pizza_runner.customer_orders.order_id) as pizzas_delivered_by_order
    FROM pizza_runner.runner_orders
    JOIN pizza_runner.customer_orders
    	ON pizza_runner.runner_orders.order_id =
        	pizza_runner.customer_orders.order_id
    WHERE cancellation = ''
    GROUP BY pizza_runner.customer_orders.order_id
    ORDER BY pizza_runner.customer_orders.order_id, pizzas_delivered_by_order) AS tbd
    ;
    ```
    
    Output:
    
    | max_pizzas_delivered_in_single_order |
    | --- |
    | 3 |
    
2. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
    
    ```sql
    SELECT
    	pizza_runner.customer_orders.customer_id,
        SUM(CASE 
            WHEN exclusions >='1' OR extras >= '1'
            THEN 1 ELSE 0 END) AS total_pizzas_with_changes,
        SUM(CASE 
            WHEN exclusions = '' OR extras = ''
            THEN 1 ELSE 0 END) AS  total_pizzas_without_changes
    FROM pizza_runner.runner_orders
    JOIN pizza_runner.customer_orders
    	ON pizza_runner.runner_orders.order_id =
        	pizza_runner.customer_orders.order_id
    WHERE cancellation = ''
    GROUP BY customer_id
    ;
    ```
    
    Output:
    
    | customer_id | total_pizzas_with_changes | total_pizzas_without_changes |
    | --- | --- | --- |
    | 101 | 0 | 2 |
    | 102 | 0 | 3 |
    | 105 | 1 | 1 |
    | 104 | 2 | 2 |
    | 103 | 3 | 3 |
    
3. How many pizzas were delivered that had both exclusions and extras?
    
    ```sql
    WITH both_ex_changes AS
    (
    SELECT
    	pizza_runner.customer_orders.customer_id,
        SUM(CASE 
            WHEN exclusions >='1' OR extras >= '1'
            THEN 1 ELSE 0 END) AS total_pizzas_with_changes,
        SUM(CASE 
            WHEN exclusions = '' OR extras = ''
            THEN 1 ELSE 0 END) AS  total_pizzas_without_changes
    FROM pizza_runner.runner_orders
    JOIN pizza_runner.customer_orders
    	ON pizza_runner.runner_orders.order_id =
        	pizza_runner.customer_orders.order_id
    WHERE cancellation = ''
    GROUP BY customer_id )
    SELECT
     customer_id,
     (total_pizzas_with_changes + total_pizzas_without_changes) AS Total_pizzas_both_changes
    FROM both_ex_changes
    WHERE total_pizzas_with_changes >= '1' 
    AND total_pizzas_without_changes >= '1'
    ;
    ```
    
    Output:
    
    | customer_id | total_pizzas_both_changes |
    | --- | --- |
    | 105 | 2 |
    | 104 | 4 |
    | 103 | 6 |

1. What was the total volume of pizzas ordered for each hour of the day?
    
    ```sql
    SELECT
    EXTRACT(HOUR FROM order_time) AS hour_of_day,
    COUNT(order_id) AS total_orders
    FROM pizza_runner.customer_orders
    GROUP BY hour_of_day
    ;
    ```
    
    Output:
    
    | hour_of_day | total_orders |
    | --- | --- |
    | 18 | 3 |
    | 23 | 3 |
    | 21 | 3 |
    | 11 | 1 |
    | 19 | 1 |
    | 13 | 3 |
2. What was the volume of orders for each day of the week?
    
    ```sql
    SELECT
    --EXTRACT(HOUR FROM order_time) AS hour_of_day,
    EXTRACT(DAY FROM order_time) AS day_of_week,
    COUNT(order_id) AS total_orders
    FROM pizza_runner.customer_orders
    GROUP BY day_of_week
    ORDER BY day_of_week
    ;
    ```
    
    Output:
    
    | day_of_week | total_orders |
    | --- | --- |
    | 1 | 2 |
    | 2 | 2 |
    | 4 | 3 |
    | 8 | 3 |
    | 9 | 1 |
    | 10 | 1 |
    | 11 | 2 |

```sql
SELECT
to_char(order_time, 'Day') as Dia,
COUNT(order_id) as cantidad_pizzas
FROM pizza_runner.customer_orders
GROUP BY Dia;
```

| dia | cantidad_pizzas |
| --- | --- |
| Saturday | 5 |
| Thursday | 3 |
| Friday | 1 |
| Wednesday | 5 |

### **B. Runner and Customer Experience**

1. How many runners signed up for each 1 week period? (i.e. week starts `2021-01-01`)
    
    ```sql
    WITH
    runner_by_week AS
    (SELECT
    		TO_CHAR(registration_date, 'WEEK') as registration_week,
    		count(runner_id) as qnty_of_runner_by_period
    		FROM pizza_runner.runners
    		GROUP BY runners.registration_date
    		ORDER BY registration_week
    		)
    SELECT
    	registration_week,
    	SUM(qnty_of_runner_by_period) AS total_runner_by_week
    FROM runner_by_week
    GROUP BY registration_week
    ORDER BY registration_week
    ;
    ```
    
    Output:
    
    | registration_week | total_runner_by_week |
    | --- | --- |
    | 1EEK | 2 |
    | 2EEK | 1 |
    | 3EEK | 1 |
    
2. What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?
    
    # Tengo que agregar a los codigos de limpieza el formato de las string y las tipo fechas a timestamp
    
3. Is there any relationship between the number of pizzas and how long the order takes to prepare?
4. What was the average distance travelled for each customer?
5. What was the difference between the longest and shortest delivery times for all orders?
6. What was the average speed for each runner for each delivery and do you notice any trend for these values?
7. What is the successful delivery percentage for each runner?

### **C. Ingredient Optimisation**

1. What are the standard ingredients for each pizza?
2. What was the most commonly added extra?
3. What was the most common exclusion?
4. Generate an order item for each record in the `customers_orders` table in the format of one of the following:
    - `Meat Lovers`
    - `Meat Lovers - Exclude Beef`
    - `Meat Lovers - Extra Bacon`
    - `Meat Lovers - Exclude Cheese, Bacon - Extra Mushroom, Peppers`
5. Generate an alphabetically ordered comma separated ingredient list for each pizza order from the `customer_orders` table and add a `2x` in front of any relevant ingredients
    - For example: `"Meat Lovers: 2xBacon, Beef, ... , Salami"`
6. What is the total quantity of each ingredient used in all delivered pizzas sorted by most frequent first?

### **D. Pricing and Ratings**

1. If a Meat Lovers pizza costs $12 and Vegetarian costs $10 and there were no charges for changes - how much money has Pizza Runner made so far if there are no delivery fees?
2. What if there was an additional $1 charge for any pizza extras?
    - Add cheese is $1 extra
3. The Pizza Runner team now wants to add an additional ratings system that allows customers to rate their runner, how would you design an additional table for this new dataset - generate a schema for this new table and insert your own data for ratings for each successful customer order between 1 to 5.
4. Using your newly generated table - can you join all of the information together to form a table which has the following information for successful deliveries?
    - `customer_id`
    - `order_id`
    - `runner_id`
    - `rating`
    - `order_time`
    - `pickup_time`
    - Time between order and pickup
    - Delivery duration
    - Average speed
    - Total number of pizzas
5. If a Meat Lovers pizza was $12 and Vegetarian $10 fixed prices with no cost for extras and each runner is paid $0.30 per kilometre traveled - how much money does Pizza Runner have left over after these deliveries?

### **E. Bonus Questions**

If Danny wants to expand his range of pizzas - how would this impact the existing data design? Write an `INSERT` statement to demonstrate what would happen if a new `Supreme` pizza with all the toppings was added to the Pizza Runner menu?

# **Conclusion**

I hope there were enough case study questions to keep you occupied - I kinda went out all out in this case study!

Ready for the next 8 Week SQL challenge case study? Click on the banner below to get started with case study #3!

**Solution:**

```sql

```

**Output:**
