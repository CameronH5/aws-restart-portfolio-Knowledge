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
