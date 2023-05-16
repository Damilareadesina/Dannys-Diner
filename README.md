
<h1>Damilareadesina - Danny's Diner </h1>

<h2>Description</h2>
Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.

Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business. 


Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers. He plans on using these insights to help him decide whether he should expand the existing customer loyalty program.
<br />


<h2>Tools Used</h2>

- <b>Structure Query Language </b>



<h2>Environments Used </h2>

- <b>MY SQL WORK BENCH </b>

<h2>Process walk-through:</h2>


  <p align="center"> 
   1. What is the total amount each customer spent at the restaurant?

SELECT sales.customer_id, SUM(menu.price) AS total_spending
FROM sales
LEFT JOIN menu 
	ON menu.product_id = sales.product_id
    GROUP BY sales.customer_id; <br />
	<img src="https://github.com/Damilareadesina/Dannys-Diner/assets/126564128/7b6726a3-3e71-4091-87a3-4cd3da8b70d4.jpg"/>
<br />
<br />

<p align="center">
 2. How many days has each customer visited the restau
<p align="center">
SELECT sales.customer_id, 
    COUNT(DISTINCT sales.order_date) as Days_visiting
    FROM sales
    GROUP BY customer_id; <br />
<img src="https://github.com/Damilareadesina/Dannys-Diner/assets/126564128/6eb453d9-6e9c-48bc-8633-e592066e2db1.JPG"/>
  <p align="center"> 
<br />
<br />  
<p align="center"> 
3. What was the first item from the menu purchased by each customer?
   
SELECT sales.customer_id,menu.product_name
   FROM sales
   LEFT JOIN menu
   ON menu.product_id = sales.product_id
    GROUP BY customer_id; <br />
<img src="https://github.com/Damilareadesina/Dannys-Diner/assets/126564128/940e5e48-b356-4730-8005-71c5e5bbe561.JPG"/>
  <p align="center"> 
 <br />
<br />
 4. What is the most purchased item on the menu and how many times was it purchased by all customers?

SELECT product_name AS most_purchase_item, 
COUNT(sales.product_id) AS times_purchased
FROM menu 
JOIN sales 
ON menu.product_id = sales.product_id
GROUP BY menu.product_name
ORDER BY times_purchased DESC
LIMIT 1;<br />
<img src="https://github.com/Damilareadesina/Dannys-Diner/assets/126564128/ea943337-56e6-4d13-987a-4a64967d8d9c.JPG"/>
  <p align="center"> 
 <br />
<br />

<p align="center">
 5. Which item was the most popular for each customer?

SELECT customer_id, product_name, 
Count(product_name) AS time_purchased
FROM sales
JOIN menu 
on sales.product_id = menu.product_id
GROUP BY customer_id, product_name
ORDER BY (time_purchased ) DESC;<br />
 <img src="https://github.com/Damilareadesina/Dannys-Diner/assets/126564128/1f4eab7b-3672-483b-8a93-8dcdfb78dd8a.JPG"/>
  <p align="center"> 
<br />
<br />
<p align="center">
 6. Which item was purchased first by the customer after they became a member?

SELECT DISTINCT members.customer_id, join_date,menu.product_name, order_date
FROM members
JOIN sales
ON sales.customer_id = members.customer_id
JOIN menu
ON sales.product_id =menu.product_id
WHERE order_date >= join_date  
GROUP BY members.customer_id
ORDER BY order_date AND join_date
LIMIT 2;<br />
<img src="https://github.com/Damilareadesina/Dannys-Diner/assets/126564128/635e67f9-e9cd-4679-be35-6ee89d9c4b47.JPG"/>
  <p align="center"> 
<br />
<br />
<p align="center">
 7. Which item was purchased just before the customer became a member?

SELECT DISTINCT members.customer_id, join_date,product_id, order_date
FROM members
JOIN sales
ON sales.customer_id = members.customer_id
WHERE order_date < join_date  
GROUP BY members.customer_id,product_id
ORDER BY order_date DESC
LIMIT 3;<br />
<img src="https://github.com/Damilareadesina/Dannys-Diner/assets/126564128/e29f9af8-7252-46d2-8c59-f8f6150ec870.JPG"/>
  <p align="center"> 
<br />
<br />
<p align="center">
 8. What is the total items and amount spent for each member before they became a member?

SELECT DISTINCT members.customer_id, join_date, COUNT(sales.customer_id) AS total_items, SUM(price) AS total_amount
FROM members
JOIN sales
ON members.customer_id = sales.customer_id
JOIN MENU 
ON sales.product_id = menu.product_id
WHERE order_date < join_date  
GROUP BY members.customer_id
ORDER BY order_date DESC
LIMIT 4;<br />

<img src="https://github.com/Damilareadesina/Dannys-Diner/assets/126564128/e82d253a-1ec9-4048-9cf4-5b5fe60fc3a4.JPG"/>
  <p align="center"> 
<br />
<br />
 <p align="center">

9.  If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

SELECT sales.customer_id,  

SUM(CASE WHEN sales.product_id = 1 THEN price * 20 ELSE price * 10 END) AS points

FROM sales

JOIN menu

ON sales.product_id = menu.product_id

 GROUP BY customer_id;

<img src="https://github.com/Damilareadesina/Dannys-Diner/assets/126564128/53eea35e-1989-4e17-b3dc-e0ae98f4cde3.JPG"/>
  <p align="center">
<br />
<br />
<p align="center">


10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

SELECT sales.customer_id, MONTH(join_date) AS month,

SUM(CASE WHEN sales.order_date BETWEEN join_date AND DATE_ADD(join_date, INTERVAL 1 WEEK) THEN price * 20 

WHEN MONTH(join_date) = MONTH(sales.order_date) THEN 

CASE WHEN sales.product_id = 1 THEN price * 20 ELSE price * 10 END END)  AS points 

FROM sales

JOIN menu

ON sales.product_id = menu.product_id

JOIN members

ON sales.customer_id = members.customer_id

GROUP BY customer_id ;

<img src="https://github.com/Damilareadesina/Dannys-Diner/assets/126564128/3a55d93d-b1da-4375-89e2-285807c96157.JPG"/>
  <p align="center">

<br />
<br />
