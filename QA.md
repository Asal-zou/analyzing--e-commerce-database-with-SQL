What are your risk areas? Identify and describe them.
**Data Quality and Integrity**



QA Process:
Describe your QA process and include the SQL queries used to execute it.


**Checking for nulls for visitors id and fullvisitorsid at all_sessions, analytics**
```
 select fullvisitorid
 from public.all_sessions
      where fullvisitorid is null 
```

**The duplications in analytics and all_sessions**
 --checking for dupplication
```
select visited,
      fullvisitorid,
      count(*) 
from all_sessions
group by visitid , fullvisitorid
having count(*) >1
order by count(*) desc ;
```
```
SELECT *
FROM analytics
WHERE visitid = '1501560158' AND fullvisitorid = '1692871892756005610'
ORDER BY unit_price;
SELECT *
```
**choosing one of the Visitid and fullvisitor id to see if we can delete the duplications** 
```
SELECT *
from analytics 
WHERE visitid = '1501560158'
AND fullvisitorid = '1692871892756005610' 
having unit_sold::integer > 1
ORDER BY date;
```
If the visitid is eunique for the fullvisitid
```
SELECT *
from analytics 
WHERE visitid <> '1501560158'
AND fullvisitorid = '1692871892756005610' 
having unit_sold::integer > 1
ORDER BY date;
```
The change in the number of row return 
```
select distinct *
from all_sessions
where unit_sold::integer > 0
```
**being unable to make a primary key for all_session due to duplications**

**Missing or null values in critical field in all_session tables**
```
SELECT COUNT(*) AS null_transaction_revenue
FROM all_sessions
```
```
SELECT fullvisitorid, visitid, COUNT(*)
FROM all_sessions
GROUP BY fullvisitorid, visitid
HAVING COUNT(*) > 1
WHERE transactionrevenue IS NULL;
```
**can't define a relationship between all_session and products table using productsku and sku due to having productsku's that dosent exists in product sku**

**checked to see if the values in the sales_by_sku is exacly the same as sales_report**
```
SELECT COUNT(*) FROM sales_by_sku ;
SELECT COUNT(*) FROM sales_report;
```
checking if every record exists in sales_report 
```
SELECT COUNT(*)
FROM sales_by_sku sbk
JOIN sales_report sr
ON sbk.productsku = sr.productsku
```
Checking for productssku that exists in sales_by_sku and is null in sales_report
```
SELECT sbk.*
FROM sales_by_sku sbk
LEFT JOIN sales_report sr
ON sbk.productsku = sr.productsku 
WHERE sr.productsku IS NULL;
```
chechinkg if there is an order by the produtsku in sales_by_sku 
```
SELECT *
FROM sales_by_sku
where productsku= '9180753'
and productsku= '9184677' 
and productsku= '9184663' 
and productsku= '9182763'
and productsku= '9182779' 
and productsku= '9182182'
```
values are 0, it's safe to only use sales_report.
