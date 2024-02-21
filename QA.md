What are your risk areas? Identify and describe them.
**Data Quality and Integrity**



QA Process:
Describe your QA process and include the SQL queries used to execute it.
**checking and understanding the duplications in analytics and all_sessions**
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
from analytics 
WHERE visitid <> '1501560158' AND fullvisitorid = '1692871892756005610' 
having unit_sold::integer > 1
ORDER BY date;
select distinct *
from all_sessions
where unit_sold::integer > 0
```
Seeing if visitid is necceserly conected to fullvisitorid
```
from analytics 
WHERE visitid <> '1501560158' AND fullvisitorid = '1692871892756005610' 
having unit_sold::integer > 1
ORDER BY date;
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


