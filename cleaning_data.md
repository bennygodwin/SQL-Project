What issues will you address by cleaning the data?

----
- The unit cost in the data needs to be divided by 1,000,000.
- 
-

Queries:
Below, provide the SQL queries you used to clean your data.

----
The unit cost in the data needs to be divided by 1,000,000.
> UPDATE public.analytics SET unit_price = CAST(unit_price AS FLOAT) / 1000000;
- 
- 