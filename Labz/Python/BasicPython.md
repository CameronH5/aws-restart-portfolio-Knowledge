# Lab Summary: Python Fundamentals – Data Types, Collections, and File I/O

## Overview
In this multi‑part lab, I worked through a series of exercises to build a solid foundation in Python programming. I started with the interactive Python shell, moved on to creating and running Python scripts, explored fundamental data types (integers, floats, complex numbers, booleans, and strings), worked with collections (lists, tuples, and dictionaries), and finally processed a real‑world CSV file using composite data structures. Throughout the process, I encountered and corrected several typical mistakes – from typos in commands to incorrect filenames – which reinforced the importance of attention to detail in coding.

---

## Exercise 1: Using the Python Shell
I began by launching the Python interactive shell with the command `python3`. At the `>>>` prompt, I experimented with basic arithmetic operations:
- `2 + 2` returned `4`
- `4 - 2` returned `2`
- `2 * 2` returned `4`
- `4 / 2` returned `2.0` (note that division always yields a float)

I also inadvertently typed `pwd` (a shell command) inside the Python interpreter, which raised a `NameError` – a reminder that the shell expects Python syntax, not bash commands. I exited the shell using `quit()`.

---

## Exercise 2: Introducing the `int` Data Type
I created a Python file named `numeric-data.py` (originally I had a file `hello-world.py` that printed `"Hello, World"`). In this script, I:

- Printed a message: `print("Python has three numeric types: int, float, and complex")`
- Defined a variable `myValue = 1`
- Printed its value: `print(myValue)`
- Used `type(myValue)` to display `<class 'int'>`
- Converted the integer to a string with `str()` and concatenated it with a descriptive message.

I ran the script with `python3 numeric-data.py` and verified the output. I also noticed that in the earlier hello‑world script I had a missing space after `print` – I corrected it to `print("Hello, World")` to avoid a syntax error.

---

## Exercise 3: Introducing the `float` Data Type
To store decimal numbers, I added a new assignment: `myValue = 3.14`. I then repeated the same pattern – printing the value, its type (`<class 'float'>`), and a combined string. When I ran the script again, the output now included both the integer and the float sections, clearly showing the difference between the two data types.

---

## Exercise 4: Introducing the `complex` Data Type
For complex numbers (used in advanced mathematics), I assigned `myValue = 5j` (the imaginary unit). The `type()` function returned `<class 'complex'>`. I again printed the value and a descriptive message. Running the script added the complex number output to the previous results, demonstrating how the same variable name can hold different types over time (though in practice it’s better to use distinct names).

---

## Exercise 5: Introducing the `bool` Data Type
I then explored Boolean values. I set `myValue = True` and later `myValue = False`. Each time I printed the value, its type (`<class 'bool'>`), and a formatted message. The output confirmed that `True` and `False` are indeed of type `bool`. Interestingly, I noticed that `True` is represented as `1` and `False` as `0` internally, which aligns with Python’s implementation.

---

## Exercise 6: Introducing the `string` Data Type
I created a new file `string-data-type.py`. In this script:

- I defined `myString = "This is a string."` and printed it.
- I printed its type: `<class 'str'>`.
- I concatenated `myString` with its type using `str(type(myString))`.

Next, I demonstrated string concatenation by combining `"water"` and `"fall"` into `"waterfall"` and printed it.

I then used the `input()` function to ask for the user’s name, favorite color, and favorite animal. Finally, I used the `.format()` method to insert these values into a sentence: `"{}, you like a {} {}!".format(name, color, animal)`. However, I initially made a typo in the format string – I wrote `{{}}` incorrectly – but after reviewing the code, I corrected it to `{}` for each placeholder. When I ran the script, it prompted me for input and printed a personalised message, e.g., `Cameron, you like a Red Lion!`.

During this exercise, I also mistakenly typed `mython3` instead of `python3` when trying to run the script – the shell returned `command not found`, so I quickly corrected it to `python3`.

---

## Exercise 7: Collections – List, Tuple, and Dictionary
I created a new file `collections.py` to experiment with three collection data types.

### List
- I defined a list: `myFruitList = ["apple", "banana", "cherry"]`
- Printed the list and its type (`<class 'list'>`).
- Accessed individual elements by index (`[0]`, `[1]`, `[2]`) and printed them.
- Changed the third element from `"cherry"` to `"orange"` with `myFruitList[2] = "orange"` and printed the updated list – proving that lists are mutable.

