# Revenue-Analysis-of-Products-in-North-wind

## Project Overview

This data project aims to provide insights into Sales improvements, which employee is bringing in more sales and Frequent customers. By analysing various aspects of the sales data, we seek to identify trends, dada driven recommendations and gain a deeper understanding of the company's performance.

## Tools

-Power BI - Data cleaning was donee in power query
          - Data analysis was done too.
          - Creating reports .

## Data cleaning / Preparation.

Initial data preparation phase;
- Data loading and inspection.
- Deleting of empty coloumns and removing of columns not needed for analysis.
- Data cleaning and formatting.
  - - Creating of Calendar Date Auto and connecting it to the date column in the data model.

## Exploratory Data ANalysis

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

Color Sales/Target = IF([Total Sales] >= [Sales Target], "Green", "Red")

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


KPI Total Sales = FORMAT([Total Sales], "$#,0") & " "

KPI Total Sales Vs Budget = 
                Var var1 = [Total Budget]
                var percentage = DIVIDE([Total Sales], [Total Budget]) - 1
                Var sign = IF([Total Sales] >= var1, "+", "")
                Var ret = sign & FORMAT(percentage, "#0.0%") & " | " & 
                        FORMAT(var1, "#0,#")
            Return ret
                          

KPI Total Sales Vs Last Month = 
                Var var1 = [Previous Month Sales]
                var percentage = DIVIDE([Total Sales], [Previous Month Sales]) - 1
                Var sign = SWITCH(TRUE(),
                                        [Total Sales] >= var1, "+",
                                          "")
                Var ret = sign & FORMAT(percentage, "#0.0%") & " | " & 
                        FORMAT(var1, "#0,#")
            Return ret


KPI Total Sales Vs Last Year = 
                Var var1 = [SPLY Sales]
                var percentage = DIVIDE([Total Sales], [SPLY Sales]) - 1
                Var sign = IF([Total Sales] >= var1, "+", "")
                Var ret = sign & FORMAT(percentage, "#0.0%") & " | " & 
                        FORMAT(var1, "#0,#")
            Return ret


KPI Total Sales Vs Tartget = 
                Var var1 = [Sales Target]
                var percentage = DIVIDE([Total Sales], [Sales Target]) - 1
                Var sign = IF([Total Sales] >= var1, "+", "")
                Var ret = sign & FORMAT(percentage, "#0.0%") & " | " & 
                        FORMAT(var1, "#0,#")
            Return ret


Monthly % Share = DIVIDE([Total Sales],[Total Sales For The Year], 0)


Previous Month Sales = CALCULATE([Total Sales], PREVIOUSMONTH(OrderCalendar[Date]))


Sales Target = SWITCH(TRUE(),
                            [Total Sales] >= 100000 && [Total Sales] <= 200000, [Total Sales]/2,
                            [Total Sales] < 1000000, [Total Sales]+20000,
                            [Total Sales] > 200000, [Total Sales]-5000)
                        
                      


SPLY Sales = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(OrderCalendar[Date]))


Total Budget = SWITCH(TRUE(),
                            [Total Sales] >= 100000 && [Total Sales] <= 200000, [Total Sales]*2,
                            [Total Sales] < 1000000, [Total Sales]+50000,
                            [Total Sales] > 200000, [Total Sales]+10000)


Total Sales = SUM(Orders[SalesAmount])


Total Sales For The Year = CALCULATE([Total Sales], REMOVEFILTERS(OrderCalendar[MonthNumber], OrderCalendar[OrderMonth]))


Total Sales YTD = CALCULATE([Total Sales], DATESYTD(OrderCalendar[Date]))


Variance = 
        Var Vari = DIVIDE([Total Sales], [Previous Month Sales]) - 1
        Var Ret = FORMAT(Vari, "#%") & " " & IF(Vari > 0, "(+)", "(-)")
     Return Ret   





