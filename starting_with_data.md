Question 1: What are the top 5 best-selling products based on quantity ordered?

SQL Queries:
```
SELECT p.name, SUM(sr.total_ordered) AS total_ordered
FROM products p
JOIN sales_report sr ON p.sku = sr.productsku
GROUP BY p.name
ORDER BY total_ordered DESC
LIMIT 5;
```

Answer: 
| Name                               | Total Ordered |
|------------------------------------|---------------|
| Ballpoint LED Light Pen            | 456           |
| 17oz Stainless Steel Sport Bottle  | 334           |
| Leatherette Journal                | 319           |
| Spiral Journal with Pen            | 290           |
| Foam Can and Bottle Cooler         | 253           |




Question 2:   Which cities and countries have the highest engagement with a specific product category?

SQL Queries:
```
SELECT a.city,
       a.country,
       a.v2productcategory,
         SUM(a.timeonsite:: numeric) AS total_engagement
FROM products p
 JOIN all_sessions a ON p.sku = a.productsku
      where a.timeonsite is not null
      and a.city <> 'nocity'
      and a.v2productcategory<> 'nocategory'
      and a.country <> 'nocountry'
GROUP BY a.city, a.v2productcategory
ORDER BY total_engagement DESC
limit 5;
```

Answer:
| City          | Country       | Product Category                       | Total Engagement |
|---------------|---------------|----------------------------------------|------------------|
| Mountain View | United States | Apparel Men's Men's-T-Shirts           | 17,608           |
| Mountain View | United States | Nest Nest-USA                          | 14,185           |
| Mountain View | United States | Electronics                            | 12,933           |
| Mountain View | United States | Apparel Men's Men's-Outerwear          | 11,204           |
| Mountain View | United States | Shop by Brand Google                   | 10,074           |




Question 3: 


SQL Queries:

Answer:



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
