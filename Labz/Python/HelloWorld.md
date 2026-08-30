# Lab Summary: Introduction to Python Programming

## Overview
In this lab session, I began my journey into Python programming by setting up my development environment, verifying the Python installation, and executing my first script. The primary objective was to create a simple "Hello, World!" program, a traditional first step in learning a new language.

## Steps Performed

### 1. Environment Setup and Version Check
- I started by checking the installed Python version to ensure my environment was correctly configured.
- I executed the command `python3 --version` to verify that Python 3 was available.
- **Note:** I initially made a small typo by entering `python3--version` (missing a space between the command and the argument). The system returned a `bash: python2: command not found` error. I quickly corrected this to `python3 --version` and received the expected output: `Python 3.11.15`.

### 2. Attempting to Run an Unnamed File
- I attempted to run a generic script by using the command `python3 filename.py`.
- This resulted in an error: `python3: can't open file '/home/ec2-user/environment/filename.py': [Errno 2] No such file or directory`.
- I realized this was because I had not yet created a file with that exact name, so I proceeded to create the correct script.

### 3. Creating and Executing the "Hello, World!" Script
- I created a new Python file named `hello-world.py` which contained the line `print("Hello,world")`.
- Before successfully executing the file, I made a spelling mistake in the terminal by typing `python3 hello-wor1.py` (using the number `1` instead of the letter `l`). I corrected this to the proper filename.
- Finally, I ran the command `python3 hello-world.py`.
- The script executed successfully, and the output `Hello,world` was displayed in the terminal.

## Reflection
This lab session was a practical introduction to Python programming. A key takeaway from this task was the importance of attention to detail. The process of debugging typos—both in the terminal commands and the filenames—mirrors real-world development. It reinforced the need to carefully check syntax and filenames, as even a small error can cause the program to fail. This experience provided a solid foundation for future, more complex coding tasks.

<img width="1802" height="861" alt="Screenshot 2026-08-30 101708" src="https://github.com/user-attachments/assets/7e0e2916-2158-4d18-ab9b-6a54b3c0e6a6" />
