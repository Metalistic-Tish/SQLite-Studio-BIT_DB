# SQLite-Studio-BIT_DB
In this SQL, I'm querying a database with 6 tables in it to answer statistics about customer and order data. 

Question 1. How many orders were placed in January? 

SELECT COUNT(orderid)
FROM BIT_DB.JanSales
WHERE length(orderid) = 6 
AND orderid <> 'Order ID'

Question 2. How many of those orders were for an iPhone? 
SELECT COUNT(orderid)
FROM BIT_DB.JanSales
WHERE Product='iPhone'
AND length(orderid) = 6 
AND orderid <> 'Order ID'

Question 3. Select the customer account numbers for all the orders that were placed in February. 
SELECT distinct acctnum
FROM BIT_DB.customers cust

INNER JOIN BIT_DB.FebSales Feb
ON cust.order_id=FEB.orderid
WHERE length(orderid) = 6 
AND orderid <> 'Order ID'

Question 4. Which product was the cheapest one sold in January, and what was the price? 
SELECT distinct Product, price
FROM BIT_DB.JanSales
WHERE  price in (SELECT min(price) FROM BIT_DB.JanSales)
#OR 
SELECT distinct product, price FROM BIT_DB.JanSales 
ORDER BY price ASC LIMIT 1

Question 5. What is the total revenue for each product sold in January?
SELECT sum(quantity)*price as revenue
,product
FROM BIT_DB.JanSales
GROUP BY product

Question 6. Which products were sold in February at 548 Lincoln St, Seattle, WA 98101, how many of each were sold, and what was the total revenue?
select 
sum(Quantity), 
product, 
sum(quantity)*price as revenue
FROM BIT_DB.FebSales 
WHERE location = '548 Lincoln St, Seattle, WA 98101'
GROUP BY product

Question 7. How many customers ordered more than 2 products at a time, and what was the average amount spent for those customers? 
select 
count(distinct cust.acctnum), 
avg(quantity*price)
FROM BIT_DB.FebSales Feb
LEFT JOIN BIT_DB.customers cust
ON FEB.orderid=cust.order_id
WHERE Feb.Quantity>2
AND length(orderid) = 6 
AND orderid <> 'Order ID'
