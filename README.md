# Sql-Restaurant-Analysis

## Table of Contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](#data-cleaning)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results/Findings](#results-findings)
- [Recommendations](#recommendations)
  


### Project Overview
The International Cuisine Restaurant offers a diverse menu inspired by flavors from around the world, providing a unique dining experience that celebrates global culinary traditions. As the company's Analyst, I utilize SQL to monitor and analyze menu performance, identifying customer favorites and underperforming items to ensure the menu stays dynamic and aligned with our guests tastes.


### Data Sources

The dataset for this analysis is provided by Maven Analytics. It consist of two tables; the item data this analysis is the Menu_item file, which provides detailed information about each menu item offered by the company The order details contains comprehensive information about all orders placed with the company.

### Tools

- Excel - Data Cleaning
- MySQL - Data Analysis [Download here

### Data Cleaning/Preparation

In the initial data preparation phase, we performed the following tasks:

1. Data loading and inspection.
2. Handling missing values.
3. Data cleaning and formatting.
 
### Exploratory Data Analysis

  
The exploratory data analysis (EDA) focused on examining the sales data to address key questions, such as:
### menu_item

1. Find the number of items on the menu.
2. What are the least and most expensive item on the menu ?
3. How many italian dishes are on the menu ?
4. What are the least and most expensive italian dishes on the menu ?
5. How many dishes are in each category ?
6. What is the average dish price within each category ?  

### order_details

1. What is the date range of the table ?
2. How many orders where made within this date range ?
3. How many items where ordered within this date range ?
4. Which orders had the most number of items ?
5. How many orders had more than 12 items ?

### Combine the menu_items table and order_details table into a single table

1. What were the least and most ordered items ? What categories where they in ?
2. What were the top 5 orders that spent the most money ?
3. View the details of the highest spent order. What insights can you gather from them ?
4. View the details of top 5 highest spent orders. What insights can you gather from them ?

# Data Analysis

1. View the menu_items table

``` sql
SELECT * FROM menu_items;
```
2. Find the number of items on the menu

``` sql
SELECT COUNT(*)
FROM menu_items;
```

3. What are the least and most expensive item on the menu ?

``` sql
SELECT *
FROM menu_items
ORDER BY price;

SELECT *
FROM menu_items
ORDER BY price DESC;
```

4. How many italian dishes are on the menu ?

``` sql
SELECT COUNT(*) FROM menu_items
WHERE category='Italian'
```

5. What are the least and most expensive italian dishes on the menu ?

``` sql
SELECT *
FROM menu_items
WHERE category = 'italian'
ORDER BY price;

SELECT *
FROM menu_items
WHERE category = 'italian'
ORDER BY price DESC;
```

6. How many dishes are in each category ?

``` sql
SELECT category, COUNT(menu_item_id) AS num_dishes
FROM menu_items
GROUP BY category
```

7. What is the average dish price within each category ?

``` sql
SELECT category, AVG(price)
FROM menu_items
GROUP BY category;
```

___________________________________________________________

1. View order_details table.

``` sql
SELECT * FROM order_details;
```
  
2. What is the date range of the table ?

``` sql
SELECT MIN(order_date), MAX(order_date) FROM order_details;
```

3. How many orders where made within this date range ?

``` sql
SELECT COUNT(DISTINCT order_id) FROM order_details
```

4. How many items where ordered within this date range ?

``` sql
SELECT COUNT(*)
FROM order_details;
```

5. Which orders had the most number of items ?

``` sql
SELECT order_id, COUNT(item_id) AS num_items
FROM order_details
GROUP BY order_id
ORDER BY num_items DESC;
```

6. How many orders had more than 12 items ?

``` sql
SELECT COUNT(*) FROM
(SELECT order_id, COUNT(item_id) AS num_items
FROM order_details
GROUP BY order_id
HAVING num_items > 12) AS num_orders;
```
_________________________________________________________________________________

1. Combine the menu_items table and order_details table into a single table

``` sql
SELECT * FROM menu_items;
SELECT * FROM order_details;

SELECT *
FROM order_details od LEFT JOIN menu_items mi
     ON od.item_id = mi.menu_item_id;
```

2. What were the least and most ordered items ? What categories where they in ?

``` sql
SELECT item_name, category, COUNT(order_details_id) AS num_purchases
FROM order_details od LEFT JOIN menu_items mi
     ON od.item_id = mi.menu_item_id
GROUP BY item_name, category
ORDER BY num_purchases DESC;
```

3. What were the top 5 orders that spent the most money ?

``` sql
SELECT order_id, SUM(price) AS total_spend
FROM order_details od LEFT JOIN menu_items mi
     ON od.item_id = mi.menu_item_id
GROUP BY order_id
ORDER BY total_spend DESC
LIMIT 5;
```

4. View the details of the highest spent order. What insights can you gather from them ?

``` sql
SELECT category, COUNT(item_id) AS num_items
FROM order_details od LEFT JOIN menu_items mi
     ON od.item_id = mi.menu_item_id
WHERE order_id = 440
GROUP BY category
```

5. View the details of top 5 highest spent orders. What insights can you gather from them ?

``` sql
SELECT order_id, category, COUNT(item_id) AS num_items
FROM order_details od LEFT JOIN menu_items mi
ON od.item_id = mi.menu_item_id
WHERE order_id IN (440, 2075, 1957, 330, 2675 )
GROUP BY order_id, category;
```


### Results/Findings

Using SQL analysis, it was identified that the most expensive menu item is the Shrimp Scampi from the Italian category, while the least expensive is Edamame from the Asian category. Additionally, the American Hamburger emerged as the most frequently ordered item, whereas the Mexican Chicken Tacos had the lowest order frequency.

### Recommendations

1. Create Lunch Specials: Introduce lunch specials featuring underperforming items like Chicken Tacos, Pot Stickers, and Lasagna to increase their visibility and appeal to the lunch crowd.
2. Bundle Deals: Offer meal bundles that pair these items with popular sides or drinks, incentivizing customers to try them while enhancing the overall dining experience.
3. Menu Promotions: Highlight these dishes through targeted marketing campaigns, such as limited-time discounts, chef’s recommendations, or social media posts to generate curiosity and boost sales.


### Reference

- Maven Analytics 
