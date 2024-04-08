# Sales Data Analysis Project

## A deep dive into sales data, aimed at extracting valuable insights to enhance strategic decision-making.
In this project, I will delve into a substantial sales dataset to extract valuable insights. I will explore sales trends over time, identify the top-selling products, calculate essential revenue metrics like total sales and profit margins, and develop visualizations to effectively present my findings. This project demonstrates my proficiency in manipulating and deriving insights from extensive datasets, allowing me to provide data-driven recommendations for optimizing sales strategies.


This study will pass through the six phases of the data life cycle which are;

1. Ask
2. Prepare
3. Process
4. Analyze
5. Share
6. Act

# Ask


### **Business task**

To Analyze sales data to identify trends, top-selling products, and revenue metrics for business decision-making.



### **Key stakeholders**

MeriSkill


# Prepare

In this project, I'll delve into an extensive sales dataset to uncover valuable insights. I'll start by analyzing sales trends over time, aiming to understand how sales have evolved monthly or quarterly and identifying any seasonal patterns.   
This analysis will provide a clear view of the business's growth trajectory and areas for improvement.  
Next, I'll focus on identifying the top-selling products that have been driving revenue. By pinpointing these products, we can tailor marketing strategies, optimize inventory, and allocate resources effectively.   
Additionally, I'll calculate essential revenue metrics such as total sales and profit margins to assess the business's financial performance.  
To effectively communicate these insights, I'll utilize data visualization techniques. From line charts illustrating sales trends to pie charts showcasing product revenue shares, these visuals will make interpreting the data easier and presenting findings clearly.  
Ultimately, the project's goal is to provide actionable recommendations based on these data-driven insights. These recommendations may include adjusting pricing strategies, diversifying product offerings, or targeting specific customer segments.   
By leveraging the insights gained from this analysis, we can optimize sales strategies for business growth and success.  

# Process


For this analysis, I will be using the Sales data provided by Meriskil. 


It has the following Columns:

* ORDER ID
* PRODUCT
* QUANTITY ORDERED
* PRICE EACH
* ORDER DATE
* PURCHASE ADDRESS
* MONTH
* SALES
* CITY
* HOUR

## Actions taken on Excel:

Upon opening and reviewing the dataset, I gained a deeper understanding of the problem and potential solutions. 
To enhance data clarity and analysis, I split the "Order Date" column into separate "Order Date" and "Order Time" columns using the datetime formatting tool. This step allowed for a more granular examination of sales patterns.
Ensured columns are in the correct data types.
Subsequently, I meticulously checked the dataset for any duplicate entries and undertook necessary data-cleaning procedures. 
By ensuring data integrity, we can proceed with confidence in our analysis, drawing reliable insights to inform our business decisions.
After cleaning and organizing the dataset, I uploaded it to BigQuery for SQL-based analysis, aiming to extract insights on sales trends and top-selling products.

# Process

# Exploratory SQL Analysis(BigQuery):

## 1. Explore Sales Trends Over Time:

SQL query to analyze sales trends over time using the "ORDER DATE" column.

`SELECT
  CASE
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 1 THEN 'January'
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 2 THEN 'February'
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 3 THEN 'March'
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 4 THEN 'April'
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 5 THEN 'May'
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 6 THEN 'June'
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 7 THEN 'July'
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 8 THEN 'August'
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 9 THEN 'September'
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 10 THEN 'October'
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 11 THEN 'November'
    WHEN EXTRACT(MONTH FROM ORDER_DATE) = 12 THEN 'December'
  END AS MonthName,
  SUM(QUANTITY_ORDERED * PRICE_EACH) AS TotalSales
FROM
  harshit-401709.MeriSkil_sale_data.Sales_data
GROUP BY  MonthName
ORDER BY  MonthName;`

