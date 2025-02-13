Time Intelligence

DateTable = 
ADDCOLUMNS( 
CALENDAR(DATE(2023, 1, 1), DATE(2024, 12, 31)), 
"Year", YEAR([Date]), 
"Month", MONTH([Date]), 
"Quarter", QUARTER([Date]), 
"MonthName", FORMAT([Date], "MMMM") 
)

Measure 2 = 
TOTALYTD(SUM(Sample_Sales_Data[Sales Amount]), DateTable[Date])

Sales MTD = 
TOTALMTD(SUM(Sample_Sales_Data[Sales Amount]), DateTable[Date]) 

Sales QTD = 
TOTALQTD(SUM(Sample_Sales_Data[Sales Amount]), DateTable[Date])

Sales Previous Year = 
CALCULATE(SUM(Sample_Sales_Data[Sales Amount]), 
SAMEPERIODLASTYEAR(DateTable[Date]))

YoY Growth = 
DIVIDE([Sales YTD] - [Sales Previous Year], [Sales Previous Year], 0)

Sales Last Month = 
CALCULATE(SUM(Sales[SalesAmount]), DATEADD(DateTable[Date], -1, MONTH))

Sales Two Years Ago = 
CALCULATE(SUM(Sales[SalesAmount]), PARALLELPERIOD(DateTable[Date], -2, YEAR))


Sales Last 3 Months = 
CALCULATE(SUM(Sales[SalesAmount]), 
DATESINPERIOD(DateTable[Date], 
LASTDATE(DateTable[Date]), -3, MONTH))

/////////////////////////////////////////////////////
One dimensional data
import pandas as pd 
import numpy as np 
data = pd.DataFrame({'values': [10, 20, 30, 40, 50]}) 
data['Addition'] = data['values'] + 5 
data['Multiplication'] = data['values'] * 213 
data['Subtraction'] = data['values'] - 10 
data['Division'] = data['values'] / 5 
defsquare(x): 
return x * x 
data['Squared'] = data['values'].apply(square) 
statistics = { 
'Sum': [data['values'].sum()], 
'Mean': [data['values'].mean()], 
'Max': [data['values'].max()], 
'Min': [data['values'].min()] 
} 
stats_df = pd.DataFrame(statistics) 
filtered_data = data[data['values'] > 25] 
data_with_nan = pd.DataFrame({'values': [10, 20, np.nan, 40, 50]}) 
data_with_nan['Is_NaN'] = data_with_nan['values'].isna() 
data_with_nan['Filled'] = data_with_nan['values'].fillna(0) 
data['Sorted_Ascending'] = data['values'].sort_values().reset_index(drop=True) 
data['Sorted_Descending'] = data['values'].sort_values(ascending=False).reset_index(drop=True) 
output = pd.concat([data, stats_df], axis=1)

/////////////////////////////////////////////////
Reshaping hierarchical data
import pandas as pd 
import numpy as np 
Create a MultiIndex DataFrame (Hierarchical Data) 
arrays = [ 
['A', 'A', 'A', 'B', 'B', 'B'], 
['X', 'Y', 'Z', 'X', 'Y', 'Z'] 
] 
index = pd.MultiIndex.from_arrays(arrays, names=('Letter', 'Symbol')) 
data = pd.DataFrame({ 
'Value1': [10, 20, 30, 40, 50, 60],15 
'Value2': [15, 25, 35, 45, 55, 65] 
}, index=index) 
print("Original DataFrame (Hierarchical Data):") 
print(data) 
print("\n") 
Stack Operation 
 Stack the DataFrame (Convert Columns to Rows) 
stacked = data.stack() 
print("Stacked DataFrame:") 
print(stacked) 
print("\n") 
 
Unstack Operation 
 Unstack the DataFrame (Convert Rows to Columns) 
unstacked = stacked.unstack() 
print("Unstacked DataFrame:") 
print(unstacked) 
print("\n") 

Pivoting Data 
Create DataFrame for Pivoting 
data_for_pivot = pd.DataFrame({ 
'Date': ['2024-01-01', '2024-01-01', '2024-01-02', '2024-01-02'], 
'City': ['New York', 'Los Angeles', 'New York', 'Los Angeles'], 
'Temperature': [32, 75, 30, 77] 
}) 
print("Original Data for Pivoting:") 
print(data_for_pivot) 
print("\n") 
 Pivot the DataFrame to make cities as columns and dates as rows 
pivoted_data = data_for_pivot.pivot(index='Date', columns='City', values='Temperature') 
print("Pivoted DataFrame (Cities as Columns):") 
print(pivoted_data) 
print("\n") 

Pivot Table Data 
 Create DataFrame for Pivot Table 
data_for_pivot_table = pd.DataFrame({ 
'Date': ['2024-01-01', '2024-01-01', '2024-01-02', '2024-01-02'], 
'City': ['New York', 'Los Angeles', 'New York', 'Los Angeles'], 
'Temperature': [32, 75, 30, 77], 
'Humidity': [80, 20, 85, 18] 
})16 
print("Original Data for Pivot Table:") 
print(data_for_pivot_table) 
print("\n") 
 Pivot Table to calculate the average temperature and humidity per city and date 
pivot_table_data = data_for_pivot_table.pivot_table( 
index='Date', 
columns='City', 
values=['Temperature', 'Humidity'], 
aggfunc=np.mean 
) 
print("Pivot Table DataFrame (Average Temperature and Humidity):") 
print(pivot_table_data)


///////////////////////////
Tableau Calculated field
Sales growth
(SUM([Current Year Sales]) - SUM([Previous Year Sales])) / SUM([Previous Year Sales]) 
(SUM([Sales]) - LOOKUP(SUM([Sales]), -1)) / LOOKUP(SUM([Sales]), -1)

Category performance
IF SUM([Sales]) > 100000 THEN "High" 
ELSEIF SUM([Sales]) > 50000 THEN "Medium" 
ELSE "Low" 
END

Sales threshold
IF SUM([Sales]) > [Sales Threshold] THEN "Above Threshold" 
ELSE "Below Threshold" 
END
