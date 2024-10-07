# Ecommerce Sales Dashboard

### Dashboard Link : https://github.com/user-attachments/assets/94328b52-f58c-430e-9ef8-55617b61f5b4

## Problem Statement

This dashboard helps the US based Ecommerce company understand their customers better. It helps the business know about their profit and revenue for a certain year and it's corresponding growth/decline w.r.t. previous year.

Through different KPIs and charts, we get to be acquainted with the categories(Office Supplies, Furniture and Technology) or the Sales regions in the US that are performing well or the ones which are lagging behind and needs to be worked upon. 

With the help of this dashboard, the company has also analysed the top 5 and bottom 5 performing products w.r.t. YTD Sales measure. By employing interactive buttons as well as slicers, this dashboard helps to view the individual performance of the segments(Consumer, Corporate, Home Office) accompanied by the shipping type used by the customers. 



### Steps followed 

- Step 1 : Review the dataset which is available as a set of 2 csv file in excel and analyse the number of column heads(in this case 21 columns) and the number of rows for the data(this dataset contains 113270 rows). The other table contains the latitude and longitude information for the various states of US. 

- Step 2: Load data into MS SQL server by creating a new database and importing the aforementioned flat files as tables. Remember to modify the columns types appropriately for each of the column heads(for e.g. choose varchar(200) data type for product name as the number of characters are large in this column head). 

- Step 3: Open Power BI Desktop and connect it to MS SQL server. Enter the credentials for the server and database correctly and select the Data connectivity mode to IMPORT. 

- Step 4: In the Model View of the Power BI Desktop, create a relationship between the two tables i.e. Latitude/Longitude table and the Ecommerce_date table via ONE-TO-MANY Cardinality Relationship on Customer state field and set the Cross-filter direction to SINGLE. 

- Step 5: Open Power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options. Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".

- Step 6: Now, to calculate the Time-Intelligence Functions like YTD functions, PYTD functions, etc., we need to add a Date Table.
Following DAX expression was used for calculating Calendar Table: 

    Calendar = CALENDAR(MIN(Ecommerce_data[order_date]), MAX(Ecommerce_data[order_date]))

The Calendar Table uses dynamic start and end dates to optimize the storage space. 

