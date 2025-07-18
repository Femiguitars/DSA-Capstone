First thing was to input the two given csv files

📁 Files: KMS Sql Case Study.csv, Order_Status.csv

```
create database MY_Dsa_Project
SELECT * FROM [dbo].[KMS Sql Case Study];
SELECT * FROM [dbo].[Order_Status]
```

#### 1. which product category had the highest sales
```
SELECT 
    Product_Category,
    SUM(Sales) AS [Total Sales]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Product_Category
ORDER BY [Total Sales] DESC;
```
#### 2. Top and bottom 3 region in sales
```
SELECT TOP 3 
    Region,
    SUM(Sales) AS [Total Sales]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Region
ORDER BY [Total Sales] DESC;

SELECT TOP 3 
    Region,
    SUM(Sales) AS [Total Sales]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Region
ORDER BY [Total Sales] ASC;
```

#### 3. What were the total sales of appliances in Ontario?
```
SELECT 
    SUM(Sales) AS [Total Appliance Sales]
FROM [dbo].[KMS Sql Case Study]
WHERE 
    Product_sub_Category = 'Appliances'
    AND Province = 'Ontario';
```

#### 4. Advise the management of KMS on what to do to increase the revenue from the bottom 
10 customers

##### 4.1. Bottom 10 Customers by Total Sales
```
SELECT TOP 10 
    Customer_Name,
    SUM(Sales) AS [Total Sales],
    SUM(Profit) AS [Total Profit],
    COUNT(*) AS [Order Count]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Customer_Name
ORDER BY [Total Sales] ASC;
```

##### 4.2. Which segments they belong
```
SELECT 
    Customer_Segment, 
    COUNT(DISTINCT Customer_Name) AS [Total Customers]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Customer_Segment
ORDER BY [Total Customers] ASC;
```

##### 4.3. What products they tend to buy

```
SELECT Customer_Name, Product_Category, SUM(Sales) AS [Total Sales]
FROM [dbo].[KMS Sql Case Study]
WHERE Customer_Name IN (
    SELECT TOP 10 Customer_Name
    FROM [dbo].[KMS Sql Case Study]
    GROUP BY Customer_Name
    ORDER BY SUM(Sales) ASC  
)
GROUP BY Customer_Name, Product_Category
ORDER BY Customer_Name;
```

##### 4.4. Which regions or shipping modes they use
```
SELECT Customer_Name, Region, Ship_Mode, COUNT(*) AS OrderCount
FROM [dbo].[KMS Sql Case Study]
WHERE Customer_Name IN (
    SELECT TOP 10 Customer_Name
    FROM [dbo].[KMS Sql Case Study]
    GROUP BY Customer_Name
    ORDER BY SUM(Sales) ASC 
)
GROUP BY Customer_Name, Region, Ship_Mode;
```

```
After analyzing the sales data, the bottom 10 customers 
have low revenue contributions and limited order frequency.
Most of them belong to the Small Business, consumer,  Home office segments and tend to buy Furnitures, office supplies, Technology e.t.c..

To increase revenue from these customers, KMS should:
1. Implement targeted promotions based on their purchase history.

2. Offer personalized product bundles to encourage larger order sizes.

3. Introduce loyalty incentives for repeat orders.

4. Use cost-effective shipping incentives to improve margins.

These steps can help re-engage these low-performing customers and convert them into higher-value clients.******


#### 5. KMS incurred the most shipping cost using which shipping method?
```
SELECT 
    Ship_Mode,
    SUM(Shipping_Cost) AS [Total Shipping Cost]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Ship_Mode
ORDER BY [Total Shipping Cost] DESC;
```

#### 6. Who are the most valuable customers, and what products or services do they typically purchase?
```
SELECT TOP 10 
    Customer_Name,
    SUM(Sales) AS [Total Sales],
    SUM(Profit) AS [Total Profit],
    COUNT(*) AS [Order Count]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Customer_Name
ORDER BY [Total Sales] DESC;
```

#### 7. Which small business customer had the highest sales? 
```
SELECT TOP 1 
    Customer_Name,
    SUM(Sales) AS [Total Sales]
FROM [dbo].[KMS Sql Case Study]
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name
ORDER BY [Total Sales] DESC;
```

#### 8. Which Corporate Customer placed the most number of orders in 2009 – 2012?
```
SELECT TOP 1 
    Customer_Name,
    COUNT(*) AS [Order Count]
FROM [dbo].[KMS Sql Case Study]
WHERE 
    Customer_Segment = 'Corporate' AND
    TRY_CAST(Order_Date AS DATE) BETWEEN '2009-01-01' AND '2012-12-31'
GROUP BY Customer_Name
ORDER BY [Order Count] DESC;
```

#### 9. Which consumer customer was the most profitable one? 
```
SELECT TOP 1 
    Customer_Name,
    SUM(Profit) AS [Total Profit]
FROM [dbo].[KMS Sql Case Study]
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY [Total Profit] DESC;
```

#### 10. Which customer returned items, and what segment do they belong to? 
```
SELECT 
    [dbo].[KMS Sql Case Study].*,
    [dbo].[Order_Status].[Status]
FROM [dbo].[KMS Sql Case Study]
LEFT JOIN [dbo].[Order_Status]
    ON [dbo].[KMS Sql Case Study].[Order_ID] = [dbo].[Order_Status].[Order_ID];
```
----------------------------------------------------------------------------
```
SELECT 
    Customer_Name,
    Customer_Segment,
    COUNT(*) AS [Count_of_segment_return]
FROM [dbo].[KMS Sql Case Study]
LEFT JOIN [dbo].[Order_Status]
    ON [dbo].[KMS Sql Case Study].[Order_ID] = [dbo].[Order_Status].[Order_ID]
WHERE [Order_Status].[Status] = 'Returned'
GROUP BY Customer_Name, Customer_Segment
ORDER BY Count_of_segment_return DESC;
```

#### 11.If the delivery truck is the most economical but the slowest shipping method and 
Express Air is the fastest but the most expensive one, do you think the company 
appropriately spent shipping costs based on the Order Priority? Explain your answer 

```
SELECT 
    Order_Priority,
    Ship_Mode,
    COUNT(*) AS [Order Count],
    SUM(Shipping_Cost) AS Total_Shipping_Cost
FROM [dbo].[KMS Sql Case Study]
GROUP BY Order_Priority, Ship_Mode
ORDER BY Order_Priority, Total_Shipping_Cost DESC;
```
********* **************
```
SELECT 
    Ship_Mode,
    COUNT(*) AS Total_Order_Count,
    SUM(Shipping_Cost) AS Total_Shipping_Cost
FROM 
    [dbo].[KMS Sql Case Study]
GROUP BY 
    Ship_Mode
ORDER BY 
    Total_Shipping_Cost DESC;
```
