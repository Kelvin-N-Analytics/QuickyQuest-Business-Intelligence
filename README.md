# Retail Sales Analysis 
## Introduction
QuickyQuest Enterprises is a dynamic retail enterprise operating a network of ten conveniently located refreshment stores in the USA. Our mission is to provide customers with a quick, easy, and enjoyable shopping experience while offering a diverse selection of high-quality beverages.

![QuickyQuest Introduction](https://github.com/user-attachments/assets/327e064c-f2a0-48e1-bea7-330b017c1df9)

QuickyQuest Enterprises operates a network of ten convenience refreshment stores throughout the USA:
- Rapid Refreshment
- Quest Grab
- Quick Stop
- Pour Palace
- Quest Mart
- Grab & Go
- Quench Corner
- Quick Mart
- Icy Blast
- Sip & Dash
  
All QuickyQuest stores are committed to providing excellent customer service and a clean, inviting atmosphere. Our friendly staff is always ready to assist customers and ensure a positive shopping experience. Our stores are strategically located and designed to provide customers with a quick and easy shopping experience, offering a wide variety of beverages, snacks, and other convenience items.

## Problem Statement
To gain a comprehensive understanding of customer behavior, product performance, and store-level profitability at QuickyQuest Enterprises, we need to answer the following questions:
1.	**Customer Analysis:**
-	What is the profitability of male and female customers?
-	How do spending patterns vary across different age groups?
-	Which customer segments are the most valuable to the business?
2.	**Profitability Trends:**
-	Are there any seasonal patterns in profitability?
-	What is the overall trend of profit growth over time?
-	Are there any areas where profitability can be improved?
3.	**Product Performance:**
-	Which products are the top-sellers and most profitable?
-	What are the product return and refund rates for different products?
- How can inventory management be optimized based on product performance data?
4.	**Store Performance:**
-	Which stores are underperforming or exceeding revenue targets?
-	What are the reasons for performance gaps at individual stores?
-	How can resource allocation be optimized to improve store-level performance?
5.	**Revenue Analysis:**
-	What are the trends in quarterly and weekday/weekend revenue?
-	Are there any areas where revenue can be optimized?
-	How can operational decisions be informed by revenue analysis?

## User Story
I came across the dataset online and admired how comprehensive and rich the dataset is. As a data analyst, I want to work with a comprehensive and rich dataset to enhance my skills in data cleaning, data modeling, analysis, and visualization. I named the business QuickyQuest Enterprises and after exploring I came up with questions for the project.
As a data analyst at QuickyQuest Enterprises, I want a comprehensive dashboard that provides insights into: 
-	customer behavior.
-	product performance.
-	store-level profitability.
   
so that I could make data-driven decisions to optimize operations and improve overall QuickyQuest Enterprises business performance.

## Data Source
The primary data used in this analysis is ‘sales_data.csv’ file containing five (5) tables downloaded online.
[Download Files Here](https://drive.google.com/file/d/1ymmNpf4jyo4ahXZY78F7irc9eQj6E_f0/view?usp=sharing)
Data files:
-	**Customer_table:** Contains information about customers, including Customer ID, First Name, Last Name, Gender, Location and Date of Birth. 610 rows of data
-	**Fact_table:** Contains transaction-level data, including Product ID, customer ID, Sales person ID, Order date, quantity sold, Quantity returned and Payment method. (*20,030 rows of 
  data*) 
-	**Monthly_store_targets:** Contains monthly revenue targets for each store, including Store ID, Month, Monthly Target. (*120 rows of data*)
-	**Products_table:** Contains information about products, including Product ID, Product Name, Category, Sales Price, and Cost Price. (*100 rows of data*)
-	**Sales_persons_table:** Contains information about salespersons, including Sales Person ID, First Name, Last Name, Store Name and Date of Birth. (*10 rows of data*)

## Exploratory Data Analysis
Exploratory Data Analysis involved gaining a preliminary understanding of the data and identify potential insights and trends within the QuickyQuest Enterprises dataset. Key areas of the Exploration:
-	Identified distribution of customer characteristics, such as age, gender, and location.
-	Evaluated sales performance of different product categories and individual products.
-	Compared the performance of different stores in terms of revenue, customer traffic, and profitability.
-	Identified any data quality issues, such as missing values or inconsistencies.
-	Identified necessary data cleaning and transformation steps for the analysis.

## Tools
| Tool     | Functionality                                                                 |
|----------|------------------------------------------------------------------------------|
| Excel    | Primary data analysis and visualization                                      |
| Power Query | Data import, transformation, and cleaning from CSV file                   |
| Power Pivot | Data modeling, complex calculations, relationships, and analysis |                                                               

## Data Transformation
The raw data contained in the sales_data.csv file underwent several transformation processes to prepare it for analysis:
Data was efficiently cleaned and transformed with Power Query Editor in Excel 

![Transformed Data](https://github.com/user-attachments/assets/e467bfc0-70df-48b1-bd24-43f7b7c7576e)

**Customer_table:**
Renamed Customer_table to Dim_Customer and promoted first row as headers.
Data Cleaning: Removed duplicate records, Merged First and Last Name together, corrected inconsistent data on Location column and handled missing values.
Data Standardization: Standardized customer location data to a consistent format 
Data Enrichment: Inserted Customer Age column based on Customer Date of Birth and Added a conditional column Customer Age Group.

**Fact_table:**
Promoted first row as headers
Data Validation: Ensured data integrity by checking for inconsistencies between transaction data, customer data and product data.
Time Zone Conversion: Standardized transaction timestamps on Order Date column to a consistent time zone.

**Monthly_store_target:**
Promoted first row as headers
Data Standardization: Renamed Month column to Date. To create a relationship between monthly_store_targets table and Date table

**Products_table:**
Renamed Products_table to Dim_Products and promoted first row as headers
Data Categorization: Assigned products to appropriate categories based on product descriptions.

**Sales_persons_table:**
Renamed Sales_persons_table to Dim_SalesPerson and promoted first row as headers
Data Standardization: Merged First and Last name together.
Data Enrichment: Added SalesPerson_Age column base on Date of Birth 

**Created a new table for Date**
Named the table Date. The Date table provided flexibility for custom calculations (i.e. Quarter, WeekType..) and data exploration-based time-related dimensions like weekday sales and weekend sales

![New table Date](https://github.com/user-attachments/assets/aa6d21ee-c9bd-4b4c-85ad-ab7fa2a68328)

**Created a new table for Calculations** 
Name the table Calculations. By creating a separate table for calculations in Power Query improved performance by reducing query complexity and joins. Separating calculations into a dedicated table helped in maintaining an organized data model. It also offered greater flexibility for custom calculations and reusable measures, while maintaining a cleaner data model. This approach helped me simplify maintenance and updates, making it easier to manage calculations within Power Pivot model. The following are some of the calculations done the on the calculation table:

```DAX Measures
Total Revenue
=SUMX(fact_table,RELATED(Dim_Products[Sales Price]) * fact_table[Quantity Sold])

COGS
=SUMX(fact_table,
                  RELATED(Dim_Products[Cost Price]) * fact_table[Quantity Sold])

Profit Margin
=[Total Revenue] - [COGS]

% Profit Margin
=DIVIDE(
                [Profit Margin], 
                [Total Revenue]
                )

Return Rate
=DIVIDE(
                [Qty Returned ],
                 SUM(fact_table[Quantity Sold]),
                  0
                  )


Variance
=[Total Revenue] - [Total Target]

Variance %
=([Total Revenue] - [Total Target]) / [Total Target]

Total Refund
=SUMX(fact_table,
                 fact_table[Quantity Returned] * RELATED(Dim_Products[Sales Price]))
```

## Data Modelling
Created a robust and efficient data model that supported the analysis of customer behavior, product performance, and store-level profitability at QuickyQuest Enterprises. By following this data modeling approach, QuickyQuest Enterprises was able to establish a solid foundation for data analysis and reporting, enabling informed decision-making and effective business management. The calculations part shows measures that were calculated within the data model. Separating calculations into a dedicated table helped maintain a clean and organized data model, making it easier to understand and manage.

![Data Modelling](https://github.com/user-attachments/assets/05afe9b4-8265-4180-80b1-bce10306c212)

## Data Analysis and Visualization
For visualizing this analysis 3 dashboards were created:
**1.	Store-Level Revenue vs Target Dashboard (Stores Revenue)**
Store-Level Revenue vs Target Dashboard provides insights on:
-	Store Revenue vs Target: Presents a comparison of revenue vs. target for each store, which helps in understanding which stores are under or over-performing against their targets.
-	Month-by-Month Analysis: Offers a detailed month-by-month breakdown of total revenue and targets, highlighting variances. This can be important for financial planning and performance 
  management.
 	
![Stores-Level Revenue vs Target Dashboard](https://github.com/user-attachments/assets/d39324e3-763a-4f94-bf27-4ce916979467)

From the above Store-Level Revenue vs Target Dashboard:
-	QuickyQuest achieved a total revenue of $5.4M against a target of $5.3M, resulting in a positive variance of +3.7%. This indicates that the company exceeded its overall financial 
  goals.
-	There are significant variations in performance across individual stores. 
-	Some stores consistently outperform their targets, while others underperform.
-	Rapid Refreshment store has significantly exceeded its target revenue, showing a variance of +31.1%.
-	Quick Stop store underperformed, with a variance of -10.8% from its target revenue.

 **2.	Time-Frame Analysis Dashboard (Time Frame)**
Time-Frame Analysis Dashboard provides insights on:
-	Quarterly Revenue Analysis: Shows revenue generated per quarter against the average,
  providing a quick view of performance relative to the norm.
-	Weekday/Weekend Revenue Analysis: Gives a comparison of revenue generated on
  weekdays versus weekends, which can help in making staffing and operational decisions.
-	Monthly Revenue vs Target: Tracks monthly performance against goals, which is
  fundamental for short-term financial planning and adjustments.
 
![Time Frame Dashboard](https://github.com/user-attachments/assets/04eb9189-6a19-4712-9d27-6dafff0ecf92)

From the above Time-Frame Analysis Dashboard:
Weekday/Weekend Revenue:
-	Revenue Generated by Weekdays: $2M (71% of total revenue)
-	Revenue Generated by Weekends: $846K (29% of total revenue)
-	Weekdays are the primary drivers of revenue for the company.
  
 Monthly Revenue Growth:
-	Month of May and August exceeded their target revenue
  
Quarterly Revenue Growth:
•	The company's quarterly revenue has fluctuated throughout the year.
•	There have been periods of growth and decline, indicating seasonality or other factors influencing revenue.
•	The average quarterly revenue is approximately $136,869.

**3.	Profitability View Dashboard (Profitability)**
Profitability View Dashboard provides insights on:
•	Customer Analysis: provide insights into the profit generated from male and female
  customers, and breaks down the average spending by customer age-groups, showing which age groups are most profitable.
•	Profitability over Time: This includes a profit trend and month-over-month growth rate, which could help in identifying seasonal patterns or trends in sales effectiveness.
•	Profitability by Weekday: Analyzes which days of the week generate the most profit.
•	Product Analysis: Details the top-selling and most profitable products, as well as product return and refund rates. This is critical for inventory management and identifying which 
  products are most valuable to the QuickyQuest business.

![Profitability View Dashboard](https://github.com/user-attachments/assets/8a0c6a6a-68cc-4a39-8e27-c97e5812bf95)

From the above Profitability View Dashboard:
Customer Analysis:
-	Gender-Based Profit: Female customers contribute slightly more to overall profit than male customers, accounting for 48.53% of total profit compared to 51.47% for male customers.
-	Profit by Customer Age: The 31-40 age group is the most profitable, followed by the 41-50 age group.

Product Analysis:
-	Profit Margin by Product Category: Soft drinks and sports drinks have the highest profit margins, while alcoholic beverages and juice have the lowest.
-	Top-Selling Products: Common Splash, Eight Brew, and Attorney Mist are the top-selling products.
-	Product Return and Refund Rates: Product return and refund rates are relatively low, indicating high customer satisfaction.

Volume-Driven Profitability:
-	Top Profitable Products: Common Splash, Eight Brew, and Attorney Mist are not only top-selling but also highly profitable.

Profitability Trends:
-	Trend in Profit & MOM Growth Rate: The overall profit trend has been positive, with month-over-month growth rates fluctuating but generally showing an upward trend.
-	Profit Trend by Weekday: Tuesdays and Wednesdays generally have higher profit margins compared to other weekdays.

Store Performance:
-	Top-5 Profitable Locations: Washington, California, Michigan, Virginia, and Missouri are the top-5 profitable locations.
-	Average Customer Age: The average customer age varies across different locations, indicating varying customer demographics.

## Overall Performance Analysis Findings
-	QuickyQuest enterprises has demonstrated consistent revenue growth, exceeding the overall target by +3.7%.
-	Customer segmentation analysis revealed that female customers and specific age groups contribute significantly to profit.
-	The product mix is well-balanced, with a focus on high-margin and high-volume products.
-	Store performance varies across different locations, indicating the need for tailored strategies to optimize profitability.

## Conclusions and Recommendations
-	Leverage successful strategies from top-performing stores.
-	Optimize staffing and operational decisions based on weekday/weekend revenue patterns.
-	Targeted Marketing: Implement targeted marketing campaigns to attract and retain profitable customer segments.
-	Customer-Centric Approach: Focus on understanding and meeting the needs of the most profitable customer segments.
-	Product Optimization: Continuously analyze product performance to optimize the product mix and maximize profitability.
-	Store-Level Strategies: Implement targeted strategies to improve the performance of underperforming stores and leverage the success of top-performing locations.
-	Data-Driven Decision Making: Utilize the insights from these dashboards to make informed decisions and drive business growth.
-	Continuous Monitoring: Regularly review and analyze performance metrics to identify emerging trends and opportunities for improvement.
By implementing these recommendations, QuickyQuest Enterprises has enhanced customer satisfaction, optimized profitability, and achieved sustainable growth.
