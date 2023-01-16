Question 1: Which cities and countries have the highest level of transaction revenues on the site?

SQL Queries:
SELECT city, country, "transactionRevenue"
	FROM all_sessions
	WHERE "transactionRevenue" is not NULL
	ORDER BY CAST("transactionRevenue" as numeric) DESC limit 1;

Answer: 
city	country	transactionRevenue
not available in demo dataset	United States	1015480000
![image](https://user-images.githubusercontent.com/89874908/212736791-5349cc02-b459-45dd-8bef-e981ea6c881c.png)

Question 2: 
What is the average number of products ordered from visitors in each city and country?

SQL Queries:
SELECT country, AVG(CAST(products."orderedQuantity" as numeric)) as AVGCOUNTRY
  FROM all_sessions
  JOIN products on all_sessions."productSKU" = products."SKU"
  GROPU BY country
  HAVING country is not NULL
  ORDER BY country;

SELECT country, city, AVG(CAST(products."orderedQuantity" as numeric)) as AVGCITY
  FROM all_sessions
  JOIN products on all_sessions."productSKU" = products."SKU"
  GROUP BY city, country
  ORDER BY country;

Answer:
country	avgcountry
	520.611111111111
Albania	510.333333333333
Algeria	720.5
Argentina	460.030303030303
Armenia	1351
Australia	520.915422885572
Austria	319.090909090909
Bahamas	498
Bahrain	226.333333333333


country	city	avgcity
		520.611111111111
Albania		510.333333333333
Algeria		720.5
Argentina	Rosario	433
Argentina	Buenos Aires	434.5
Argentina	Santa Fe	1932.5
Argentina		344.833333333333
Armenia		1351
Australia	Melbourne	659.375
Australia		550.492753623188


Question 3: 
Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?

SQL Queries:
SELECT city, country, split_part(all_sessions."v2ProductCategory", '/', 2) as categories, COUNT (*) as nCV FROM all_sessions
  GROUP BY city, country, categories
  ORDER BY nCV DESC;

Answer:
city	country	categories	ncv
	United States	Apparel	1454
	United States	Shop by Brand	960
Mountain View	United States	Apparel	416
	United States	Electronics	393
	United States	Accessories	289
	United States	Office	268
New York	United States	Apparel	253
	United States		233
	United States	Drinkware	218
	United Kingdom	Shop by Brand	212
	United States	Bags	202
San Francisco	United States	Apparel	170
Mountain View	United States	Electronics	140
Sunnyvale	United States	Apparel	136

Question 4: 
 What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?
 
SQL Queries:
WITH tbl as (SELECT DISTINCT country, SUM(CAST(products."orderedQuantity" as numeric)) as qty, products."name" FROM all_sessions join products on all_sessions."productSKU" = products.'SKU' GROUP BY country, products."name" ORDER BY qty DESC) SELECT country, qty, X."name" from (SELECT *, row_number()over(partition by tbl."country" ORDER BY tbl."qty" desc) rn from tbl)X where rn = 1;

Answer:
country	qty	name
	3786	 Custom Decals
Albania	1465	22 oz  Bottle Infuser
Algeria	1429	 Twill Cap
Argentina	3682	SPF-15 Slim & Slender Lip Balm
Armenia	1351	Windup Android
Australia	18930	 Custom Decals
Austria	3786	 Custom Decals
Bahamas	917	 Twill Cap
Bahrain	528	 Men's 100% Cotton Short Sleeve Hero Tee White
Bangladesh	1870	 RFID Journal
Barbados	25	 Men's Pullover Hoodie Grey


Question 5: 
Can we summarize the impact of revenue generated from each city/country?

SQL Queries:
SELECT * FROM all_sessions WHERE all_sessions."transactionRevenue" is not NULL;

Answer:
"4.4E+18"	"Referral"	"0"	"United States"	"Sunnyvale"	"200000000"	"1"
"7.3E+18"	"Organic Search"	"651734"	"United States"	"not available in demo dataset"	"169970000"	"1"
"1.48E+18"	"Organic Search"	"257532"	"United States"	"not available in demo dataset"	"1015480000"	"1"
"3.44E+18"	"Direct"	"273057"	"United States"	"not available in demo dataset"	"1005500000"	"1"
