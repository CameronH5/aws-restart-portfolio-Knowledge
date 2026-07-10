
# Challenge Lab: Bash Shell Scripting Exercise

## Overview
In this challenge, I successfully designed and implemented a Bash shell script to automate file generation. The script dynamically creates 25 empty files with specific naming conventions, automatically handling numbering to prevent overwriting existing files. I also validated the script's functionality by executing it and inspecting the directory contents.

---

## Objectives Addressed
*   Create a Bash script that generates files automatically.
*   Implement file naming conventions: `<yourName><number>`.
*   Design the script to be idempotent (automatically finding the next available number in sequence instead of hard-coding values).
*   Test and validate the script execution against the generated files.

---

## Task: Bash Shell Scripting Exercise

### Step 1: Create the Bash Script File
To begin, I opened my terminal and created a new file named `createfiles.sh` using the `nano` command-line text editor. I was already located in my home directory (`~`), which was appropriate for this exercise.

```bash
nano createfiles.sh
```


### Step 2: Write the Script Logic
Inside the editor, I wrote the Bash script to handle the dynamic file generation. 
I defined my name as `yourName="Cameron"`. The script uses a `for` loop to iterate 25 times. To avoid hard-coding starting numbers, it first looks at the current directory, extracts the highest number from any existing `Cameron` files, and starts the new batch from `max_number + 1`. It then uses the `touch` command to create the 25 empty files.

Here is the logic I typed into the script:

```bash
#!/bin/bash
yourName="Cameron"
max_number=0

# Find the highest existing number
for file in ${yourName}[0-9]*; do
  if [ -f "$file" ]; then
    number=$(echo "$file" | sed "s/${yourName}//")
    if [ "$number" -gt "$max_number" ]; then
      max_number=$number
    fi
  fi
done

# Create the next 25 files
next_number=$((max_number + 1))
for i in $(seq 1 25); do
  touch "${yourName}${next_number}"
  next_number=$((next_number + 1))
done

echo "Created 25 files starting from ${yourName}$((max_number + 1)) to ${yourName}$((next_number - 1))"
```


### Step 3: Make the Script Executable
After saving the file (using `Ctrl+O` and `Enter`) and exiting `nano` (using `Ctrl+X`), I made the script executable by changing its file permissions.

```bash
chmod +x createfiles.sh
```


### Step 4: Execute the Script
Next, I ran the script to generate the files. The script executed successfully and output a confirmation message indicating that it created files starting from `Cameron1` all the way to `Cameron25`.

```bash
./createfiles.sh
```
*Expected Output:* `Created 25 files starting from Cameron1 to Cameron25!/bin/bash`


### Step 5: Validate the Script Results
Finally, to confirm that the script performed as expected, I listed all files in the directory in long format using the `ls -la` command. The output verified that 25 empty files (0 KB each) were successfully created, correctly named from `Cameron1` to `Cameron25`.

```bash
ls -la
```
<img width="732" height="327" alt="Screenshot 2026-06-20 110625" src="https://github.com/user-attachments/assets/cc16c550-65ed-4b89-851a-38912b396e71" />
<img width="817" height="440" alt="Screenshot 2026-06-20 110850" src="https://github.com/user-attachments/assets/61722847-470b-4fe8-9fc5-2e95ef0a82d7" />
<img width="842" height="642" alt="Screenshot 2026-06-20 110955" src="https://github.com/user-attachments/assets/8c7966ed-7a13-40b5-bdf6-5147c4cd62e9" />


```
