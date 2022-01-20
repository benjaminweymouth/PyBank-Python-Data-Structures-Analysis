# Python For Finance: Methods and Data Structures
# PyBank and PyRamen Examples

The purpose of this code is to demonstrate Python proficiency in working with CSV files, lists, dictionaries, loops and particulary nested data structures. 

The code below is a Python script for analyzing the financial records of a fictional company. I utilize a dataset: budget_data.csv. This dataset is composed of two columns, Date and Profit/Losses. This code originally appeared in a Jupyter notebook on Github. 

The script will utilze the following actions / methods:  

- The total number of months included in the dataset.


- The net total amount of Profit/Losses over the entire period.


- The average of the changes in Profit/Losses over the entire period.


- The greatest increase in profits (date and amount) over the entire period.


- The greatest decrease in losses (date and amount) over the entire period.

```python
#import required libraries 
import csv
from pathlib import Path
import numpy as np 
```


```python
# set up the file path 
# importing the financial dataset 
csvpath = Path('./Resources/budget_data.csv')
csvpath
```




    WindowsPath('Resources/budget_data.csv')




```python
# Initialize list to hold financial date data  
dates = []

#list holding only the financial profits and losses 
profit_losses = []
```


```python
# use open method to open csvpath 
with open(csvpath, 'r') as csvfile:

    # check datatype 
    print(type(csvfile))

    # set up csv.reader, specify a delimiter 
    csvreader = csv.reader(csvfile, delimiter=',')
    # check csv datatype 
    print(type(csvreader))
        
    # skip over header
    header = next(csvreader)
        
    #read in data into a list object 
    for row in csvreader:
        # date variable equal to date row values 
         
        date = row[0]
        # Append the row date value to the list of dates
        dates.append(date)
        #set profit_loss value and casting to an integer 
        profit_loss = int(row[1])
        profit_losses.append(profit_loss)      
    
```

    <class '_io.TextIOWrapper'>
    <class '_csv.reader'>
    ['Date', 'Profit/Losses']
    


```python
#initialize Variables for total and count 

total_profit_losses = 0
count_profit_losses = 0

#use profit_losses list to find total and count 

for profit_loss in profit_losses:

    # Sum the total and count variables
    total_profit_losses += profit_loss
    count_profit_losses += 1

#for testing purposes, printing values of variables 
print(total_profit_losses)  
print(count_profit_losses)
 
```

    38382578
    86
    


```python
#Find the total number of months included in the dataset.
#using the built in "len" method 
total_months = len(dates)

total_months
```




    86




```python
#using Numpy diff() method to compute all changes in profit/losses
#Link for reference purposes: https://numpy.org/doc/stable/reference/generated/numpy.diff.html

differences  = np.diff(profit_losses)

#the numpy array "differences" now represents all changes in profit/losses
differences
```




    array([  116771,  -662642,  -391430,   379920,   212354,   510239,
            -428211,  -821271,   693918,   416278,  -974163,   860159,
           -1115009,  1033048,    95318,  -308093,    99052,  -521393,
             605450,   231727,   -65187,  -702716,   177975, -1065544,
            1926159,  -917805,   898730,  -334262,  -246499,   -64055,
           -1529236,  1497596,   304914,  -635801,   398319,  -183161,
             -37864,  -253689,   403655,    94168,   306877,   -83000,
             210462, -2196167,  1465222,  -956983,  1838447,  -468003,
             -64602,   206242,  -242155,  -449079,   315198,   241099,
             111540,   365942,  -219310,  -368665,   409837,   151210,
            -110244,  -341938, -1212159,   683246,   -70825,   335594,
             417334,  -272194,  -236462,   657432,  -211262,  -128237,
           -1750387,   925441,   932089,  -311434,   267252, -1876758,
            1733696,   198551,  -665765,   693229,  -734926,    77242,
             532869])




```python
#Using max method for the greatest increase in profits over the entire period.
greatest_increase_profits = max(differences)
greatest_increase_profits

```




    1926159




```python
#Using min method for the greatest decrease in profits over the entire period.
greatest_decrease_profits = min(differences)
greatest_decrease_profits
```




    -2196167




```python
#Using the Numpy average method for the average change in profits over the entire period.
average_change = np.average(differences)
average_change
```




    -2315.1176470588234




```python
#this is to match the inc/decrease index with the correct month value 

#use where() method to find index of greatest increase & decrease
max_index = np.where(differences == greatest_increase_profits)
min_index = np.where(differences == greatest_decrease_profits)

#compare dates and differences array. then make new variable to hold difference
if (len(differences)) != (len(dates)): 
    length_diff = np.absolute(len(differences) - len(dates))

#to account for the header, we adjust for that difference to the index. 
adjusted_max = int(max_index[0]) + length_diff
adjusted_min = int(min_index[0]) + length_diff

#set the index for dates as either adjusted max/min with the difference added
max_date = dates[adjusted_max]
min_date = dates[adjusted_min]
```


```python
#print the analysis to the terminal 

print("Financial Analysis")
print("----------------------------")
print(f"Total Months: {total_months}")
print(f"Total: ${total_profit_losses}")
print(f"Average  Change: ${round(average_change, 2)}")
print(f"Greatest Increase in Profits: {max_date} (${greatest_increase_profits})")
print(f"Greatest Decrease in Profits: {min_date} (${greatest_decrease_profits})")
```

    Financial Analysis
    ----------------------------
    Total Months: 86
    Total: $38382578
    Average  Change: $-2315.12
    Greatest Increase in Profits: Feb-2012 ($1926159)
    Greatest Decrease in Profits: Sep-2013 ($-2196167)
    


```python
# export a text file with the results.
output_file = Path('./Resources/output.txt')

# use file.write to output to the file. 
with open(output_file, 'w') as file:
    file.write("Financial Analysis\n")
    file.write("----------------------------\n")
    file.write(f"Total Months: {total_months}\n")
    file.write(f"Total: ${total_profit_losses}\n")
    file.write(f"Average  Change: ${round(average_change, 2)}\n")
    file.write(f"Greatest Increase in Profits: {max_date} (${greatest_increase_profits})\n")
    file.write(f"Greatest Decrease in Profits: {min_date} (${greatest_decrease_profits})")
```

