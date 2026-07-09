# Working with Commands Lab - Complete Documentation

## Objectives
In this lab, I completed the following objectives:
- Used the `tee` command to direct output to a file
- Used the `sort` command to reorder the contents of a .csv file
- Used the `cut` command to extract fields from a file
- Used the `sed` command to transform text
- Used the pipe operator to chain commands

---

## Task 2: Used the tee command

**Objective:** Display output to the screen and write the same output to a file using `tee`.

**Commands Executed:**
```bash
# Confirm working directory (optional)
pwd

# Capture and display the hostname, and write it to file1.txt
hostname | tee file1.txt

# Verify the file was created
ls
```

What I did and what happened:
- I confirmed my working directory (I was in /home/ec2-user).
- I executed `hostname | tee file1.txt`. The instance hostname printed to the terminal and the same hostname was written into `file1.txt`.
- I ran `ls` to verify that `file1.txt` existed alongside other items in the directory.

<img width="567" height="108" alt="Screenshot 2026-06-19 145951" src="https://github.com/user-attachments/assets/3415b648-6c47-4419-8024-4941b6bb49fc" />


---

## Task 3: Used the sort command and the pipe operator

**Objective:** Create a CSV file, reorder it with `sort`, and search the results using a pipeline.

**Commands Executed:**
```bash
# Create test.csv and enter records, then press CTRL+D to save
cat > test.csv
Factory, 1, Paris
Store, 2, Dubai
Factory, 3, Brasilia
Store, 4, Algiers
Factory, 5, Tokyo
# Press CTRL+D to finish

# Reorder the file contents
sort test.csv

# Search for the Paris record using a pipeline
cat test.csv | grep Paris
# (or) find | grep Paris test.csv
```
<img width="538" height="173" alt="Screenshot 2026-06-19 150328" src="https://github.com/user-attachments/assets/ef061c85-42b1-4b5e-a0e1-d6c1e2414205" />

What I did and what happened:
- I created `test.csv` with five records and saved it.
- I ran `sort test.csv`. The `sort` command read the file and printed a sorted version to stdout; the output grouped similar prefixes (e.g., "Factory") and ordered entries according to default lexical rules.
- I piped the file contents to `grep` to search for the record containing "Paris" and the pipeline returned the matching line.

<img width="497" height="143" alt="Screenshot 2026-06-19 150352" src="https://github.com/user-attachments/assets/f729764a-eb29-4dd1-a740-4fbedb4a75e9" />

<img width="577" height="72" alt="Screenshot 2026-06-19 150434" src="https://github.com/user-attachments/assets/765fd256-8bcf-484e-9bff-c5bc1799ff62" />


Notes:
- `sort` printed the reordered list to stdout and did not modify `test.csv` unless you redirected output back into the file.
- Using a pipe (`|`) let me stream the file contents into `grep` to filter for lines that contained "Paris".

---

## Task 4: Used the cut command

**Objective:** Create a cities CSV and extract the first field (city names) with `cut`.

**Commands Executed:**
```bash
# Create cities.csv and enter city,state pairs, then press CTRL+D to save
cat > cities.csv
Dallas, Texas
Seattle, Washington
Los Angeles, California
Atlanta, Georgia
New York, New York
# Press CTRL+D to finish

# Extract the city names using comma as the delimiter
cut -d ',' -f 1 cities.csv
```

What I did and what happened:
- I created `cities.csv` containing five city,state lines.
- I ran `cut -d ',' -f 1 cities.csv`. `cut` used the comma as the field delimiter and returned the first field from each line (the city names).
- The terminal output showed:
  - Dallas
  - Seattle
  - Los Angeles
  - Atlanta
  - New York

<img width="618" height="267" alt="Screenshot 2026-06-19 150651" src="https://github.com/user-attachments/assets/763792b9-c930-43b3-abc9-ff94015d3276" />


Notes:
- `cut` printed the extracted fields to stdout and did not modify the original file.
- Because lines contained a comma followed by a space, extracted results preserved the exact text left of the comma; any leading/trailing spaces could be trimmed if desired.

---

## Additional Challenge: Used the sed command

**Objective:** Replace commas with periods in both `cities.csv` and `test.csv` using `sed`.

**Commands Executed:**
```bash
# Replace the first comma on each line (prints to stdout)
sed 's/,/./' cities.csv

# Replace the first comma on each line in test.csv (prints to stdout)
sed 's/,/./' test.csv

# If you wanted to replace every comma on each line, you would use:
sed 's/,/./g' cities.csv
sed 's/,/./g' test.csv

# To save the replacements to the same file (in-place), you could use:
sed -i.bak 's/,/./g' cities.csv
sed -i.bak 's/,/./g' test.csv
```

What I did and what happened:
- I ran `sed 's/,/./' cities.csv`. `sed` replaced the first comma on each line with a period and printed the transformed lines to stdout. The output looked like:
  - Dallas. Texas
  - Seattle. Washington
  - Los Angeles. California
  - Atlanta. Georgia
  - New York. New York

<img width="568" height="148" alt="Screenshot 2026-06-19 150752" src="https://github.com/user-attachments/assets/8f6710b6-7596-420c-9f21-cb00cd06ce16" />


- I ran `sed 's/,/./' test.csv`. `sed` replaced the first comma on each line in `test.csv` with a period and printed the results to stdout (e.g., `Factory. 1, Paris` etc.).

<img width="565" height="152" alt="Screenshot 2026-06-19 150818" src="https://github.com/user-attachments/assets/a7bfe4bc-c46e-4a41-bd0d-b90d1f29ffa4" />


Notes and important details:
- By default `sed 's/old/new/'` replaces only the first occurrence of the pattern on each line. To replace all occurrences on a line use the `g` flag: `sed 's/,/./g'`.
- `sed` printed changes to stdout. To permanently modify files, either redirect the output to a new file or use the `-i` (in-place) option (for safety, `-i.bak` can create a backup).
- To trim whitespace after extracting fields with `cut`, you could pipe to `sed` or `awk`, for example:
  - `cut -d',' -f1 cities.csv | sed 's/^ *//'`  # removes leading spaces

---

## Commands Recap (what I ran)
- pwd — confirmed my working directory.
- hostname | tee file1.txt — printed the hostname and saved it to `file1.txt`. (Image 4)
- ls — verified the file existed. (Image 4)
- cat > test.csv (entered records, CTRL+D) — created `test.csv`.
- sort test.csv — displayed the sorted contents of `test.csv`. (Image 5)
- cat test.csv | grep Paris — piped file contents into `grep` to find Paris. (Image 7)
- cat > cities.csv (entered city,state lines, CTRL+D) — created `cities.csv`.
- cut -d ',' -f 1 cities.csv — extracted city names from `cities.csv`. (Image 8)
- sed 's/,/./' cities.csv and sed 's/,/./' test.csv — replaced the first comma on each line with a period for each file. (Images 9 & 10)

---

## Summary of Outcomes
- I captured the instance hostname to a file while also printing it to the terminal using `tee`.
- I created and populated `test.csv`, then used `sort` to reorder the file contents and `grep` (via a pipe) to find a specific record.
- I created `cities.csv` and used `cut` to extract the city names.
- I used `sed` to replace commas with periods on each line (for the first comma), and noted how to replace all commas and make changes in-place when desired.


