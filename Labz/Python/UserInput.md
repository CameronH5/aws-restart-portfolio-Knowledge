# Past Tense Lab Summary: Strings and User Input

In this lab, I continued my Python programming journey by learning about the **string** data type. I progressed from defining basic strings to concatenating them, gathering user input, and utilizing formatted string output. I also practiced the real-world skill of debugging terminal syntax errors.

## Exercise 1: Introducing the String Data Type
In this first exercise, I created a new file called `string-data-type.py`. I assigned the variable `myString` the value `"This is a string."`. I then used the `print()` function to display the value, the `type()` function to confirm it was of class `<class 'str'>`, and the `str()` built-in function to convert the type into a string so I could concatenate it with the rest of my sentence. 

* **What the code did:** 
  * `print(myString)`: Outputs the string.
  * `print(type(myString))`: Outputs the data type.
  * `print(myString + " is of the data type " + str(type(myString)))`: Concatenates the string, a literal text string, and the string representation of the variable's type.

**Real-World Debugging & Spelling Mistakes:** 
When trying to run the file, I made a typo in the terminal by typing `mpython3 string-data-type.py`. This resulted in an error because the `mpython3` command does not exist. I corrected this by retyping the command correctly as `python3 string-data-type.py`.

<img width="1251" height="773" alt="Screenshot 2026-08-31 190722" src="https://github.com/user-attachments/assets/b863b08c-83e1-4950-b1ef-9345f7e72fa6" />


## Exercise 2: String Concatenation
I returned to my script to practice concatenating (combining) two separate strings. I created two variables: `firstString = "water"` and `secondString = "fall"`. I then combined them using the `+` operator to create `thirdString`, and printed the result `waterfall`.

* **What the code did:** The `+` operator, when used on strings, joins them together into a single string.

**Real-World Debugging & Spelling Mistakes:** 
Once again, I accidentally typed `mpython3` when attempting to run the script. The terminal threw an error, prompting me to correct my command to `python3 string-data-type.py` to achieve the correct output.

<img width="1217" height="845" alt="Screenshot 2026-08-31 190842" src="https://github.com/user-attachments/assets/72871c03-50b8-471b-aaea-71d5ddac4f2f" />


## Exercise 3: Getting User Input
Next, I learned how to make my programs interactive by using the `input()` function. I set `name = input("What is your name? ")`, which prompts the user for their name and stores whatever they type as a string variable. I then used `print(name)` to output the entered name (Cameron).

* **What the code did:** The `input()` function pauses the script execution, displays the prompt to the user, waits for them to type on the keyboard, and returns the entered text as a string.

<img width="1241" height="848" alt="Screenshot 2026-08-31 190940" src="https://github.com/user-attachments/assets/e4e26f03-1278-4cc9-8edd-bfc841920568" />


## Exercise 4: String Formatting with `.format()`
In the final exercise, I expanded on user input by creating three separate prompts for `name`, `color`, and `animal`. I then used the `.format()` method to insert these variables into a pre-written sentence structure. 

* **What the code did:** `print("{}, you like a {} {}!".format(name,color,animal))` uses curly braces `{}` as placeholders. The `.format()` method sequentially replaces these placeholders with the values inside the parentheses, resulting in a clean, dynamic sentence: "Cameron, you like a Red Lion!".

<img width="1206" height="846" alt="Screenshot 2026-08-31 191105" src="https://github.com/user-attachments/assets/da52532b-4ee1-4678-afa4-4ce6e1873fb5" />


---
*Note on Debugging:* Throughout the lab, I encountered challenges with terminal command typos (specifically adding an 'm' to `python3`). Correcting these typing errors was an essential part of the process, highlighting how meticulous programmers must be with syntax in both code and command-line execution.
