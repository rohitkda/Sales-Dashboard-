# Sales Dashboard

 Problem Statement

This dashboard helps the company understand its sales performance, product profitability, and regional distribution better. It enables the company to identify areas for improvement, particularly in product categories, shipping options, and regional sales, which are critical for enhancing revenue and profit margins. By analyzing Year-to-Date (YTD) sales trends, profit margins, and quantity sold, the company can pinpoint the causes behind declining sales in key categories and adjust its strategies accordingly.

## Dataset Overview

        Columns 21 and Rows 113271 

## Key Columns and Descriptions
        
- customer_id: Unique identifier for each customer.

- first_name / last_name: Customer's first and last name.
- category_name: Product category (e.g., Office Supplies, Furniture).
- product_name: Name of the specific product ordered.
- segment: Customer segment (e.g., Corporate, Consumer, Home Office).
- city / state / country / region: Customer's location details.
- delivery_status: Delivery status of the order (e.g., delivered, pending).
- order_date / ship_date: Date the order was placed and shipped.
- shipping_type: Type of shipping selected (e.g., First Class, Second Class).
- days_for_shipment_scheduled /   days_for_shipment_real: Scheduled and actual shipping time.
- order_item_discount: Discount applied to the item.
- sales_per_order: Total sales value per order.
- order_quantity: Number of items in each order.
- profit_per_order: Profit generated per order.

### Steps followed 



- Step 1: Dataset Acquisition : Obtain the dataset from the chosen source, such as Kaggle, and load it as a CSV file into Power BI Desktop.

- Step 2: Data Preparation : Open the Power Query Editor and, in the "View" tab, enable options like "Column distribution," "Column quality," and "Column profile" under the Data Preview section. Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".

- Step 4: Data Cleaning : Check for errors, empty values, or anomalies in each column. In this dashboard, there are no null values in any columns, so no imputation or additional data cleaning is necessary.

- Step 5 Date Table Creation: Created a date table to enable time-based calculations and Added columns for year and month using DAX 
        functions to extract year and month names.

 
        Data table = CALENDAR(MIN(sales_data[order_date]),MAX(sales_data[order_date]))

        Year = YEAR('data table'[Date])
        
        Month = FORMAT('data table'[Date],"mmm")
Created one column to extract month number in order to show trend line in KPI card 
        
        Month number = MONTH('data table'[Date])



- Data Modeling: Established relationships between the tables to create an optimized data model for reporting.





- Step 5: Creating KPI Cards: KPI Metrics 

YTD Sales, YTD Profit, YTD Quantity, YTD Profit Margin.

        YTD Sales DAX: YTD_Sales = TOTALYTD(sum(ecommerce_data[sales_per_order]),'data table'[Date])


-   YoY sales: To determine YoY Sales, it is necessary to first calculate PYTD Sales, which represents the total sales during the same period in the previous year. 

        YOY Formula: YOY(YTD sales - PYTD sales) / PYTD sales

        PYTD: PYTD Sales = CALCULATE([total sale], DATESYTD(DATEADD('data table'[Date], -1,YEAR)))

        YOY: YoY_Sales = ([YTD Sales] - [PYTD Sales])/[PYTD Sales]
           
- YTD Profit Margin: we don't have any column named Profit Margin so we calculated Profit Margin.

        Profit margin = SUM(ecommerce_data[profit_per_order])/SUM(ecommerce_data[sales_per_order])

        YTD_PM: YTD profit margin = TOTALYTD([Profit margin],'data table'[Date])


- Sales Icon and Color Coding: Icons and background colors were added to indicate performance trends:

Icons: Show an upward or downward arrow based on YoY Sales

        Sales_icon = IF([YoY_Sales] > 0, UNICHAR(9650), UNICHAR(9660))  // Up or down arrow

Colors: Change background color based on positive or negative YoY trends
        
        Sales_color = IF([YoY_Profit] > 0, "Green", "Red")

- Trend line shown by month sales 

