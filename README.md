# Plato pizza sales


#### Project overview
The goal of this data analysis project is to shed light on a pizza restaurant's sales results from the previous year. Through a comprehensive examination of the sales data, our aim is to discern patterns, provide evidence-based suggestions, and get a more profound comprehension of the organization's functioning.


#### Data sources
Pizza data: The dataset contains 4 tables in a csv format


#### Tools

-	Excel  - Data cleaning
	- [Download here](https://drive.google.com/drive/folders/1sT5AReif21UXjW1kICtZPrBb8yshNSOs?usp=sharing)
-	MySQL – Data analysis
-	Tableau  – Data visualization
	- [View here](https://public.tableau.com/views/Platopizzasales/salesovercategory?:language=en-GB&:sid=&:display_count=n&:origin=viz_share_link)


#### Data Cleaning 

The following tasks were carried out
1.	Data loading and inspection
2.	Handling missing values
3.	Data cleaning and formatting


#### Exploratory Data Analysis
The sales data was explored to answer key questions such as:
-	What days and times tend to be the busiest?
-	What are the best and worst selling pizzas?
-	What's our average order value?
-	How much money did we make this year? Can we identify any seasonality in the sales?


#### Data Analysis
``` sql

---- Busiest day of the week

SELECT WEEKDAY(td.date) AS "Day of the Week",
       COUNT(od.order_details_id) AS "Total Orders",
       SUM(pz.price * od.quantity) AS "Daily Revenue"
FROM test.orders AS td
JOIN test.order_details AS od
ON td.order_id = od.order_id
JOIN test.pizzas pz
ON od.pizza_id = pz.pizza_id
GROUP BY WEEKDAY(td.date)
ORDER BY "Total Orders";

--- Busiest hour of the day

SELECT HOUR(td.time) AS "Hour of the day",
       COUNT(td.Order_id) AS "Total Orders"
FROM test.orders AS td
JOIN test.order_details AS od
ON td.order_id = od.order_id
JOIN test.pizzas pz
ON od.pizza_id = pz.pizza_id
GROUP BY HOUR(td.time)
ORDER BY "Total Orders";

---- Best/worst selling pizza

SELECT pt.name "Pizza Name",
	AVG(od.quantity * pz.price) "Average sales"
FROM test.order_details AS od
JOIN test.orders td 
ON od.order_id = td.order_id
JOIN test.pizzas pz
ON pz.pizza_id = od.pizza_id
JOIN test.pizza_types pt
ON pz.pizza_type_id = pt.pizza_type_id
GROUP BY pt.name
ORDER BY "Average sales" DESC;

----- Best/worst pizza category

SELECT pt.category "Pizza Category",
  COUNT(td.Order_id) AS "Total Orders",
	AVG(od.quantity * pz.price) "Average sales"
FROM test.order_details AS od
JOIN test.orders td 
ON od.order_id = td.order_id
JOIN test.pizzas pz
ON pz.pizza_id = od.pizza_id
JOIN test.pizza_types pt
ON pz.pizza_type_id = pt.pizza_type_id
GROUP BY pt.category
ORDER BY "Average sales" DESC;

---- Average order value

SELECT AVG(Total_Price) AS "Average Order Value"
FROM (
SELECT td.order_id, SUM(pz.price * od.quantity) AS Total_Price
FROM test.orders AS td 
JOIN test.order_details AS od 
ON td.order_id = od.order_id
JOIN test.pizzas pz
ON od.pizza_id = pz.pizza_id
GROUP BY td.order_id) AS Order_Values;

---- Total sales for the year

SELECT SUM(Total_Price) AS "Total Sales"
FROM (
SELECT td.order_id, SUM(pz.price * od.quantity) AS Total_Price
FROM test.orders AS td 
JOIN test.order_details AS od 
ON td.order_id = od.order_id
JOIN test.pizzas pz
ON od.pizza_id = pz.pizza_id
GROUP BY td.order_id) AS Order_Values;

----- Total order in the year

SELECT COUNT(td.Order_id) AS "Total Orders"
FROM test.orders AS td
JOIN test.order_details AS od
ON td.order_id = od.order_id
JOIN test.pizzas pz
ON od.pizza_id = pz.pizza_id
GROUP BY "Total Orders";

---- Monthly order

SELECT MONTH(td.date) AS Month,
       COUNT(td.Order_id) AS "Total Orders"
FROM test.orders AS td
JOIN test.order_details AS od
ON td.order_id = od.order_id
JOIN test.pizzas pz
ON od.pizza_id = pz.pizza_id
GROUP BY MONTH(td.date)
ORDER BY "Total Orders";

---- Monthly Revenue

SELECT MONTH(td.date) AS Month,
       SUM(pz.price * od.quantity) AS Revenue
FROM test.orders AS td
JOIN test.order_details AS od
ON td.order_id = od.order_id
JOIN test.pizzas pz
ON od.pizza_id = pz.pizza_id
GROUP BY MONTH(td.date)
ORDER BY Revenue DESC;

```

#### Result/Finding

The analysis results are summarized as follows:
- The restaurant sales have been good over the past year with no significant change. 
- The Classic pizza category had the highest sales with 26.906% of the total sale comprising of 10,815 orders
- The Veggie pizza category had the lowest sales with 23.683% of the total sale comprising of 11,449 orders
- The Brie Carre Pizza in the Supreme category which has the highest sale has the lowest amount of order.
- The Total order received in the past year is 48,620.
- The Average order value is 38.30.
- The month of July had the highest revenue with sales of 72,557 with 4,301 count of order.
- The month of October had the lowest sales of 64,027 with 3797 count of order.
- The busiest hour is 12 noon and the busiest day of the week is Friday

#### Recommendations
Based on the analysis, we recommend the following actions:
1. Invest in marketing and promotions during the weekend.
2. Run a customer feedback on the services run by the pizza restaurant
3. Invest in delivery service
4. Focus on expanding the Classic pizza category 
5. To successfully target high level customers, use a customer segmentation strategy.



   