Calendar Table Snap: ![Snapshot 1](https://github.com/user-attachments/assets/e1b49ef0-bd73-4612-a395-7adf2c64c87f)

- Step 7: Extract the Date and Year Columns from the Calendar by using the following DAX Functions: 

    Year = YEAR('Calendar'[Date])

    Month = FORMAT('Calendar'[Date], "MMMM")

    Month Number = MONTH('Calendar'[Date])

Remember to create the ONE-TO-MANY Cardinality Relationship between the Calendar Table and Ecommerce_data Table on Order Date and set the cross-filter direction as SINGLE. 

- Step 8 : In the Report View of the Power BI Desktop, select the background theme. In this project, a jpg image was inserted by selecting Canvas background option in Format Page of the Visualizations Tab. Note: reduce the transparency to 0%. 

- Step 9 : Now, select the required Text Box for the Title of Dashboard by selecting the Shape option from Insert Tab. 
Format the background color and shape accordingly. 
Similarly, add the 4 KPI banners required to analyse and answer the problem statements namely - YTD Sales, YTD Profit, YTD Quantity and YTD Profit Margin. 

- Step 10 : Now, for the first KPI - YTD Sales, calculate a new measure YTD Sales using the following DAX Function: 

    YTD Sales = TOTALYTD(SUM(Ecommerce_data[sales_per_order]), 'Calendar'[Date])

Add the YTD Sales Measure in the Fields head of the KPI visualization. Format the background and the text field of the KPI banner in accordance with the theme using Format Visual. Set the data type for the YTD Sales Measure as Currency. 

- Step 11 : To increase the interactivity of the YTD KPI Banner, insert an Area Chart with Month as X-axis and Sales_per_order as Y-axis. Since, we are visualising the dashboard for latest year, select the year 2022 by using the Year Filter in the Filters Tab(select Filter on this visual). 
Note: If the area chart is not sorted correctly(from January to December, select sort column by month number from Column tools in the ribbon menu).

- Step 12: Now, we'll calculate the YoY growth/decline for the Sales KPI. 
For this, first we will calculate a new measure named as PYTD Sales using the following DAX expression: 

    PYTD Sales = CALCULATE(SUM(Ecommerce_data[sales_per_order]), DATESYTD(SAMEPERIODLASTYEAR('Calendar'[Date])))

Now, the YoY growth/decline will be calculated as a new measure using the DAX expression: 

    YoY Sales = ([YTD Sales] - [PYTD Sales])/[PYTD Sales]

In this case, the YoY Sales(data type is %) is - 0.83% which is depicted along with downward arrow in red background color using the DAX expressions: 

    Sales Icon = var positive_icon = UNICHAR(9650)
             var negative_icon = UNICHAR(9660)
             var result = IF([YoY Sales]>0, positive_icon, negative_icon)
             return result


    Sales Background Icon Color = IF([YoY Sales]>0, "Green", "Red")

The final YTD Sales KPI banner with monthly area chart, and YoY Sales trend will be as the following screenshot: 
 ![Snapshot 2](https://github.com/user-attachments/assets/d04158de-9108-4b0d-b269-a9ea964184dd)

- Step 13: Now, for the other 3 remaining KPIs, namely YTD Profit, YTD Quantity and YTD Profit Margin, follow the Steps 10 to 12 precisely and the resultant dashboard with the 4 KPIs will be as follows: 

KPI Banners Snap: 
![Snapshot 3](https://github.com/user-attachments/assets/af0a5a51-83e4-475b-89e9-df8ce81ff26c)

- Step 14: For the next part of the Ecommerce Dashboard, a matrix chart representing Sales by Category was added depicting the YTD Sales, PYTD Sales, YOY Sales, and Sales Icon. 
In the Format Visual tab, the column subtotal and row subtotal toggle option are switched off as we  require only the individual aggregations.
The background and formatting is adjusted accordingly to the theme in the Format Visual Tab. 

- Step 15 : A Map was also added to the report design area representing the Sales amount from different US states. While creating this visual, field named "region" was added to Legends bucket while "YTD Sales" was added to the Bubble size bucket, thus depicting - larger the bubble size, more is the Sales for that state in a particular region. 

Sales by State screenshot with tooltips providing relevant Sales information about New York state: 

![Snapshot 4](https://github.com/user-attachments/assets/cd6d901a-91d9-4419-bfe0-f63dd49709cb)

- Step 16 : Bar charts were used to represent Top 5 and Bottom 5 performing products on the basis of "YTD Sales" parameter. For this chart, select the Top N Filter option in the Filter Tab, and mention numeric value as "5" to select the Top 5/Bottom 5 appropriately. These charts will help to analyse the products which are performing well, and the products whose marketing needs to be enforced to increase sales, or the products that need to be discontinued due to declining sales over the period. 

Following is the snapshot of how the two bar charts will look: 
![Snapshot 5](https://github.com/user-attachments/assets/b29fbce1-1bac-4872-9763-4405f2e6ab9c)
  
  
- Step 17: The YTD Sales were then analysed through a donut chart for the regions of the US. The states were grouped into 4 major regions namely East, west, Central, and South. Moreover, the YTD Sales were also analysed through the shipping methods used to deliver the products to the customers. 

 
 - Step 18: At last, a slicer was inserted for the entire dashboard on the basis of customer segments - Consumer, Corporate or the Home Office. This interactive slicer helps to analyse the detailed performance of the individual segments just by easily clicking the required segment button. 
 
 - Step 19: The report was then published to Power BI Service.
 
 
 # Report Snapshot (Power BI DESKTOP)

 ![Capture](https://github.com/user-attachments/assets/4ce37e22-8abd-48f2-9dc9-f3231fe7b1db)

# Insights

A single page report was created on Power BI Desktop & it was then published to Power BI Service.

Following inferences can be drawn from the dashboard;

### [1] YTD Sales for the year 2022 = $11.53M

    a) The YTD Sales has decreased over the past year by 0.83%(i. e. from $11.63M in the year 2021 to $11.53M in year 2022). 

    b) The YTD Profit has increased by 4.60%(from $1.28M in year 2021 to $1.34M in the current year 2022).
   
    c) The YTD Quantity for the number of products sold has dcreased from 116K to 107K, a total decrease of 7.29%. 

    d) The YTD Profit Margin has increased by 5.37% in the year 2022 as compared to year 2021. 


#### Thus, we can analyse that although the Sales and Quantity of products has decreased but Profit and Profit Margin has increased in the year 2022, which can mean that the operating costs of the business might have decreased in the current year which is beneficial for the business. 
           
### [2] The Furniture category has seen an increase in their YTD Sales over the year 2022 while the other two categories namely Office Supplies and Technology has decreased. 

    a) The YoY sales growth for the Furniture category was 0.73%, while the YoY sales decline for the Office Supplies and Technology Category was 1.22% and 1.37% respectively. 

    b) The maximum sales share contribution was made by the Office Supplies category(60%) in comparison to the Furniture and Technology category

#### Thus, we can analyse that Office Supplies category was more popular among the customers of the Ecommerce Company, however, it was Furniture category that showed positive sales trend in the year 2022.
  
  ### [3] Sales by State 
  
    a) The YTD Sales for the California state was maximum, which was equal to $23,35,532.25 while it was lowest for Wyoming, equals to $1609.54

    b) However, the YoY growth was maximum for the state of Wyoming, with sales growth of 89.38%. 

#### Thus, we can analyse that although the state of California contributed maximum towards the Sales, however, it was Wyoming state which performed exceptionally well in contrast to other states. 

 ### [4] Some other insights
 
 ### Top 5 Products by YTD Sales:

    a) Staple Envelope
    b) Staples
    c) Easy-staple paper
    d) Staples in miscellaneous colors
    e) KI- adjustable Height Table


 ### Bottom 5 Products by YTD Sales:
 
    a) Eldon Jumbo Profile Portable File Boxes Graphite/Black
    b) Lexmark X 9575 Professional All-in-one Printer
    c) Cisco SPA525G2 5-Line IP Phone
    d) Xerox Blank Computer Paper
    e) Rediform S.O.S. Phone Message Books

    
### YTD Sales by Shipping Types: 

    a) First Class - 15.1%
    b) Second Class - 19.22%
    c) Standard Class - 60.51%

#### Thus, we can analyse that maximum customers opted for Standard shipping type rather than opting for First and Second Class shipping, which means that they value money and don't like to send more money for shipping purposes. 
