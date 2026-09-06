# Summary: Python Basics and Data Types

In this lab, I began my foundational journey into Python programming. I started by learning how to navigate the Python shell, perform basic arithmetic operations, and then transitioned to writing actual Python scripts. Through these exercises, I explored the core numeric data types (`int`, `float`, `complex`) and the `bool` data type, while learning how to use built-in functions to manipulate and display data.

## Exercise 1: Using the Python Shell
I initiated the Python interactive shell by typing `python3` in the terminal. I practiced entering simple arithmetic commands. 

**Highlighting a Real-World Mistake:** 
While inside the Python shell, I initially mistyped the Linux terminal command `pwd`. Because this is not a Python command, I received a `NameError: name 'pwd' is not defined`. To correct this, I stepped back, realized I was in the wrong environment, and proceeded with the correct Python syntax. I successfully executed mathematical operations such as addition (`2 + 2`), subtraction (`4 - 2`), multiplication (`2 * 2`), and division (`4 / 2`). I noted that division returns a float value (`2.0`). Finally, I exited the shell using the `quit()` function.

<img width="1306" height="781" alt="Screenshot 2026-08-31 182438" src="https://github.com/user-attachments/assets/ca1b063c-883e-41ff-8b86-a13832d3c933" />
<img width="1127" height="173" alt="Screenshot 2026-08-31 182825" src="https://github.com/user-attachments/assets/52d1cd36-5a4b-46f0-932d-7e0229c68b4e" />


## Exercise 2: Introducing the `int` Data Type and Editing Files
I transitioned from the shell to editing text files. I created a file named `hello-world.py` and used the `print()` built-in function to display a string of text to the console. 

I then moved to the `numeric-data.py` file and learned about variables. I created a variable named `myValue` and assigned it the integer value of `1`. I utilized the `print()` function to write the value, and used the `type()` function to check its data type, which returned `<class 'int'>`. To combine text and numbers, I used the `str()` function to convert the integer into a string, allowing me to concatenate it with other text. I ran the file using the command `python3 numeric-data.py`.

<img width="1732" height="830" alt="Screenshot 2026-08-31 183859" src="https://github.com/user-attachments/assets/980198f7-35ba-4197-9bb0-70c2da331a07" />


## Exercise 3: Introducing the `float` Data Type
In this exercise, I introduced a new data type to my code. I updated the `myValue` variable to `3.14`. I ran the `print()` and `type()` functions again, and the terminal confirmed that the value was `3.14` and its data type was `<class 'float'>`. I used the `str()` conversion again to create a descriptive sentence. 

<img width="1540" height="862" alt="Screenshot 2026-08-31 185125" src="https://github.com/user-attachments/assets/d321e4ee-0554-4955-aa37-173a3f7c8910" />


## Exercise 4: Introducing the `complex` Data Type
I then advanced to complex numbers, which are used in higher mathematics. I set `myValue` to `5j`, where `5` is the real number and `j` represents the imaginary unit. I used the `print()` and `type()` functions to confirm my output, which showed the value `5j` and the data type `<class 'complex'>`. 

<img width="1308" height="837" alt="Screenshot 2026-08-31 190056" src="https://github.com/user-attachments/assets/d3bd3d04-9cbd-46a0-8391-ad6c9441b0cf" />



## Exercise 5: Introducing the `bool` Data Type
Finally, I learned about the Boolean data type. I set `myValue` to `True` and then to `False`. I utilized the `type()` function to verify that the data type was `<class 'bool'>`. I discovered that `bool` is implemented as a subset of the `int` data type, often referred to as a "fake data type" in Python. I also used the `print(str(myValue) + " is of the data type " + str(type(myValue)))` command to produce a readable output string for both Boolean values. 

<img width="1276" height="850" alt="Screenshot 2026-08-31 190339" src="https://github.com/user-attachments/assets/6b27c54d-0fc6-4fd9-8cf3-7e50a752a07d" />


---
*Note on Debugging:* During the lab, I also had to pay close attention to syntax, ensuring I used the correct parentheses, quotes, and spacing in my `print` statements (like the `str(type(myValue))` concatenation). Correcting subtle syntax errors like these, alongside the environment mix-up with `pwd`, was a great practice for real-world coding workflows where debugging is an essential part of the process.
```
