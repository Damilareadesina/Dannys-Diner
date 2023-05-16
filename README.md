
<h1>Damilareadesina - Danny's Diner </h1>

<h2>Description</h2>
Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.

Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business. 


Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers. He plans on using these insights to help him decide whether he should expand the existing customer loyalty program.
<br />


<h2>Tools Used</h2>

- <b>Structure Query Language </b>



<h2>Environments Used </h2>

- <b>MY SQL</b>

<h2>Process walk-through:</h2>


  <p align="center"> 
   1. What is the total amount each customer spent at the restaurant?

SELECT sales.customer_id, SUM(menu.price) AS total_spending
FROM sales
LEFT JOIN menu 
	ON menu.product_id = sales.product_id
    GROUP BY sales.customer_id; <br />
	<img src="https://user-images.githubusercontent.com/126564128/230754211-675ceba1-c056-4d02-bc27-cdda8d18037a.JPG"/>
<br />
<br />

  
<p align="center">
 4. What is the most purchased item on the menu and how many times was it purchased by all customers?

SELECT product_name AS most_purchase_item, 
COUNT(sales.product_id) AS times_purchased
FROM menu 
JOIN sales 
ON menu.product_id = sales.product_id
GROUP BY menu.product_name
ORDER BY times_purchased DESC
LIMIT 1;<br />
<img src="https://user-images.githubusercontent.com/126564128/230757786-b5ed01b1-1de3-4624-b3cf-e4810ee47fd1.JPG"/>
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
 <img src="https://user-images.githubusercontent.com/126564128/230757786-b5ed01b1-1de3-4624-b3cf-e4810ee47fd1.JPG"/>
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
<img src="https://user-images.githubusercontent.com/126564128/230757786-b5ed01b1-1de3-4624-b3cf-e4810ee47fd1.JPG"/>
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
<img src="https://user-images.githubusercontent.com/126564128/230757786-b5ed01b1-1de3-4624-b3cf-e4810ee47fd1.JPG"/>
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

<img src="https://user-images.githubusercontent.com/126564128/230757786-b5ed01b1-1de3-4624-b3cf-e4810ee47fd1.JPG"/>
  <p align="center"> 
