# Plant-Co.-Quantity-performance-Report-using-Power-BI
Welcome to my blog on Developing reports with Power BI! Power BI is a powerful business intelligence tool by Microsoft that transforms data into visually appealing and insightful reports and dashboards. Whether you’re a business professional, data analyst, or someone curious about data visualization, this guide will help you take your first steps into the world of Power BI and unleash its full potential.

![Dashboard](https://github.com/user-attachments/assets/545487e6-69d1-4ee2-9632-b23c9f58d26f)

Data analysis is a game-changer for optimizing business operations, empowering informed decisions and strategic planning. Recently, I worked on a project where I analyzed Quantity Performance Data from Plant Co., designed an interactive dashboard, and visualized the insights using Power BI. In this article, I’ll walk you through my approach, from collecting the data to crafting the interactive dashboard.

## Objective
To develop a Quantity performance report for the Plant Co. based on the Account Profitability Segmentation, Gross Profit, Year-to-Date, Prior-Year-to-Date and PYTD vs YTD.

![Plant Co  Wireframe](https://github.com/user-attachments/assets/b7aec8cb-0cd1-487b-9766-47e792e5db06)


## Data Source
To get the Source data, check my Raw data folder.
## Power Query
After importing the dataset, duplicates were removed, and the Header was checked along with the data type of each column.

### DAX Calculation
To store the key Metrics which is to be calculated, I created a separate dimension Table named Measures.

To calculate YTD, the date was created using the DAX formula,

Dim_Date = 
CALENDAR(
    DATE(2022,01,01),
    DATE(2024,12,31)
)

To get the only dates present in our data,

Inpast = 
VAR lastsalesdate = MAX(Fact_Sales[Date_Time])
VAR lastsalesdatepy = EDATE(lastsalesdate,-12)
RETURN
Dim_Date[Date] <= lastsalesdatepy

Another Table for Filtering was created with Sales, GP and Quantity.

Basic measures such as GP, Sales and Quantity were calculated,

-- For GP
Gross Profit = SUM(Fact_Sales[Sales_USD])-SUM(Fact_Sales[COGS_USD])

-- For Quantity
Quantity = SUM(Fact_Sales[quantity])

-- For Sales
Sales = SUM(Fact_Sales[Sales_USD])

For PYTD, the Dax formula was,

PYTD_GP = CALCULATE([Gross Profit],SAMEPERIODLASTYEAR(Dim_Date[Date]),Dim_Date[Inpast]=TRUE)
Similarly for others.

For YTD, the Dax formula was,

YTD_GP = TOTALYTD([Gross Profit],Fact_Sales[Date_Time])
Similarly for others.

For Metrics values to be displayed in the Card, such as PYTD,

S_PYTD = 
VAR selected_value = SELECTEDVALUE(Slc_Values[Value])
VAR result = SWITCH(selected_value,
    "Sales",[PYTD_Sales],
    "Quantity",[PYTD_Quantity],
    "Gross Profit",[PYTD_GP],
    BLANK()
)
RETURN
result

S_YTD = 
VAR selected_value = SELECTEDVALUE(Slc_Values[Value])
VAR result = SWITCH(selected_value,
    "Sales",[YTD_Sales],
    "Quantity",[YTD_Quantity],
    "Gross Profit",[YTD_GP],
    BLANK()
)
RETURN
result

YTD vs PYTD was calculated by,

YTD vs PYTD = [S_YTD]-[S_PYTD]
After calculating the measures the Dashboard was created.

## Creating Dashboard
Key Metrics were created with the conditional format in colour to indicate Profit and Loss.

![Key Metrics](https://github.com/user-attachments/assets/054c15b0-2374-4b67-a2ba-7f9128c9d4bd)

A Waterfall Chart was created to indicate the profit and loss,

![Waterefall Chart](https://github.com/user-attachments/assets/88b1acc3-6cc7-4262-8449-cd3da49d8fbc)

Account Segmentation based on Profitability was represented using the Scatter plot,

![Scatter Plot](https://github.com/user-attachments/assets/cb89e0d2-a7cf-4eee-a90d-71e1f978e52b)

A treemap to indicate the Difference between YTD vs PYTD of the bottom 10 Countries was created,

![TreeMap](https://github.com/user-attachments/assets/05d9f362-fc3a-4014-ae0f-d3b4409235c6)

A stacked Bar and line chart was created to represent the Product Type Quantity Performance,

![Chart](https://github.com/user-attachments/assets/48b90f8b-ccd2-4170-99e1-1a92e9458daa)

Filter based on main key metrics such as GP, Sales and Quantity was created and also based on Year.

![Filter](https://github.com/user-attachments/assets/8aa174b6-79e2-49e3-9c08-7d6c239e5365)

## Conclusion
1. A report-based on Quantity performance report for the Plant Co. based on the Account Profitability Segmentation was created.
   
2. PYTD vs YTD by country were created which indicates that the Quantity performance in China, France and Sweden needs to be checked.

3. On comparing YTD vs PYTD, June month had a very good performance this year.

You can check my Dashboard, which I made using Power BI.

Thanks for reading, see my profile for more Data Analysis projects!
