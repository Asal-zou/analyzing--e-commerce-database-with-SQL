What issues will you address by cleaning the data?

Duplicates , Nulls, 0es, format(text, string), correct way of writing 



Queries:
Below, provide the SQL queries you used to clean your data.

**changing product price format to numeric and a cleaner look, replacing Null and updating them**
```
UPDATE public.all_sessions
SET productprice = CASE
    WHEN productprice is null THEN '0'
    ELSE productprice
END;
```

```
UPDATE public.all_sessions
set productprice=  CAST(ROUND(CAST(REPLACE(productprice, ',', '') AS NUMERIC)/1000000, 2) AS NUMERIC(12,2))
```



**changing ptotal_ordered format to numeric and a cleaner look, looking for nulls, making sure it just contains digits** 

  ```
UPDATE public.sales_report
 set total_ordered = CASE 
                           WHEN cast(total_ordered as Numeric) in null THEN 0
                           ELSE total_ordered 
                     END 
  WHERE 
      total_ordered ~ '^\d+$';
```
```
ALTER TABLE public.sales_report
ALTER COLUMN total_ordered TYPE NUMERIC
USING CAST (total_ordered AS NUMERIC)
```





**Making sure that city and country have the correct formatting and updated table**
(used internent for this part ('[^a-zA-Z\s]', '', 'g'))

``` UPDATE public.all_sessions
        SET
             city = REGEXP_REPLACE(city, '[^a-zA-Z\s]', '', 'g'),
             country = REGEXP_REPLACE(country, '[^a-zA-Z\s]', '', 'g')
```

**changing "not set" and 'not available in demo dataset' for city and country used (chatgpt for where part)**

```
UPDATE public.all_sessions
SET country = CASE 
                    WHEN country = 'not set' THEN 'nocountry'
                    WHEN country = 'not available in demo dataset' THEN 'nocountry'
                    ELSE country
                END
   WHERE country
       IN ('not set', 'not available in demo dataset');
```

**checked for nulls for visitors id  at all_sessions, analytics**
```
 select fullvisitorid
 from public.all_sessions
      where fullvisitorid is null 
```

 
**taking out "Home" from all categories**  
```
UPDATE public.all_sessions
SET v2productcategory = REPLACE(v2productcategory, 'Home', ' ');
```

**replacing "/" with ''.**

```
UPDATE all_sessions
SET v2productcategory = REPLACE(v2productcategory, '/', '   ');
```

**making sure all words are in the correct setting (used chat gpt for  ([a-z])([A-Z])', '\1 \2', 'g'))**
 
```
 UPDATE all_sessions
        SET v2productcategory= REGEXP_REPLACE(v2productcategory, '([a-z])([A-Z])', '\1 \2', 'g');
```

**replace all the (not set) to nocategory**

```
UPDATE public.all_sessions
SET v2productcategory = CASE 
                             WHEN v2productcategory = '(not set)' THEN 'nocategory'
                             ELSE v2productcategory
                         END
``` 

**making sure all currency is in USD**
```
SELECT currencycode
FROM  public.all_sessions
      where currencycode <> 'USD'
```

**dividing the unit_price/ 1000.000 and making sure that it only has 3 decimals afterwards** 
```
begin;

Update public.analytics 
set unit_price= to_char((unit_price:: numeric/ 1000000), '999G999D999') ;

select unit_price from analytics 

commit;
```

```
ALTER TABLE public.analytics 
ALTER COLUMN unit_price TYPE NUMERIC
USING CAST (unit_price AS NUMERIC)
```


**finding how many full visitors Id's are, minding duplicates**
```
select count(distinct fullVisitoriD)
from all_sessions 
```

**finding how many  visit Id's are, minding duplicates**
```
select count(distinct visitiD)
from analytics
```

**checking for nulls for productsku in sales_report and sales_by_sku, and sku in products.checking for duplicates in second code**

```
select productsku 
from sales_report
    where productsku = '0'and
          productsku is null ;
```
```
select count( distinct sku) from public.products;
```


**assuming that the columns userid, revenue , unitsold and time on site have only nulls.**

```
SELECT  Count(*) from analytics 
where userid is not null
```
**user-id returned with a "0" result, so double-checked and removed the column.**
```
SELECT SUM(CAST(userid AS numeric)) FROM analytics;
```
```
ALTER TABLE analytics DROP COLUMN userid;
```
**checking and understanding the duplications in analytics and all_sessions**

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
```
```
SELECT *
from analytics 
WHERE visitid <> '1501560158' AND fullvisitorid = '1692871892756005610' 
having unit_sold::integer > 1
ORDER BY date;
```

```
select distinct *
from all_sessions
where unit_sold::integer > 0
```

**Building primary key for products table using sku**
```
ALTER TABLE products
ADD CONSTRAINT pk_products_sku PRIMARY KEY (sku);
ALTER TABLE sales_report
```
**defining Productssku as a foreign key for sales_report**
```
ADD CONSTRAINT fk_sales_report_productsku FOREIGN KEY (productsku)
REFERENCES products (sku);
```





