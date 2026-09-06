# Lab Summary: Conditional Logic (if, else, elif)

In this lab, I continued my Python programming journey by learning how to make decisions in code using conditional statements. I created a script called `conditionals.py` that interacted with the user via the `input()` function and used `if`, `else`, and `elif` blocks to execute different blocks of code based on the user's responses. 

## Exercise 1: Working with the `if` statement
I started by writing a basic shipping program. I used the `input()` function to ask the user, "Do you need to ship a package? (Enter yes or no)" and stored the answer in a variable named `userReply`. I then learned about the comparative operator `==` (which means "is equal to"). I used an `if` statement to check if `userReply` was equal to the string `"yes"`. If it was, the indented code block underneath printed, "We can help you ship that package!". 

I ran the program multiple times, entering "yes" and "no" to observe how the program behaved when the condition was not met (it simply exited without printing anything).

**Highlighting Real-World Mistakes:**
When writing my `if` statement, I initially accidentally typed a single equals sign (`=`) instead of the double equals sign (`==`). This caused a syntax error because `=` is used for assignment, not comparison. I also caught myself missing the colon `:` at the end of the `if` statement and had to correct it to get the code to run.

<img width="473" height="148" alt="Screenshot 2026-09-06 205331" src="https://github.com/user-attachments/assets/d9171ac8-3591-4165-bb26-715746d9c0fd" />


## Exercise 2: Working with the `else` statement
To improve user experience, I added an `else` statement. This allowed the program to respond even when the user did not need shipping. If the `if` condition was false (the user entered anything other than "yes"), the program would execute the `else` block, printing, "Please come back when you need to ship a package. Thank you."

This exercise taught me that indentation in Python is crucial, as it defines which block of code belongs to which statement.

<img width="1121" height="180" alt="Screenshot 2026-09-06 205343" src="https://github.com/user-attachments/assets/d8830fa9-b2a5-4eff-b63b-a50e28e1af8d" />


## Exercise 3: Working with the `elif` statement
I expanded the program to handle multiple options using the `elif` statement (short for "else if"). I added a new prompt asking the user, "Would you like to buy stamps, buy an envelope, or make a copy?". 

If the user typed "stamps", it printed a specific message. If they typed "envelope", it printed another. I then introduced logic for "copy" where I used a nested `input()` function to ask "How many copies would you like?". I converted that input to an integer and used `.format()` to print the final count.

I ran the script multiple times testing "stamps", "envelope", "copy", and "no" to verify that the `if`, `elif`, and `else` blocks were correctly stopping the program once a true condition was found.

**Highlighting Real-World Mistakes:**
I had to be very careful with spelling in this exercise. I misspelled the word "stamps" in one of my `elif` conditions, which meant the program skipped it even when I typed "stamps" correctly in the terminal. I had to go back and fix the typo in my code to make it match the user input perfectly.

<img width="1065" height="857" alt="Screenshot 2026-09-06 205608" src="https://github.com/user-attachments/assets/ad6635de-5a7b-41ab-b379-0b8d154d70aa" />


---
*Note on Debugging:* Throughout this lab, I learned that Python relies heavily on strict syntax. Making simple mistakes like using the wrong operator (`=` instead of `==`), missing a colon, or misspelling string values in conditions completely breaks the logic of the program. Correcting these typos and syntax errors is a normal and essential part of the real-world coding process.
```
