Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
>select city, country, "transactionRevenue" from all_sessions where "transactionRevenue" is not NULL order by CAST("transactionRevenue" as numeric) desc limit 1;



Answer:
NULL    "United States" "1005500000"



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
> select country, AVG(CAST(products."orderedQuantity" as numeric)) as AVGCOUNTRY from all_sessions join products on all_sessions."productSKU" = products."SKU" group by country having country is not NULL order by country;

> select country, city, AVG(CAST(products."orderedQuantity" as numeric)) as AVGCITY from all_sessions join products on all_sessions."productSKU" = products."SKU" group by city, country order by country;


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
Bangladesh	242.741935483871
Barbados	25

and

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
Australia	Sydney	396.090909090909
Australia	Los Angeles	917
Australia	Adelaide	54






**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
>select city, country, split_part(all_sessions."v2ProductCategory", '/', 2) as categories, count(*) as nCV from all_sessions group by city, country, categories order by nCV desc;


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



**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
>with tbl  as (select DISTINCT country, SUM(CAST(products."orderedQuantity" as numeric)) as qty, products."name" from all_sessions join products on all_sessions."productSKU" = products."SKU" group by country, products."name" order by qty desc) select country, qty, X."name" from (select *, row_number()over(partition by tbl."country" order by tbl."qty" desc) rn from tbl)X where rn = 1;



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




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
>select * from all_sessions where all_sessions."transactionRevenue" is not NULL;


Answer:
fullVisitorId	channelGrouping	time	country	city	totalTransactionRevenue	transactions	timeOnSite	pageviews	sessionQualityDim	date	visitId	type	productRefundAmount	productQuantity	productPrice	productRevenue	productSKU	v2ProductName	v2ProductCategory	productVariant	currencyCode	itemQuantity	itemRevenue	transactionRevenue	transactionId	pageTitle	searchKeyword	pagePathLevel1	eCommerceAction_type	eCommerceAction_step	eCommerceAction_option
4.39692706302377E+018	Referral	0	United States	Sunnyvale	200000000	1	83	6	NULL	20170320	1489992840	PAGE	NULL	1	119000000	120000000	GGOENEBB078899	Nest® Cam Indoor Security Camera - USA	Nest-USA	Single Option Only	USD	NULL	NULL	200000000	ORD201703201805	Checkout Confirmation	NULL	/ordercompleted.html	6	1	NULL
7.2984162134389E+018	Organic Search	651734	United States		169970000	1	652	21	NULL	20170405	1491424130	PAGE	NULL	1	55990000	58656666	GGOEGEVJ023999	Compact Bluetooth Speaker	Electronics	Single Option Only	USD	NULL	NULL	169970000	ORD201704052261	Checkout Confirmation	NULL	/ordercompleted.html	6	1	NULL
1.48031187712734E+018	Organic Search	257532	United States		1015480000	1	258	9	NULL	20161101	1478007873	PAGE	NULL	50	3500000	176400000	GGOEGBJR018199	Reusable Shopping Bag	Bags	Single Option Only	USD	NULL	NULL	1015480000	ORD201611011533	Checkout Confirmation	NULL	/ordercompleted.html	6	1	NULL
3.44464270601341E+018	Direct	273057	United States		1005500000	1	273	14	NULL	20161213	1481693637	PAGE	NULL	1	59990000	60365000	GGOEGEVB070899	Google Bongo Cupholder Bluetooth Speaker	Electronics	Single Option Only	USD	NULL	NULL	1005500000	ORD201612132887	Checkout Confirmation	NULL	/ordercompleted.html	6	1	NULL