### Tuple
- I created a tuple: `myFinalAnswerTuple = ("apple", "banana", "pineapple")`
- Printed the tuple and its type (`<class 'tuple'>`).
- Accessed its elements by index – tuples are immutable, so I could not change them, but I could read them.

### Dictionary
- I defined a dictionary mapping names to favourite fruits:
  ```python
  myFavoriteFruitDictionary = {
      "Akua": "apple",
      "Saanvi": "banana",
      "Paulo": "pineapple"
  }


# Data Types, Structures, and File Handling

Throughout this lab, I completed a hands-on introduction to Python programming. I started by learning foundational concepts like variables and basic data types, then progressed to more complex structures such as lists, tuples, dictionaries, and eventually file handling. Here is a descriptive summary of the specific tasks I performed.

## Exercise 1: Mastering String Data Types and User Input
I began by creating a simple string variable. I used the `print()` function to display "This is a string," and then used the `type()` function to confirm its data type was `<class 'str'>`. To concatenate the sentence, I learned how to convert the return value of `type()` into a string using the `str()` function.

I then practiced string concatenation by combining `firstString = "water"` and `secondString = "fall"` to create "waterfall". Next, I explored user interaction by using the `input()` function to ask the user for their name, favorite color, and favorite animal. Finally, I used the `.format()` method to create a dynamic, formatted sentence that read: "Cameron, you like a Red Lion!".

## Exercise 2: Working with Lists and Tuples
In the `collections.py` file, I defined a list called `myFruitList` containing "apple", "banana", and "cherry". I printed the list to verify its contents and used `type()` to confirm it was a list. 

I practiced **indexing** (which starts at 0) to access individual items: `myFruitList[0]`, `[1]`, and `[2]`. I then demonstrated that **lists are mutable** by changing the value at index 2 from "cherry" to "orange". 

Afterwards, I learned about **tuples**, which are immutable (cannot be changed). I created a tuple called `myFinalAnswerTuple` using parentheses and accessed its items by position, just like a list.

## Exercise 3: Exploring Dictionaries
In this exercise, I defined a dictionary called `myFavoriteFruitDictionary`. Unlike lists and tuples which use indices, dictionaries use named keys. I mapped names like "Akua" to "apple", "Saanvi" to "banana", and "Paulo" to "pineapple". 

I printed the entire dictionary to see its contents, used `type()` to confirm it was a dictionary (`<class 'dict'>`), and then accessed specific values by passing the keys (e.g., `["Akua"]`, `["Saanvi"]`, `["Paulo"]`) into the brackets.

## Categorizing Mixed Data Types
I created a script to test mixed data types. I defined a list called `myMixedTypeList` that contained an integer, another integer, a float, a boolean (`True`), and strings. I then used a `for` loop to iterate over the list, using `.format()` to print the value and its corresponding type for every item.

**The Real-World Debugging Experience:**
During this exercise, I encountered a common real-world problem. I had named the file `categorize-values.py`, but when I tried to run it in the terminal, I mistakenly typed `python3 categorize-value.py` (forgetting the 's' at the end). This resulted in a `[Errno 2] No such file or directory` error. I successfully debugged this by looking at my file explorer, realizing my typo, and running the correct command `python3 categorize-values.py`, which then executed perfectly.

## Final Task: Creating a Car Inventory with Composite Data
In the final, more advanced exercise, I created a script to read and process tabular data from a CSV file (`car_fleet.csv`). 

I first imported the `csv` and `copy` modules. I defined a dictionary called `myVehicle` to act as a template for the data, with keys for the VIN, make, model, year, range, top speed, zero-to-sixty, and mileage. 

I used a `with open(...)` statement to handle the file safely. By using `csv.reader()`, I was able to loop through the CSV data. I implemented an `if-else` statement to correctly skip the header row and process the data rows. 

The most crucial part of this lab was using `copy.deepcopy()`. I learned that without this, Python would create a shallow copy pointing to the same memory location, causing all imported cars to overwrite each other. By using a deep copy, I ensured that each car's data was stored uniquely in a separate memory box within `myInventoryList`. Finally, I used a nested `for` loop to iterate through the inventory and print out all the key-value pairs for each car, separated by dashed lines.

**Conclusion:** This lab successfully took me from basic string manipulation all the way to complex composite data structures and file input/output, solidifying my foundational knowledge of Python.
```