# Dashboard 
![Screenshot 2024-11-17 132700](https://github.com/user-attachments/assets/57c1cf67-e491-436c-b8d2-2113d61a04f8)



## Dashboard Visualizations

- Sales by Category (Matrix)
        
        Dimensions: Category, YTD Sales, PYTD Sales, YoY Sales, Sales Icon.

        Provides a breakdown of sales Performance across product categories.

- Sales by Region (Map)

        Displays sales distribution by state with regional color coding.

- Top 5 & Bottom 5 Products by YTD Sales (Bar Chart)

        Visualizes the highest and lowest performing products.

- Sales by Region (Pie Chart)
      
        Dimensions: Customer region, YTD Sales.
        Displays sales by region with percentage breakdowns.

- Sales by Shipping Class (Pie Chart)

        Dimensions: Shipping category, YTD Sales.
        Shows the proportion of sales based on the shipping method chosen.



## Key Observations from the Dashboard

- Overall Performance:

        YTD Sales: $11.53M, which shows a decline of 0.83% YoY.

        YTD Profit: $1.34M, an increase of 4.50% YoY, indicating profitability improvements despite sales decline.

        YTD Quantity Sold: 107.2K, a decrease of 7.29% YoY, suggesting a drop in the number of items sold.

        Profit Margin: 11.58%, showing a healthy increase of 5.37% YoY.

- Sales by Category:
        
        Technology: Slight YoY decline (-1.37%).

        Office Supplies: Consistent but marginal YoY decline (-1.22%).

        Furniture: Positive growth with a 0.73% YoY increase.

- Shipping Analysis:

        Standard class with (60.51%)
        Second class with (19.22%)
        First class with (15.1%)
        Same day with (5.17%)

- Regional Sales:

        Top Region: $3.72M (32.22%) - West.
        Second-highest: $3.28M (28.42%) - East 
        Mid-tier:  $2.67M (23.19%) - Central
        Lowest Region: $1.87M (16.17%) - South.

        Potential Opportunity: Central and South regions, as they contribute less to overall sales.

- Product-Level Performance:

        Top Product: Staple envelope ($57.09K).     
        Bottom Product: Rediform S.O.S. ($179.99), showing extremely low sales.

# Insights
### YTD KPIs Overview 

 Sales ($11.53M, -0.83%)

        Sales have slightly declined year-over-year (YoY), driven by underperformance in Technology (-1.37%) and Office Supplies (-1.22%).
 Profit ($1.34M, +4.50%)
    
        Increased profitability suggests better cost control or optimized pricing strategies, particularly in high-margin products.

 Quantity Sold (107.2K, -7.29%)

        A sharp decline in units sold may indicate higher prices or reduced customer demand for specific products.

 Profit Margin (11.58%, +5.37%)

        Higher profit margins point to effective operational and pricing efficiencies.

### Sales by Category

Technology (YTD $2.10M, -1.37%)
        
        Decline in sales could be linked to reduced demand for specific products or increased competition

Office Supplies (YTD $6.92M, -1.22%)

        As the largest contributing category, its decline has a significant impact on overall performance.

Furniture (YTD $2.52M, +0.73%)

        The only growing category; this trend should be leveraged for cross-selling opportunities.

### Sales by Region

West ($3.72M, 32.22%)
        
        Top-performing region, contributing the largest share of revenue.

South ($1.87M, 16.17%)

        Lowest-performing region; requires targeted campaigns to increase penetration.

East ($3.28M, 28.42%) and Central ($2.67M, 23.19%)

        Moderate performance with room for improvement.

 ### Sales by Shipping Type
Standard Class (60.51%)

        Most preferred shipping type, highlighting its importance in maintaining customer satisfaction.
First Class and Same Day (20.34% combined)

        Limited adoption suggests niche demand; consider bundling with high-value products.

### Product-Level Performance
Top Products: Staples, Staple Envelope, and Easy-staple Paper dominate sales.

        Focus on these high-performing 

products for promotions and upselling.
Bottom Products: Eldon Jumbo, Lexmark X, and Cisco SPAS25C contribute minimally.

        Evaluate whether to discontinue or reposition these products.

