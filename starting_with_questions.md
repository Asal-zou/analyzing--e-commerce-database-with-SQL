Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

```
SQL Queries:
SELECT country, 
        city, 
        SUM(revenue::numeric) 
        AS total_revenue
FROM Analytics
      JOIN all_sessions
      ON Analytics.visitid = all_sessions.visitid
          where (revenue::numeric) >0 and city <> 'nocity' and country <> 'nocountry'
           GROUP BY country, city
           ORDER BY SUM(revenue::numeric) DESC;
```


Answer: united sates(city,New York,Sunnyvale,Mountain View ,Seattle,San Francisco, Chicago,San Jose,Palo Alto, Israel(Tel Avivyafo) and switzerland(zurich)





**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```
SELECT country, city, AVG(unit_sold::numeric) AS average_units_ordered
FROM Analytics
JOIN all_sessions ON Analytics.visitid = all_sessions.visitid
where (unit_sold::numeric) >0  and city <> 'nocity' and country <> 'nocountry'
GROUP BY country, city
order by AVG(unit_sold::numeric) desc ;
```


Answer:
-- 32 ROWS 
``
| Country       | City          | Average Units Ordered |
|---------------|---------------|-----------------------|
| United States | Chicago       | 5.00                  |
| United States | Pittsburgh    | 4.00                  |
| United States | New York      | 3.82                  |
| United States | Houston       | 2.50                  |
| Canada        | Toronto       | 2.33                  |
| United States | Sunnyvale     | 2.26                  |
| United States | Mountain View | 1.56                  |
| India         | Hyderabad     | 1.50                  |
| United States | Seattle       | 1.44                  |
| United States | San Francisco | 1.36                  |






**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```
SELECT city, country, v2productcategory, COUNT(v2productcategory) AS total_orders
FROM all_sessions
where  v2productcategory is not null and city <> 'nocity' and country <> 'nocountry'
GROUP BY city, country, v2productcategory
ORDER BY COUNT(v2productcategory) DESC
limit 10;
```



Answer:

| city           | country        | v2productcategory                     | total_orders |
|----------------|----------------|---------------------------------------|--------------|
| Mountain View  | United States  | Apparel Men's Men's-T-Shirts          | 110          |
| Mountain View  | United States  | Nest Nest-USA                         | 103          |
| Mountain View  | United States  | Electronics                           | 83           |
| Mountain View  | United States  | Shop by Brand Google                  | 80           |
| New York       | United States  | Apparel Men's Men's-T-Shirts          | 77           |
| Mountain View  | United States  | Apparel Men's Men's-Outerwear         | 68           |
| Mountain View  | United States  | Office                                | 64           |
| London         | United Kingdom | Shop by Brand YouTube                 | 61           |
| Mountain View  | United States  | nocategory                            | 49           |
| New York       | United States  | Electronics                           | 48           |






**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```
SELECT country, city, productsku, MAX(productquantity::numeric) as TopSelling
FROM all_sessions
where  productquantity is not null and city <> 'nocity' and country <> 'nocountry'
GROUP BY country, city, productsku
ORDER BY MAX(productquantity::numeric) DESC
```



Answer: 30 rows ,the unitedstates is the biggest buyer 

| Country       | City       | Product SKU      | Top Selling |
|---------------|------------|------------------|-------------|
| Spain         | Madrid     | GGOEWAEA083899   | 10          |
| United States | Salem      | GGOEGOCT019199   | 8           |
| United States | Atlanta    | GGOEGBJR018199   | 4           |
| United States | Houston    | GGOEGAAX0037     | 2           |
| United States | New York   | GGOENEBQ079199   | 2           |
| United States | Columbus   | GGOEGAAJ032617   | 1           |
| United States | Chicago    | GGOEGHGR019499   | 1           |
| United States | Detroit    | GGOEGDHC018299   | 1           |
| United States | Dallas     | GGOEYOLR018699   | 1           |
| United States | Ann Arbor  | GGOEGAAX0338     | 1           |








**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```
SELECT country, city, SUM(COALESCE(revenue::numeric, 0) + (productquantity::numeric * unit_price::numeric)) as Estimated_Revenue
FROM Analytics
JOIN all_sessions ON Analytics.visitid = all_sessions.visitid
where and city <> 'nocity' and country <> 'nocountry' 
GROUP BY country, city
ORDER BY Estimated_Revenue DESC;


Answer:
| Country       | City          | Estimated Revenue |
|---------------|---------------|-------------------|
| United States | Seattle       | 11,814,004,889.00 |
| United States | Mountain View | 135,920,218.92    |
| United States | New York      | 3,480.00          |
| United States | Sunnyvale     | 2,179.00          |
| Ireland       | Dublin        | 1,465.60          |
| United States | Dallas        | 821.04            |
| United States | Ann Arbor     | 385.82            |
| United States | Detroit       | 338.74            |
| United States | Houston       | 14.00             |








