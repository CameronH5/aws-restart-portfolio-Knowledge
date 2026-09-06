# Lab Summary: Composite Data Types and CSV File Handling

In this lab, I advanced my Python skills by working with composite data types and file I/O. I started by iterating through a list of mixed data types, then moved on to creating and reading a CSV file. I learned how to use the `csv` and `copy` modules to read tabular data, create in-memory dictionary objects, and store them in a list for later processing.

## Exercise 1: Categorizing Values in a Mixed List
In this exercise, I created a new file called `categorize-values.py`. I defined a list called `myMixedTypeList` containing different data types: an integer (`45`), a large integer (`290578`), a float (`1.02`), a boolean (`True`), a string (`"My dog is on the bed."`), and a string representation of a number (`"45"`). I used a `for` loop to iterate through the list. Inside the loop, I used the `.format()` method to print the value and its corresponding `type()`.

*   **What the code did:** `for item in myMixedTypeList:` iterates over each element. `print("{} is of the data type {}".format(item,type(item)))` dynamically formats a string showing the value and its class type.

**Real-World Debugging and Spelling Mistakes:** 
When running the script in the terminal, I accidentally typed `python3 categorize-value.py` (missing the 's' at the end of "values"). The terminal threw an `[Errno 2] No such file or directory` error. I realized my spelling mistake, corrected the command to `python3 categorize-values.py`, and successfully ran the program, confirming the correct data types for each item.

> **[Screenshot Placeholder 1: Insert Image 1 here showing the code for `categorize-values.py`, the terminal error for `categorize-value.py`, and the successful output of the corrected command]**

## Exercise 2: Creating a Car Inventory CSV File
I created a new file named `car_fleet.csv`. I saved a header row followed by four rows of data. This file served as the tabular data source for the rest of the lab. The header included `vin,make,model,year,range,topSpeed,zeroSixty,mileage`.

*   **What the code did:** This is a Comma-Separated Values file, which acts as a spreadsheet-like structure for storing data.

> **[Screenshot Placeholder 2: Insert Image 2 here showing the raw CSV data in the editor]**
> **[Screenshot Placeholder 3: Insert Image 3 here showing the instructions for creating the CSV file]**

## Exercise 3: Importing Modules and Defining a Dictionary
I created a new Python script called `composite2.py`. First, I imported the `csv` module (for reading the file) and the `copy` module (for deep copying). I then defined a dictionary named `myVehicle` with keys for all the columns in my CSV file, setting default values like `"empty"` for strings and `0` for integers. I used a `for` loop with the `.items()` function to print the initial key-value pairs of this dictionary.

*   **What the code did:** `import csv` and `import copy` bring in the necessary libraries. `for key, value in myVehicle.items():` traverses the dictionary and `print("{} : {}".format(key,value))` prints the keys and their initial values.

> **[Screenshot Placeholder 4: Insert Image 4 here showing the instructions for importing modules and defining the dictionary]**
> **[Screenshot Placeholder 5: Insert Image 5 here showing the code for the `myVehicle` dictionary and the initial `for` loop]**

## Exercise 4: Copying CSV Data into Memory
I used the `with open("car_fleet.csv") as csvFile:` syntax to automatically open and close the file. I used `csv.reader(csvFile, delimiter=',')` to parse the data. I initialized a `lineCount` variable to 0 and created an empty list called `myInventoryList`. 

Inside the `for row in csvReader:` loop, I used an `if/else` block. The `if` block printed the column names for the first line. The `else` block processed the data rows. Crucially, I used `currentVehicle = copy.deepcopy(myVehicle)` to create a completely independent copy of the dictionary, preventing the program from storing the same memory location multiple times. I then mapped the CSV row data to the appropriate dictionary keys (e.g., `currentVehicle["vin"] = row[0]`) and appended this dictionary to my list. I used an f-string to print the number of lines processed.

*   **What the code did:** The `with` statement handles file management. `csv.reader` converts each line into a list of strings. The `copy.deepcopy` function creates a new storage location for each vehicle, ensuring `myInventoryList` holds distinct records for every car.

> **[Screenshot Placeholder 6: Insert Image 6 here showing the instructions for copying CSV data into memory]**
> **[Screenshot Placeholder 7: Insert Image 7 here showing the detailed code for the file reading logic and deep copy]**

## Exercise 5: Printing the Car Inventory
In the final step, I completed the script by iterating through `myInventoryList`. I used a nested loop: the outer loop (`for myCarProperties in myInventoryList:`) iterates through each vehicle dictionary, and the inner loop (`for key, value in myCarProperties.items():`) iterates through the properties of that specific vehicle. I printed each key and value, followed by a dashed line separator (`"-------"`) to neatly separate each car record.

*   **What the code did:** It formatted the output of the in-memory list into a readable inventory.

> **[Screenshot Placeholder 8: Insert Image 8 here showing the code for printing the inventory]**
> **[Screenshot Placeholder 9: Insert Image 9 here showing the terminal output of the full program, displaying all cars from the CSV]**
> **[Screenshot Placeholder 10: Insert Image 10 here showing the instructions for printing the car inventory]**

---
*Note on Debugging:* The mistake I made earlier in the lab (typing `categorize-value.py` instead of `categorize-values.py`) was an excellent reminder of how important precise filenames are in programming. I also paid close attention to variable names and dictionary keys to ensure they matched perfectly between the CSV header and my Python code.
```
