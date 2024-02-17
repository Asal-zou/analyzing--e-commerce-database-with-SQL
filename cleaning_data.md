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
  END AS formatted_productprice
FROM 
  public.all_sessions;````
