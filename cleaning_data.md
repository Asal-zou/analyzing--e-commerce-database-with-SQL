What issues will you address by cleaning the data?





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
            country = REGEXP_REPLACE(city, '[^a-zA-Z\s]', '', 'g')
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




