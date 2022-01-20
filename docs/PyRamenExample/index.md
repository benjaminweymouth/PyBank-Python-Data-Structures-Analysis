# PyRamen Example: Methods / Nested Data Structures

## Code Explanation & Dataset
The purpose of this code is to analyze a business's financial performance by cross-referencing their sales data with internal product data to figure out revenues and costs for the year. 

Thus, two different datasets will be utilized to perform the following actions / methods:

- Include a calculation of the total number of months in the dataset. 
- Calculate the net total amount of Profit/Losses over the entire period. 
- Calculate the average of the changes in Profit/Losses over the entire period. 
- Calculate the greatest increase in Profits over the entire period (Date and Amount). 
- Calculate the greatest decrease in Losses over the entire period (Date and Amount).
- Print the analysis and export the analysis to a text file that contains the final results. 

 
 ```python
#PyRamen - Python Homework 1 - Benjamin Weymouth 
```


```python
#import required libraries 
import csv
from pathlib import Path
```


```python
# set up the file path for menu and check it 
menu_filepath = Path('./Resources/menu_data.csv')
menu_filepath
```




    WindowsPath('Resources/menu_data.csv')




```python
# set up the file path for sales and check it 
sales_filepath = Path('./Resources/sales_data.csv')
sales_filepath
```




    WindowsPath('Resources/sales_data.csv')




```python
#list objects for menus and sales data 
menus = []
sales = []
```


```python
# Read in the menu data into the menu list

with open(menu_filepath, 'r') as csvfile:

    # check type
    print(type(csvfile))

    # set up csv.reader, specify a delimiter 
    csvreader = csv.reader(csvfile, delimiter=',')
    # check csv datatype 
    print(type(csvreader))
    
      # skip over header
    header = next(csvreader)
        
    #read in menu data into a list object 
    for row in csvreader:
        # menu variable equal to menu row values 
        menu = row[0:5]
        # Append the row menu value to the list of menus
        menus.append(menu)
```

    <class '_io.TextIOWrapper'>
    <class '_csv.reader'>
    


```python
#print total number of records in menu data
#should be 32 because we skipped the header   

print(len(menus))
```

    32
    


```python
# Read in the sales data into the sales list
# use open method to open sales_filepath

with open(sales_filepath, 'r') as csvfile:

    # check type
    print(type(csvfile))

    # set up csv.reader, specify a delimiter 
    csvreader = csv.reader(csvfile, delimiter=',')
    
    # check csv datatype 
    print(type(csvreader))
    
    # skip over header
    header = next(csvreader)
        
    #read in data into a list object 
    for row in csvreader:
        # sale variable equal to sale row values 
         
        sale = row[0:5]
        # Append the row sale value to the list of sales
        sales.append(sale)
```

    <class '_io.TextIOWrapper'>
    <class '_csv.reader'>
    


```python
# Initialize dict object to hold our key-value pairs of items and metrics
report = {}
report
```




    {}




```python
#list for data that doesnt match 
no_match = []

#loop through the sales list, check if the menu_item column exists in the dictionary called report 

for row in sales:
    #set sales data metrics, specify row and cast quantity to integer
    menu_item = row[4]
    quantity = int(row[3])
    
    #check for item, if not already there add initialized metrics
    if menu_item not in report:
        report[menu_item] = { "01-count": 0, "02-revenue": 0, "03-cogs": 0, "04-profit": 0,}
    for i in menus: 
        item = i[0]
        price = float(i[3])
        cost = float(i[4])
        profit = (price - cost) 
        #if sales item matches menu item, track metrics
        if item == menu_item:
            report[menu_item]["01-count"] += quantity
            report[menu_item]["02-revenue"] += price * quantity
            report[menu_item]["03-cogs"] += cost * quantity
            report[menu_item]["04-profit"] += profit * quantity
        #if sales item does not match, append message to list 
        elif item != menu_item: 
            no_match.append(f"{menu_item} does not equal {item}! NO MATCH!")
```


```python
#Print out the matching Data with the metrics (will also be written to output.txt file)
print(report) 

#for testing purposes: prints content of list that holds "no matches"
#print(no_match)
```

    {'spicy miso ramen': {'01-count': 9238, '02-revenue': 110856.0, '03-cogs': 46190.0, '04-profit': 64666.0}, 'tori paitan ramen': {'01-count': 9156, '02-revenue': 119028.0, '03-cogs': 54936.0, '04-profit': 64092.0}, 'truffle butter ramen': {'01-count': 8982, '02-revenue': 125748.0, '03-cogs': 62874.0, '04-profit': 62874.0}, 'tonkotsu ramen': {'01-count': 9288, '02-revenue': 120744.0, '03-cogs': 55728.0, '04-profit': 65016.0}, 'vegetarian spicy miso': {'01-count': 9216, '02-revenue': 110592.0, '03-cogs': 46080.0, '04-profit': 64512.0}, 'shio ramen': {'01-count': 9180, '02-revenue': 100980.0, '03-cogs': 45900.0, '04-profit': 55080.0}, 'miso crab ramen': {'01-count': 8890, '02-revenue': 106680.0, '03-cogs': 53340.0, '04-profit': 53340.0}, 'nagomi shoyu': {'01-count': 9132, '02-revenue': 100452.0, '03-cogs': 45660.0, '04-profit': 54792.0}, 'soft-shell miso crab ramen': {'01-count': 9130, '02-revenue': 127820.0, '03-cogs': 63910.0, '04-profit': 63910.0}, 'burnt garlic tonkotsu ramen': {'01-count': 9070, '02-revenue': 126980.0, '03-cogs': 54420.0, '04-profit': 72560.0}, 'vegetarian curry + king trumpet mushroom ramen': {'01-count': 8824, '02-revenue': 114712.0, '03-cogs': 61768.0, '04-profit': 52944.0}}
    


```python
#print total number of records in sales data
#should be 74125 minus the header so 74124 rows. 

print(len(sales))
```

    74124
    


```python
#write the output to a txt file 
output_file = Path('./Resources/output.txt')

output_file
```




    WindowsPath('Resources/output.txt')




```python
# use file.write to output to the file. 

with open(output_file, 'w') as file:
     for key, value in report.items(): 
        file.write('%s:%s\n' % (key, value))
```
