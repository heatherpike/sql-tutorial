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

SELECT ProductName, CategoryName
FROM Products
INNER JOIN Categories
ON Products.CategoryID=Categories.CategoryID
WHERE CategoryName="Condiments";

Number of Records: 12

ProductName	CategoryName
Aniseed Syrup	Condiments
Chef Anton's Cajun Seasoning	Condiments
Chef Anton's Gumbo Mix	Condiments
Grandma's Boysenberry Spread	Condiments
Northwoods Cranberry Sauce	Condiments
Genen Shouyu	Condiments
Gula Malacca	Condiments
Sirop d'érable	Condiments
Vegie-spread	Condiments
Louisiana Fiery Hot Pepper Sauce	Condiments
Louisiana Hot Spiced Okra	Condiments
Original Frankfurter grüne Soße	Condiments

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

SELECT Count(OrderID) FROM [Orders]
where OrderDate like '1997-%'

Count(OrderID)
44

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

SELECT Name, MAX(TotalPrice)
FROM (
SELECT Customers.CustomerName as Name, Orders.OrderID as ID, SUM(OrderDetails.Quantity * Products.Price) as TotalPrice
FROM Customers
JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
INNER JOIN OrderDetails
ON Orders.OrderID=OrderDetails.OrderID
INNER JOIN Products
ON Products.ProductID = OrderDetails.ProductID
Group by OrderDetails.OrderID
ORDER BY TotalPrice Desc);

Number of Records: 1
Name	MAX(TotalPrice)
Queen Cozinha	15353.6


10. Select all the product names that are in the most expensive order

SELECT * FROM Products
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
ProductID	ProductName	SupplierID	CategoryID	Unit	Price	OrderDetailID	OrderID	Quantity
20	Sir Rodney's Marmalade	8	3	30 gift boxes	81	331	10372	12
38	Côte de Blaye	18	1	12 - 75 cl bottles	263.5	332	10372	40
60	Camembert Pierrot	28	4	15 - 300 g rounds	34	333	10372	70
72	Mozzarella di Giovanni	14	4	24 - 200 g pkgs.	34.8	334	10372	42

11. Select the name of the employee with the least amount of orders

SELECT Employees.FirstName, Employees.LastName, COUNT(Orders.OrderID) AS TotalOrders
FROM Employees
LEFT JOIN Orders
ON Employees.EmployeeID=Orders.EmployeeID
GROUP BY LastName
ORDER BY TotalOrders;

Number of Records: 10
FirstName	LastName	TotalOrders
Adam	West	0
Anne	Dodsworth	6
Steven	Buchanan	11
Robert	King	14
Michael	Suyama	18
Andrew	Fuller	20
Laura	Callahan	27
Nancy	Davolio	29
Janet	Leverling	31
Margaret	Peacock	40

12. Select the number of orders made in each month sorted by the number of orders

//This is not working!! :(

SELECT DATEPART(m,OrderDate) AS OrderMonth, COUNT(Orders.OrderID)
FROM Orders
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

SELECT Country, COUNT(Products.ProductName) AS TotalProducts
FROM Customers
INNER JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
INNER JOIN OrderDetails
ON Orders.OrderID=OrderDetails.OrderID
INNER JOIN Products
ON Products.ProductID=OrderDetails.ProductID
GROUP BY Country
ORDER BY TotalProducts Desc;

Number of Records: 21
Country	TotalProducts
USA	76
Germany	74
Brazil	55
France	44
Austria	39
UK	28
Canada	26
Venezuela	22
Ireland	21
Mexico	20
Spain	18
Sweden	18
Finland	15
Denmark	14
Italy	13
Switzerland	12
Portugal	9
Belgium	6
Norway	4
Argentina	2
Poland	2

15. Select the best selling product, the total number of orders, and gross revenue

SELECT Product, (TotalQty*Price) AS GrossRevenue, Orders, TotalQty
FROM
(SELECT Products.ProductName AS Product, Products.Price AS Price, COUNT(OrderDetails.OrderID) AS Orders, SUM(OrderDetails.Quantity) AS TotalQty
FROM Products
INNER JOIN OrderDetails
ON Products.ProductID=OrderDetails.ProductID
GROUP BY Products.ProductName
ORDER BY Orders Desc)
LIMIT 1;

Number of Records: 1
Product	GrossRevenue	Orders	TotalQty
Gorgonzola Telino	5725	14	458


