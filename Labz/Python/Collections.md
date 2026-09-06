# Lab Summary: Exploring Python Collections (Lists, Tuples, and Dictionaries)

In this lab, I advanced my Python programming skills by learning about collection data types. I created a new file called `collections.py` and explored three different ways to store data: **Lists**, **Tuples**, and **Dictionaries**. I learned the specific syntax for each, how to access their elements, and the fundamental differences between them (mutable vs. immutable, and indexed vs. key-based access).

## Exercise 1: Introducing the List Data Type
I started this exercise by creating a list named `myFruitList`. I defined it using square brackets `[]` and comma-separated values: `["apple", "banana", "cherry"]`. I used the `print()` function to output the list and the `type()` function to confirm it was a `<class 'list'>`. 

I then practiced accessing specific elements in the list using indexing (where Python starts counting at 0). I used `print(myFruitList[0])` to get "apple", `[1]` for "banana", and `[2]` for "cherry". 

Finally, I learned that lists are *mutable* (changeable). I replaced the value at index 2 by using the assignment operator: `myFruitList[2] = "orange"`. I printed the list again to confirm that "cherry" had been successfully changed to "orange".

**Real-World Note on Debugging:** 
After the previous lab where I accidentally typed `mpython3`, I was very careful in this lab to run the script correctly using `python3 collections.py`. I also paid close attention to the syntax, making sure to use square brackets `[]` for the list as specified in the instructions, rather than parentheses.

<img width="1258" height="747" alt="Screenshot 2026-08-31 191602" src="https://github.com/user-attachments/assets/05feba49-0e63-4341-af02-74ebc7ed77c7" />


## Exercise 2: Introducing the Tuple Data Type
Next, I explored tuples. I created a tuple named `myFinalAnswerTuple` using parentheses `()` instead of brackets: `("apple", "banana", "pineapple")`. I used `print()` and `type()` to confirm the output was a `<class 'tuple'>`. 

I then accessed items in the tuple by their position, just like a list, using indices `[0]`, `[1]`, and `[2]`. I learned that tuples are *immutable*, meaning that unlike lists, they cannot be changed after they are created.

<img width="1231" height="861" alt="Screenshot 2026-08-31 191657" src="https://github.com/user-attachments/assets/b54107af-4151-4cae-be0a-a24b03287425" />


## Exercise 3: Introducing the Dictionary Data Type
In the final exercise, I learned about dictionaries, which store data using "key-value" pairs. I created a dictionary named `myFavoriteFruitDictionary` using curly braces `{}`. The keys were names of individuals (like `"Akua"`, `"Saanvi"`, and `"Paulo"`), and the values were their favorite fruits (`"apple"`, `"banana"`, `"pineapple"`). 

I printed the entire dictionary and used `type()` to confirm it was a `<class 'dict'>`. Unlike lists or tuples, dictionaries are accessed by their keys rather than by numerical positions. I successfully retrieved specific values by placing the key inside square brackets, such as `print(myFavoriteFruitDictionary["Akua"])`.

**Real-World Note on Debugging:** 
Writing dictionaries can be tricky because they require a strict syntax of `{ key: value, }` separated by colons and commas. I carefully checked my spelling and punctuation to ensure the script ran without SyntaxErrors, allowing the final terminal output to display the correct values for Akua, Saanvi, and Paulo.
<img width="1340" height="856" alt="Screenshot 2026-08-31 192205" src="https://github.com/user-attachments/assets/0f0ca632-55f7-44ac-bd09-fe61878ac48c" />
<img width="1241" height="697" alt="Screenshot 2026-08-31 192240" src="https://github.com/user-attachments/assets/70237554-ec22-47a8-b0ad-9a6d415e560e" />

```
