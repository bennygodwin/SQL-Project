What are your risk areas? Identify and describe them.
  There are a lot of missing values

In "all_sessions.csv" following attributes did not have any values:
  totalTransactionRevenue
  transactions
  productRefundAmount
  productQuantity
  productRevenue
  itemQuantity
  itemRevenue
  searchKeyword

In "analytics.csv" following attributes did not have any values:
  userid
  units_sold
  revenue

QA Process:
Describe your QA process and include the SQL queries used to execute it.
SELECT DISTINCT analytics."fullvisitorId" FROM analytics JOIN all_sessions on analytics."fullvisitorId" = all_sessions."fullVisitorId";

SELECT COUNT(DISTINCT analytics."fullvisitorId") FROM analytics JOIN all_sessions on analytics."fullvisitorId" = all_sessions."fullVisitorId";

SELECT country, AVG(CAST(products."orderedQuantity" as numeric)) as AVGCOUNTRY from all_sessions JOIN products on all_sessions."productSKU" = products."SKU" GROUP BY country HAVING country is not NULL ORDER BY country;

SELECT country, city, AVG(CAST(products."orderedQuantity" as numeric)) as AVGCITY FROM all_sessions JOIN products on all_sessions."productSKU" = products."SKU" GROUP BY city, country ORDER BY country;

![image](https://user-images.githubusercontent.com/89874908/212741343-6c418957-7373-4e6d-a482-72d9bba56c63.png)
