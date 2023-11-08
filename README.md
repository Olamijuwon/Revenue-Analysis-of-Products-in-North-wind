# Revenue-Analysis-of-Products-in-North-wind

- [Project Overview](#project-overview)
- [Tools](#tools)
- [ Data Cleaning/Preparation](#data-cleaning/preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysyis](#data-analysyis)
- [ Result/Findings](#result/findings)
## Project Overview

This data project aims to provide insights into Sales improvements, which employee is bringing in more sales and Frequent customers. By analysing various aspects of the sales data, we seek to identify trends, dada driven recommendations and gain a deeper understanding of the company's performance.

## Tools

-Power BI - Data cleaning was donee in power query
          - Data analysis was done too.
          - Creating reports .

## Data Cleaning/Preparation

Initial data preparation phase;
- Data loading and inspection.
- Deleting of empty coloumns and removing of columns not needed for analysis.
- Data cleaning and formatting.
  - - Creating of Calendar Date Auto and connecting it to the date column in the data model.

## Exploratory Data Analysis

Key questions
- Showing Total sales by budget, last month, last year , last target this was done using Dax.
- Showing sales by continent.
- What is the average Sales Amount by Employee?
- Show sales representative that hits the highest target.
- Sales target by order month and title.

## Data Analysyis


DAX Measures used for the Nortwind Database


``` DAX
    Color = IF([Total Sales]>[Previous Month Sales], "Green", "Red")
```

``` DAX
    Color Sales/Target = IF([Total Sales] >= [Sales Target], "Green", "Red")
```

``` DAX
    KPI Color = 
            Var Sel = SELECTEDVALUE(Comparison[Comparison Fields])
            Var cond = SWITCH(TRUE(),
                                    CONTAINSSTRING(Sel, "Target"), [Total Sales]-[Sales Target],
                                    CONTAINSSTRING(sel, "Budget"), [Total Sales]- [Total Budget],
                                    CONTAINSSTRING(sel, "Last Month"), [Total Sales]-[Previous Month Sales], 
                                    CONTAINSSTRING(sel, "Last Year"), [Total Sales]-[SPLY Sales])
            var ret = SWITCH(TRUE(),
                            cond >= 0, "Green",
                            cond < 0, "Red",
                            "Black")
            Return 
                  ret
```


``` DAX
    KPI Total Sales = FORMAT([Total Sales], "$#,0") & " "
```

``` DAX
    KPI Total Sales Vs Budget = 
                Var var1 = [Total Budget]
                var percentage = DIVIDE([Total Sales], [Total Budget]) - 1
                Var sign = IF([Total Sales] >= var1, "+", "")
                Var ret = sign & FORMAT(percentage, "#0.0%") & " | " & 
                        FORMAT(var1, "#0,#")
            Return ret
```
                          

``` DAX
    KPI Total Sales Vs Last Month = 
                Var var1 = [Previous Month Sales]
                var percentage = DIVIDE([Total Sales], [Previous Month Sales]) - 1
                Var sign = SWITCH(TRUE(),
                                        [Total Sales] >= var1, "+",
                                          "")
                Var ret = sign & FORMAT(percentage, "#0.0%") & " | " & 
                        FORMAT(var1, "#0,#")
            Return ret
```


``` DAX
    KPI Total Sales Vs Last Year = 
                Var var1 = [SPLY Sales]
                var percentage = DIVIDE([Total Sales], [SPLY Sales]) - 1
                Var sign = IF([Total Sales] >= var1, "+", "")
                Var ret = sign & FORMAT(percentage, "#0.0%") & " | " & 
                        FORMAT(var1, "#0,#")
            Return ret
```


``` DAX
    KPI Total Sales Vs Tartget = 
                Var var1 = [Sales Target]
                var percentage = DIVIDE([Total Sales], [Sales Target]) - 1
                Var sign = IF([Total Sales] >= var1, "+", "")
                Var ret = sign & FORMAT(percentage, "#0.0%") & " | " & 
                        FORMAT(var1, "#0,#")
            Return ret
```

``` DAX
    Monthly % Share = DIVIDE([Total Sales],[Total Sales For The Year], 0)
```

``` DAX
   Previous Month Sales = CALCULATE([Total Sales], PREVIOUSMONTH(OrderCalendar[Date]))
```
 
 ``` DAX
     Sales Target = SWITCH(TRUE(),
                            [Total Sales] >= 100000 && [Total Sales] <= 200000, [Total Sales]/2,
                            [Total Sales] < 1000000, [Total Sales]+20000,
                            [Total Sales] > 200000, [Total Sales]-5000)
  ```                      
                      


``` DAX
    SPLY Sales = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(OrderCalendar[Date]))
```

``` DAX
    Total Budget = SWITCH(TRUE(),
                            [Total Sales] >= 100000 && [Total Sales] <= 200000, [Total Sales]*2,
                            [Total Sales] < 1000000, [Total Sales]+50000,
                            [Total Sales] > 200000, [Total Sales]+10000)
```

``` DAX
    Total Sales = SUM(Orders[SalesAmount])
```

``` DAX
   Total Sales For The Year = CALCULATE([Total Sales], REMOVEFILTERS(OrderCalendar[MonthNumber], OrderCalendar[OrderMonth]))
```

``` DAX
   Total Sales YTD = CALCULATE([Total Sales], DATESYTD(OrderCalendar[Date]))
```

``` DAX
    Variance = 
        Var Vari = DIVIDE([Total Sales], [Previous Month Sales]) - 1
        Var Ret = FORMAT(Vari, "#%") & " " & IF(Vari > 0, "(+)", "(-)")
     Return Ret   
```

``` DAX
OrderCalendar = CALENDARAUTO()
MonthNumber = FORMAT('OrderCalendar'[Date],"mm")
OrderDay = FORMAT('OrderCalendar'[Date],"dd")
OrderMonth = FORMAT('OrderCalendar'[Date],"mmm")
OrderQuater = FORMAT('OrderCalendar'[Date],"Q")
OrderYear = FORMAT('OrderCalendar'[Date],"YYYY")
```

## Result/Findings

1 - Sales target by order month and title:
Sales representative have been hiring the highest target $50k - 65k from Jan to march .
Vice President hit the highest sales target In the month of April of $66k

2 - Sales representative hit the highest target from $46k to $109k from May to Dec .
There was a dip in June ($46k), This may be due to mid of the year and less purchases are made , more promotional sales can be made to improve sales.
The Sales representative hitting the highest target is understandably as they are the ones in the fore front meeting with customers.

3 - Average Sales Amount by Employee
To know the employee bringing in more sales Average gives a better representation than sum because of outliers.
Margaret brings the highest sales
And Steven the least sales

4 - Sales by Continent
North America brings the highest sales, this is visible due to their large population and stable economy , followed by Europe and South America the least.
By Country :
USA topping the chart ($263k)
Germany ($241k)
Austria ($139k)
The least being Poland at $5k.