![Chart 1_ Sales by month](https://github.com/harshthegoose/MeriSkill-Sales-Analysis/assets/19995625/0824af45-55d9-450f-b927-f7fd401e368f)

>The Chart shows sales over each month. 

* Extracted Month Numbers: We used the EXTRACT(MONTH FROM ORDER_DATE) function to get the month number (1-12) from the Order_date column.
* Converted Numbers to Month Names: A CASE statement was employed to map each month number to its corresponding name (January, February, etc.).
* Calculated Total Sales per Month: The query grouped the data by month (now with names) and used SUM(QUANTITY_ORDERED * PRICE_EACH) to calculate total sales for each month.
* The analysis revealed that December had the highest total sales, followed by October and April.
* A bar chart created in Tableau likely visualized these sales trends across the months.  

## 2. Identify Top-Selling Products:

SQL query to determine the best-selling products using the "PRODUCT" and "QUANTITY_ORDERED" columns:

`SELECT
    PRODUCT,
    SUM(QUANTITY_ORDERED) AS TotalQuantitySold
FROM
    harshit-401709.MeriSkil_sale_data.Sales_data
GROUP BY    PRODUCT
ORDER BY
    TotalQuantitySold DESC;`

![Most selling Product (quantity)](https://github.com/harshthegoose/MeriSkill-Sales-Analysis/assets/19995625/0a00a8d6-7002-41df-9b1a-a57dc08cadbd)


>This chart shows the most selling products.


**Top Seller:**

* Product: AAA Batteries (4-pack)
* Total Quantity Sold: 30,372

**Least Seller:**

* Product: LG Dryer
* Total Quantity Sold: 633

## 3.  Finding the Best-selling product by total sales each:

`SELECT
    Product,
    SUM(QUANTITY_ORDERED * PRICE_EACH) AS TotalSales
FROM
    harshit-401709.MeriSkil_sale_data.Sales_data
GROUP BY
    Product
ORDER BY    Product`

![Best selling products](https://github.com/harshthegoose/MeriSkill-Sales-Analysis/assets/19995625/784ad284-5dd3-4bed-89ad-0130ad3e120f)

> This chart shows the distribution of sales over all products. 

* Top-selling product: Macbook Pro Laptop
* Total sales: $7,867,600
* Percentage of total sales: 23.36%




# Analyze

## 4. Top 5 best-selling products using a stacked bar chart. 

`WITH SalesPerProduct AS (
  SELECT
    Product,
    SUM(QUANTITY_ORDERED * PRICE_EACH) AS TotalSales
  FROM
    harshit-401709.MeriSkil_sale_data.Sales_data
  GROUP BY
    Product
)
SELECT
  Product,
  TotalSales,
  (TotalSales / (SELECT SUM(TotalSales) FROM SalesPerProduct) * 100) AS SalesPercentage
FROM  SalesPerProduct
ORDER BY SalesPercentage DESC
Limit 5;`

![Top 5 Best selling products](https://github.com/harshthegoose/MeriSkill-Sales-Analysis/assets/19995625/ddfa246b-dcd3-41a5-9c0d-11cf94d92734)

>The chart displays the top 5 selling products. 

**Top Products Drive Sales:**
* Sales data analysis reveals that the top 5 best-selling products contribute nearly half (almost 50%) of all total orders.

## 5. Top 5 cities by sales on map

`SELECT    CITY,
    SUM(QUANTITY_ORDERED * PRICE_EACH) AS TotalSales
FROM    
harshit-401709.MeriSkil_sale_data.Sales_data
GROUP BY    
CITY
ORDER BY    
TotalSales DESC
LIMIT 5;`

Geographic Distribution of Top Sales Cities:
Interestingly, the top 5 cities by sales are geographically dispersed across the country.
San Francisco takes the lead with impressive total sales of $8,055,046.
Los Angeles follows closely behind at $5,310,846.



![Top 5 Cites by sale](https://github.com/harshthegoose/MeriSkill-Sales-Analysis/assets/19995625/eb768afc-b21f-4e0f-a69a-269462e2fd34)

> The chart shows the top 5 cities by sales. 

## 6.  Weekly sales distribution by weekday.

`SELECT
    EXTRACT(DAYOFWEEK FROM ORDER_DATE) AS Weekday_Number,
    CASE EXTRACT(DAYOFWEEK FROM ORDER_DATE)
        WHEN 1 THEN "Sunday"
        WHEN 2 THEN "Monday"
        WHEN 3 THEN "Tuesday"
        WHEN 4 THEN "Wednesday"
        WHEN 5 THEN "Thursday"
        WHEN 6 THEN "Friday"
        WHEN 7 THEN "Saturday"
        ELSE "Unknown"
    END AS Weekday,
    EXTRACT(WEEK FROM ORDER_DATE) AS Week_Number,
    SUM(SALES) AS Total_Sales
FROM
    harshit-401709.MeriSkil_sale_data.Sales_data
GROUP BY
    Weekday_Number,
    Weekday,
    Week_Number
ORDER BY    Week_Number,    Weekday_Number;`


![Weekly sales distribution by weekday](https://github.com/harshthegoose/MeriSkill-Sales-Analysis/assets/19995625/a6cddeeb-2024-4ac5-b647-e55034871937)

> The chart shows the weekly sales distribution. 

Sales Spike: Tuesdays see the highest sales volume.
Slow Thursday: Thursdays experience the lowest sales compared to other weekdays.
Weekend Trend: Weekends show average sales figures, suggesting a consistent but less dramatic sales pattern compared to weekdays.

## 7. To find the revenue metrics:

* **Total profit**: Sum up the net profit from all sales transactions.
* **Sales quantity**: Calculate the total number of units sold.
* **Profit margin**: Compute the ratio of net profit to total revenue, usually expressed as a percentage.


### 1. Total Sales:- Revenue = Sum of Sales


![Total sales](https://github.com/harshthegoose/MeriSkill-Sales-Analysis/assets/19995625/b6cc6db9-eb73-4056-a416-8711725f58bf)

 >The results showed that the revenue was 33671.60K.


### 2. Sales quantity:- Sum of Quantity Ordered.

![Qty Ordered](https://github.com/harshthegoose/MeriSkill-Sales-Analysis/assets/19995625/25b68d5a-6985-4b9a-b41b-4d315331d531)

 >The results showed that the Sales Quantity was 200.85K.


### 3. Profit margin: 

Find the total cost by using the new measure
Total Cost = SUM('Sales Data'[Price Each])
Find the total sales by using the new measure
Total Sales = SUM( 'Sales Data'[Sales])
Find the profit margin by using this formula in the measure.
PM = ( ([Total Sales] - [Total Cost] ) / [Total Sales] ) * 100

![Profit](https://github.com/harshthegoose/MeriSkill-Sales-Analysis/assets/19995625/d1553cb4-30b7-482d-bb3d-1a6ad0db33be)

 >The results showed that the Profit Margin was 57.85 percent.


# Share

###Here are some insights that I got from my analysis,

December has the highest overall sales, followed by October and April. A bar chart would effectively illustrate these trends.

*Top-Selling Products:*

* By unit sales, AAA Batteries (4-pack) is the leader, followed by LG Dryer (least seller).
* In terms of total revenue, the Macbook Pro Laptop reigns supreme, contributing significantly (23.36%) to total sales.

*Top Contributors:*

* The top 5 best-selling products account for nearly half of all sales, highlighting their crucial role in driving revenue.
* The top 5 sales cities are spread nationwide, indicating a diverse customer base.
* San Francisco leads the pack with the highest total sales, followed by Los Angeles.

*Weekly Sales Patterns:*

* Tuesdays witness the highest sales volume, while Thursdays experience the lowest.
* Weekends see average sales, suggesting a consistent but less dramatic pattern than weekdays.

*Revenue Metrics:*

* Total revenue is impressive at 33671.60K.
* The total sales quantity is substantial at 200.85K units.
* The healthy profit margin of 57.85% indicates efficient business operations.

# Act

*Additional Recommendations:*

* We should consider analyzing sales trends by product category or brand for deeper insights.
* Explore customer demographics to identify target markets for specific products.
* Investigate customer behavior to understand purchase patterns and preferences.
* Thursdays experience the lowest sales over the week, so we can give additional offers on Thursdays to drive more sales. 

