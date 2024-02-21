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




Question 3: What is the score for the least ordered products?


SQL Queries:
```
SELECT p.name, p.sentimentscore::numeric
FROM products p
JOIN sales_report sr ON p.sku = sr.productsku
ORDER BY (sr.total_ordered:: numeric) ASC
LIMIT 5;
```

Answer:
| Name                                        | Sentiment Score |
|---------------------------------------------|-----------------|
| Women's Scoop Neck Tee Black                | 0.0             |
| Women's Short Sleeve Tri-blend Badge Tee Grey | 0.0           |
| Ballpoint Stylus Pen                        | 0.3             |
| Women's Fleece Hoodie Black                 | 0.3             |
| Women's Colorblock Tee White                | 0.8             |




Question 4: Which 5 product categories have the highest browsing time?

SQL Queries:
```
SELECT a.v2productcategory, AVG(a.timeonsite::numeric) AS average_time
FROM products p
JOIN all_sessions a ON p.sku = a.productsku
where timeonsite  is not null
GROUP BY a.v2productcategory
ORDER BY average_time 
limit 5 ;
```


Answer:
| v2productcategory | Average Time |
|-------------------|--------------|
| Office            | 993.00       |
| Apparel           | 899.13       |
| Housewares        | 800.00       |
| Clearance Sale    | 655.56       |
| Lifestyle         | 469.50       |




Question 5: 

SQL Queries:

Answer:
