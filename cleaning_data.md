What issues will you address by cleaning the data?

Duplicates , Nulls, 0es, format(text, string), correct way of writing 



Queries:
Below, provide the SQL queries you used to clean your data.

--  changing product price format to numeric and a cleaner look, replacing Null and updating them 

```
UPDATE public.all_sessions
 SET productprice = CASE 
    WHEN productprice IS NULL OR productprice:: Numeric = 0 THEN 'noprice'
    ELSE TO_CHAR(productprice::numeric, '999G999G999')
  END 
```


--    changing ptotal_ordered format to numeric and a cleaner look, looking for nulls, making sure it just contains digits 

  ```
UPDATE public.sales_report
 set total_ordered = CASE 
                           WHEN cast(total_ordered as Numeric)  = 0 THEN 'noorder'
                           ELSE total_ordered 
                     END 
  WHERE 
      total_ordered ~ '^\d+$';
```

--  Making sure that city and country have the correct formatting and updated table
(used internent for this part ('[^a-zA-Z\s]', '', 'g'))

``` UPDATE public.all_sessions
        SET
             city = REGEXP_REPLACE(city, '[^a-zA-Z\s]', '', 'g'),
             country = REGEXP_REPLACE(country, '[^a-zA-Z\s]', '', 'g')
```

-- changing "not set" and 'not available in demo dataset' for city and country used (chatgpt for where part)

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

checked for nulls for visitors id  at all_sessions, analytics
```
 select fullvisitorid
 from public.all_sessions
      where fullvisitorid is null 
```

 
--  taking out "Home" from all categories  
```
UPDATE public.all_sessions
SET v2productcategory = REPLACE(v2productcategory, 'Home', ' ');
```

-- replacing "/" with ''

```
UPDATE all_sessions
SET v2productcategory = REPLACE(v2productcategory, '/', '   ');
```

--  making sure all words are in the correct setting (used chat gpt for  ([a-z])([A-Z])', '\1 \2', 'g'))
 
```
 UPDATE all_sessions
        SET v2productcategory= REGEXP_REPLACE(v2productcategory, '([a-z])([A-Z])', '\1 \2', 'g');
```

- replace all the (not set) to nocategory'

```
UPDATE public.all_sessions
SET v2productcategory = CASE 
                             WHEN v2productcategory = '(not set)' THEN 'nocategory'
                             ELSE v2productcategory
                         END
``` 

-- making sure all currency is in USD
```
SELECT currencycode
FROM  public.all_sessions
      where currencycode <> 'USD'
```

-- dividing the unit_price/ 1000.000 and making sure that it only has 3 decimals afterwards 
```
begin;

Update public.analytics 
set unit_price= to_char((unit_price:: numeric/ 1000000), '999G999D999') ;

select unit_price from analytics 

commit;
```

-- finding how many full visitors Id's are, minding duplicates 
```
select count(distinct fullVisitoriD)
from all_sessions 
```

-- finding how many  visit Id's are, minding duplicates 
```
select count(distinct visitiD)
from analytics
```



