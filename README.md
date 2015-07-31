##SQL: Learn by query

Complete the following queries and paste the sql results under each question. Use the [wc3 school’s try sql](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all) tool to complete them.


1. Select all the customers whose first name starts with “L” and is from Germany

SELECT * FROM Customers
WHERE CustomerName LIKE "L%"
AND Country="Germany";

Number of Records: 1

CustomerID	CustomerName	ContactName	Address	City	PostalCode	Country
44	Lehmanns Marktstand Renate Messner	Magazinweg 7	Frankfurt a.M.	60528	Germany


2. Select all the product names that are in the “Condiments” category

SELECT ProductName
FROM Products
INNER JOIN Categories
ON Products.CategoryID=Categories.CategoryID
WHERE CategoryName="Condiments";

Number of Records: 12
ProductName
Aniseed Syrup
Chef Anton's Cajun Seasoning
Chef Anton's Gumbo Mix
Grandma's Boysenberry Spread
Northwoods Cranberry Sauce
Genen Shouyu
Gula Malacca
Sirop d'érable
Vegie-spread
Louisiana Fiery Hot Pepper Sauce
Louisiana Hot Spiced Okra
Original Frankfurter grüne Soße

3. Select the customer names who have never had an order

SELECT CustomerName, OrderID
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
WHERE Orders.CustomerID IS NULL

Number of Records: 17
CustomerName	OrderID
Alfreds Futterkiste	null
Blauer See Delikatessen	null
Cactus Comidas para llevar	null
FISSA Fabrica Inter. Salchichas S.A.	null
France restauration	null
Great Lakes Food Market	null
La corne d'abondance	null
Laughing Bacchus Wine Cellars	null
Lazy K Kountry Store	null
Let's Stop N Shop	null
Maison Dewey	null
North/South	null
Paris spécialités	null
Rancho grande	null
Spécialités du monde	null
The Cracker Box	null
Trail's Head Gourmet Provisioners	null

4. Select the list of customer_ids, and the number or orders, sorted descending

SELECT Customers.CustomerID, COUNT(Orders.OrderID) as [Total Count] FROM Customers
LEFT JOIN Orders
ON Orders.CustomerID = Customers.CustomerID
Group By Customers.CustomerID
ORDER BY [Total Count] Desc;

Number of Records: 91
CustomerID	Total Count
20	10
63	7
65	7
87	7
37	6
75	6
41	5
46	5
51	5
7	4
10	4
24	4
25	4
55	4
61	4
71	4
80	4
86	4
5	3
9	3
21	3
36	3
38	3
44	3
49	3
59	3
60	3
66	3
69	3
72	3
4	2
14	2
17	2
19	2
28	2
29	2
34	2
35	2
39	2
48	2
58	2
62	2
67	2
68	2
73	2
76	2
81	2
83	2
84	2
85	2
88	2
89	2
2	1
3	1
8	1
11	1
13	1
15	1
16	1
18	1
23	1
27	1
30	1
31	1
33	1
47	1
52	1
54	1
56	1
70	1
77	1
79	1
90	1
91	1
1	0
6	0
12	0
22	0
26	0
32	0
40	0
42	0
43	0
45	0
50	0
53	0
57	0
64	0
74	0
78	0
82	0

5. Select the SUM of all orders in 1997

SELECT Count(OrderID) FROM Orders
where EXTRACT(YEAR from OrderDate)='1997'

We were unable to run this query because the tool keeps throwing a syntax error. We tried running the example query provided on the Extract apge and got the same syntax error.

6. Select the name of the shipper, and the total number of orders shipped

Select Shippers.ShipperName, COUNT(Orders.OrderID) as [Total Count]
FROM Orders
JOIN Shippers
ON Shippers.ShipperId = Orders.ShipperID
Group By ShipperName
ORDER BY [Total Count] Desc;

Number of Records: 3
ShipperName	Total Count
United Package	74
Federal Shipping	68
Speedy Express	54

7. Select the ID and amount of the most expensive order

SELECT ID, MAX(TotalPrice)
FROM (
SELECT
Orders.OrderID as ID, SUM(OrderDetails.Quantity * Products.Price) as TotalPrice
FROM Orders
INNER JOIN OrderDetails
ON Orders.OrderID=OrderDetails.OrderID
INNER JOIN Products
ON Products.ProductID = OrderDetails.ProductID
Group by OrderDetails.OrderID
ORDER BY TotalPrice Desc);

Number of Records: 1
ID	MAX(TotalPrice)
10372	15353.6


