# Lab: Working with the File System

This document described what I did in the lab (past tense) to create a folder structure, create files, copy/move files and directories, and delete folders. After each command instruction I placed a screenshot placeholder with the image number you provided. Upload your screenshots into the repository's Labz/Linux/images/ folder with the matching filenames (image1.png ... image16.png) so they render in the Markdown.

## Overview

Objectives:
- Create a folder structure provided by the lab
- Create files
- Copy and move files and directories
- Delete files and directories

I recreated the CompanyA folder tree, created the CSV files, validated the structure, and then reorganized the folders so Finance and Management were moved under HR and an Employees folder was created.

---

## Task 2 — Create a folder structure

I followed the lab instructions and executed the commands below. After each command I included a screenshot placeholder with the image number you gave me.

1. I validated that I was in my home folder by running:

```bash
pwd
```

2. I created the top-level folder by running:

```bash
mkdir CompanyA
```

3. I changed into that folder and created the three subfolders:

```bash
cd CompanyA
mkdir Finance HR Management
ls
```
Screenshot of the above commands:
<img width="742" height="196" alt="Screenshot 2026-06-18 185005" src="https://github.com/user-attachments/assets/0cae975a-7144-4bc5-9aac-51d56076c114" />


4. I changed into the HR folder and created two empty CSV files:

```bash
cd HR
touch Assessments.csv TrialPeriod.csv
ls
```

<img width="732" height="192" alt="Screenshot 2026-06-18 185056" src="https://github.com/user-attachments/assets/b7f0e136-de2b-4cfe-8628-e6405b032ebc" />


5. I returned to the Finance folder, created the finance CSV files, and validated them:

```bash
cd ../Finance
touch Salary.csv ProfitAndLossStatements.csv
ls
```

<img width="797" height="108" alt="Screenshot 2026-06-18 185437" src="https://github.com/user-attachments/assets/0f259ba8-f452-4921-bb31-24ae92ab1bc5" />


6. I returned to the CompanyA folder and created the Management files:

```bash
cd ..
touch Management/Managers.csv Management/Schedule.csv
ls Management
```

<img width="815" height="123" alt="Screenshot 2026-06-18 185545" src="https://github.com/user-attachments/assets/3336b1a0-9c58-4c19-bf83-a5ce1cb7b67c" />


7. I validated the entire tree from CompanyA downward by running a recursive detailed listing:

```bash
ls -laR
```

<img width="991" height="620" alt="Screenshot 2026-06-18 185637" src="https://github.com/user-attachments/assets/c2de6e4c-5879-4f48-a603-cd9fd1a49294" />


---

## Task 3 — Delete and reorganize folders

A few weeks later I reorganized the content as the lab asked. I executed the following commands and inserted screenshots after the commands.

1. I ensured I was in the CompanyA folder:

```bash
pwd
```

2. I copied the Finance folder and its contents into HR (recursive copy) and verified the copy:

```bash
cp -r Finance HR/
ls HR/Finance
```

<img width="652" height="142" alt="Screenshot 2026-06-18 185745" src="https://github.com/user-attachments/assets/133c0bef-314f-463c-9dce-b7d458419920" />


3. I attempted to remove the original Finance folder with rmdir but it failed because the folder was not empty, then I removed it recursively after confirming the copy. The lab suggested either removing files first or using rm -r. I used rm -r to remove Finance from CompanyA:

```bash
rmdir Finance   # showed "Directory not empty"
# then, after confirming files were copied:
rm -r Finance
ls
```

<img width="641" height="98" alt="Screenshot 2026-06-18 190620" src="https://github.com/user-attachments/assets/dd07b16f-5ea2-44c0-bee1-9032c1b1680e" />


4. I removed specific files from Finance before re-running rmdir where applicable and then removed the folder. Commands executed included removing files by path and validating with ls, then rmdir Finance once empty:

```bash
rm Finance/ProfitAndLossStatements.csv
rm Finance/Salary.csv
ls Finance
rmdir Finance
ls
```

<img width="742" height="183" alt="Screenshot 2026-06-18 190833" src="https://github.com/user-attachments/assets/04451f4a-4beb-4c5e-9013-85fa89970418" />
<img width="632" height="90" alt="Screenshot 2026-06-18 190852" src="https://github.com/user-attachments/assets/30012ebf-d32b-4f5c-a4d2-d1c6ceb4afb0" />



5. I moved the Management folder inside the HR folder and verified the move:

```bash
mv Management HR/
ls -R HR/Management
```

<img width="610" height="148" alt="Screenshot 2026-06-18 190940" src="https://github.com/user-attachments/assets/c2076cb0-0f54-4661-b541-1b859eb37e34" />


6. I navigated into HR, created the Employees folder, and moved the HR CSV files into Employees:

```bash
cd HR
mkdir Employees
mv Assessments.csv TrialPeriod.csv Employees/
ls Employees
```

<img width="480" height="142" alt="Screenshot 2026-06-18 191150" src="https://github.com/user-attachments/assets/0736b6ce-cc46-42ad-9313-653031eafe59" />




End of Lab.
