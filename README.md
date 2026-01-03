
# SQL Menu Sales Analysis Project

## Project Overview

This project explores sales and ordering patterns from a restaurant menu dataset using SQL. The goal is to uncover insights about menu performance, customer behavior, and revenue trends. The findings help stakeholders identify best‑selling items, underperforming items, and peak business hours.

## Executive Summary

This SQL analytics project evaluates customer behaviour and purchasing patterns following the launch of a new restaurant menu. Using transactional order data, the analysis identifies top-performing menu items, popular categories, revenue drivers, and time-based ordering trends.

The insights generated from this project help stakeholders understand what customers prefer, which items generate the most revenue, and where operational or pricing adjustments may improve overall performance. The project demonstrates practical SQL analytics skills using aggregations, joins, subqueries, CTEs, and window functions.

## Business Problem

After introducing a new menu, the restaurant needs data-driven insights to evaluate its success. Management wants to understand:

- Which menu items are most popular among customers
- Which categories perform best by volume and revenue
- Whether premium-priced items are being ordered
- What times of day experience the highest demand
- Which items underperform or are never ordered

Without this analysis, decisions around menu optimization, pricing strategy, inventory planning, and staff scheduling would rely on assumptions rather than evidence.

This project addresses these challenges by analysing historical order data to uncover customer preferences and sales trends.

---

## 📝 Key SQL Findings

### **1. Total Number of Times Each Menu Item Was Ordered**

![Executive Overview](https://github.com/ZakheniAnalyst/SQL-Project/blob/main/The%20top-selling%20item%20for%20each%20day.png)

Identified how often each menu item appears in customer orders to discover the most frequently ordered items.

### **2. Number of Times Each Category Was Ordered**

```sql
SELECT 
    mi.item_name,
    COUNT(od.order_id) AS total_orders
FROM order_details od
JOIN menu_items mi
ON od.item_id = mi.menu_item_id
GROUP BY mi.item_name
ORDER BY total_orders DESC;

````

### 3.  Categories with more than 100 orders (Shows only high-volume categories using the HAVING clause.)

```sql
SELECT 
    mi.category,
    COUNT(od.order_id) AS total_orders
FROM order_details od
JOIN menu_items mi 
ON od.item_id = mi.menu_item_id
GROUP BY mi.category
HAVING COUNT(od.order_id) > 100
ORDER BY total_orders DESC;
````

### **4. Top-Performing Menu Items by Revenue**

Calculate revenue per menu item using price × quantity, then rank items by total revenue generated.

### **5. Top 5 Most Ordered Items**

Highlight the best-selling items based on order frequency.

### **6. Top-Selling Item for Each Day**

Determine the highest‑selling menu item for each calendar day to understand daily customer preferences.

### **7. Busiest hour of the day (Discover when customers order the most.)**
```sql
SELECT  
    HOUR(STR_TO_DATE(order_time, '%H:%i:%s')) AS hour_of_day,
    COUNT(order_id) AS total_orders
FROM order_details
GROUP BY hour_of_day
ORDER BY total_orders DESC
LIMIT 1;
Identified peak ordering hours by grouping orders by hour to understand when customer traffic is highest.
-- Insight: Helps in scheduling staff efficiently.
```

### **8. Items That Were Never Ordered**

Detect menu items with zero recorded sales—useful for menu optimization or promotional strategies.

### **9. Monthly Revenue Trend**

Tracked monthly revenue totals to observe growth patterns, seasonal changes, or dips in sales.
```sql
SELECT 
    DATE_FORMAT(STR_TO_DATE(order_date, '%d/%m/%Y'), '%Y-%m') AS month,
    ROUND(SUM(mi.price),2) AS total_revenue
FROM order_details od
JOIN menu_items mi 
ON od.item_id = mi.menu_item_id
GROUP BY month
ORDER BY month;
```
 Monthly revenue trend (Tracking revenue by month to spot growth or decline.)


### 10.
-- Top 3 selling items per category (Using RANK(Window Function) to find leading items in each category)

```sql
SELECT category, item_name, total_orders
FROM (
    SELECT 
        mi.category,
        mi.item_name,
        COUNT(od.order_id) AS total_orders,
        RANK() OVER (PARTITION BY mi.category ORDER BY COUNT(od.order_id) DESC) AS category_rank
    FROM order_details od
    JOIN menu_items mi 
    ON od.item_id = mi.menu_item_id
    GROUP BY mi.category, mi.item_name
) ranked
WHERE category_rank <= 3;
```
# Business Value

The insights from this project enable the restaurant to:

-Promote best-selling and high-revenue items
-Review or remove items that do not sell
-Optimize pricing tiers and menu structure
-Improve staff scheduling during peak hours
-Support future menu launches with historical evidence

## Strategic Recommendations

-Promote top-performing items through featured placements and meal bundles
-Rationalize the menu by reviewing or removing unsold items
-Leverage premium pricing where customers already show willingness to pay
-Optimize staffing levels during identified peak hours
-Use daily and monthly trends to guide inventory planning and promotions

## 🛠️ SQL Skills Demonstrated

* Aggregating and grouping data
* Window functions for ranking
* Joins across menu, order, and sales tables
* Date and time extraction
* Revenue calculations
* Subqueries and CTEs

---

## 📁 Repository Structure




🎉 *Thank you for exploring this SQL analysis project!*

