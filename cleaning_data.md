What issues will you address by cleaning the data?





Queries:
Below, provide the SQL queries you used to clean your data.

--  changing product price format to numeric and a cleaner look 

````select To_char(productprice :: Numeric , '999G999G999') from public.all_sessions;````

-- looking for Nulls
````select To_char(productprice :: Numeric , '999G999G999') as productprice
from public.all_sessions
     where productprice= '0';````
	 	 
-- replacing Null 
````SELECT 
  CASE 
    WHEN productprice IS NULL OR productprice:: Numeric = 0 THEN 'noprice'
    ELSE TO_CHAR(productprice::numeric, '999G999G999')
  END AS productprice
FROM 
  public.all_sessions;````


--    changing ptotal_ordered format to numeric and a cleaner look, looking for nulls, making sure it just contains digits 

  ````SELECT 
  CASE 
    WHEN cast(total_ordered as Numeric)  = 0 THEN 'noorder'
    ELSE total_ordered 
  END AS total_ordered 
FROM public.sales_report
WHERE 
  total_ordered ~ '^\d+$';````