8. Select the names of the employees that have shipped Tofu

We couldn't get this to work with concatenating first and last name. :(
SELECT Employees.FirstName+' '+Employees.LastName AS Name
FROM Employees
JOIN Orders
ON Employees.EmployeeID=Orders.EmployeeID
JOIN OrderDetails
ON Orders.OrderID=OrderDetails.OrderID
JOIN Products
ON Products.ProductID=OrderDetails.ProductID
WHERE Products.ProductName="Tofu"

We got it to work when we selected the first and last columns separately
SELECT FirstName, LastName
FROM Employees
JOIN Orders
ON Employees.EmployeeID=Orders.EmployeeID
JOIN OrderDetails
ON Orders.OrderID=OrderDetails.OrderID
JOIN Products
ON Products.ProductID=OrderDetails.ProductID
WHERE Products.ProductName="Tofu"

Number of Records: 8
FirstName	LastName
Michael	Suyama
Nancy	Davolio
Steven	Buchanan
Janet	Leverling
Nancy	Davolio
Janet	Leverling
Laura	Callahan
Margaret	Peacock

9. Select the name of the customer with the most expensive order

SELECT Customers.CustomerName as Name
FROM Customers
JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
INNER JOIN OrderDetails
ON Orders.OrderID=OrderDetails.OrderID
INNER JOIN Products
ON Products.ProductID = OrderDetails.ProductID
Group by OrderDetails.OrderID
ORDER BY SUM(OrderDetails.Quantity * Products.Price) Desc
LIMIT 1;

Number of Records: 1
Name
Queen Cozinha


10. Select all the product names that are in the most expensive order

SELECT Products.ProductName FROM Products
INNER JOIN OrderDetails
ON Products.ProductID=OrderDetails.ProductID
WHERE OrderDetails.OrderID=(SELECT OrderDetails.OrderID
FROM OrderDetails
INNER JOIN Products
ON Products.ProductID = OrderDetails.ProductID
Group by OrderDetails.OrderID
ORDER BY SUM(OrderDetails.Quantity * Products.Price) Desc
LIMIT 1);

Number of Records: 4
ProductName
Sir Rodney's Marmalade
Côte de Blaye
Camembert Pierrot
Mozzarella di Giovanni

11. Select the name of the employee with the least amount of orders

SELECT Employees.FirstName, Employees.LastName, COUNT(Orders.OrderID) AS TotalOrders
FROM Employees
LEFT JOIN Orders
ON Employees.EmployeeID=Orders.EmployeeID
GROUP BY LastName
ORDER BY TotalOrders
LIMIT 1;

Number of Records: 1
FirstName	LastName	TotalOrders
Adam	West	0

12. Select the number of orders made in each month sorted by the number of orders

Because Extract function's not working, we were unable to vaildate this one, but we think this is what it should be. Is Group By what we were missing before?

SELECT EXTRACT(MONTH FROM OrderDate) AS OrderMonth, COUNT(Orders.OrderID)
FROM Orders
GROUP BY OrderMonth
ORDER BY COUNT Desc;

13. Select the countries and the number of orders from each country sorted descending

SELECT Country, COUNT(Orders.OrderID) AS TotalOrders
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
GROUP BY Country
ORDER BY TotalOrders Desc;

Number of Records: 21
Country	TotalOrders
USA	29
Germany	25
Brazil	19
France	18
Austria	13
UK	12
Canada	9
Mexico	9
Venezuela	9
Finland	8
Italy	7
Spain	7
Sweden	7
Ireland	6
Portugal	5
Denmark	4
Switzerland	4
Belgium	2
Argentina	1
Norway	1
Poland	1

14. Select the country that has the most products

SELECT Country
FROM Suppliers
INNER JOIN Products
ON Suppliers.SupplierID=Products.SupplierID
GROUP BY Country
ORDER BY COUNT(Products.ProductID) Desc
LIMIT 1;

Number of Records: 1
Country
USA

15. Select the best selling product, the total number of orders, and gross revenue

SELECT Products.ProductName AS Product, COUNT(OrderDetails.OrderID) AS Orders, SUM(OrderDetails.Quantity)*Products.Price AS GrossRevenue
FROM Products
INNER JOIN OrderDetails
ON Products.ProductID=OrderDetails.ProductID
GROUP BY Products.ProductName
ORDER BY Orders Desc
LIMIT 1;

Number of Records: 1
Product	Orders	GrossRevenue
Gorgonzola Telino	14	5725



