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

- Screenshot: ![Image 4](images/image4.png)

2. I created the top-level folder by running:

```bash
mkdir CompanyA
```

- Screenshot (showing mkdir and subsequent ls): ![Image 6](images/image6.png)

3. I changed into that folder and created the three subfolders:

```bash
cd CompanyA
mkdir Finance HR Management
ls
```

- Screenshot (creation and listing of Finance, HR, Management): ![Image 6](images/image6.png)

4. I changed into the HR folder and created two empty CSV files:

```bash
cd HR
touch Assessments.csv TrialPeriod.csv
ls
```

- Screenshot (showing touch and ls inside HR): ![Image 7](images/image7.png)

5. I returned to the Finance folder, created the finance CSV files, and validated them:

```bash
cd ../Finance
touch Salary.csv ProfitAndLossStatements.csv
ls
```

- Screenshot (creation and ls inside Finance): ![Image 8](images/image8.png)

6. I returned to the CompanyA folder and created the Management files:

```bash
cd ..
touch Management/Managers.csv Management/Schedule.csv
ls Management
```

- Screenshot (creating Management files and listing): ![Image 9](images/image9.png)

7. I validated the entire tree from CompanyA downward by running a recursive detailed listing:

```bash
ls -laR
```

- Screenshot (recursive listing): ![Image 10](images/image10.png)

---

## Task 3 — Delete and reorganize folders

A few weeks later I reorganized the content as the lab asked. I executed the following commands and inserted screenshots after the commands.

1. I ensured I was in the CompanyA folder:

```bash
pwd
```

- Screenshot: ![Image 4](images/image4.png)

2. I copied the Finance folder and its contents into HR (recursive copy) and verified the copy:

```bash
cp -r Finance HR/
ls HR/Finance
```

- Screenshot (verify copy): ![Image 11](images/image11.png)

3. I attempted to remove the original Finance folder with rmdir but it failed because the folder was not empty, then I removed it recursively after confirming the copy. The lab suggested either removing files first or using rm -r. I used rm -r to remove Finance from CompanyA:

```bash
rmdir Finance   # showed "Directory not empty"
# then, after confirming files were copied:
rm -r Finance
ls
```

- Screenshot (rmdir failure and rm steps): ![Image 13](images/image13.png)

4. I removed specific files from Finance before re-running rmdir where applicable and then removed the folder. Commands executed included removing files by path and validating with ls, then rmdir Finance once empty:

```bash
rm Finance/ProfitAndLossStatements.csv
rm Finance/Salary.csv
ls Finance
rmdir Finance
ls
```

- Screenshot (rm then rmdir success): ![Image 14](images/image14.png)

5. I moved the Management folder inside the HR folder and verified the move:

```bash
mv Management HR/
ls -R HR/Management
```

- Screenshot (showing Management under HR): ![Image 15](images/image15.png)

6. I navigated into HR, created the Employees folder, and moved the HR CSV files into Employees:

```bash
cd HR
mkdir Employees
mv Assessments.csv TrialPeriod.csv Employees/
ls Employees
```

- Screenshot (Employees contains Assessments.csv and TrialPeriod.csv): ![Image 16](images/image16.png)

7. I validated the final structure from CompanyA by running a recursive listing and confirmed:

- Finance and Management were now under HR
- Employees contained Assessments.csv and TrialPeriod.csv
- Management still contained Managers.csv and Schedule.csv

```bash
cd ..
ls -laR
```

- Final validation screenshot placeholder: ![Image 10](images/image10.png)

---

## Notes about screenshots and placement
- I included placeholders for the screenshots you provided. Replace the placeholders with the actual image files in the `Labz/Linux/images/` folder of the repository (for example `images/image6.png`) so the screenshots render in the Markdown.
- Mapping of images to steps (recommended filenames):
  - Image 1: Diagram/overview of the expected structure (place at the top of Task 2 if desired)
  - Image 2: (optional) step-by-step command screenshots for mkdir/cd/touch/ls flows
  - Image 3 / 10: recursive listing validation (ls -laR)
  - Image 4: initial pwd checks
  - Image 5 / 11: Task 3 commands and verification screenshots for cp/mv/rm and Employees steps
  - Image 6: mkdir CompanyA, mkdir Finance HR Management, ls output
  - Image 7: cd HR, touch Assessments.csv TrialPeriod.csv, ls
  - Image 8: cd ../Finance, touch Salary.csv ProfitAndLossStatements.csv, ls
  - Image 9: touch Management/Managers.csv Management/Schedule.csv and ls Management
  - Image 11: cp -r Finance HR and verifying ls HR/Finance
  - Image 12: rmdir failure / rm -r Finance steps (if present)
  - Image 13: attempted rm of Finance/ProfitAndLossStatements.csv and follow-up
  - Image 14: rm then rmdir success showing Finance removed
  - Image 15: ls HR/Management showing Managers.csv and Schedule.csv
  - Image 16: ls Employees showing Assessments.csv and TrialPeriod.csv

## Next steps
- I added this lab write-up file to the repository at `Labz/Linux/Working_with_the_File_System.md` with placeholders for the images. To complete the lab report visually, upload your screenshot files into the repository at `Labz/Linux/images/` and name them according to the placeholders (image1.png ... image16.png), or update the Markdown to reference the actual filenames you upload.

If you want, I can also:
- Upload the image files into the `Labz/Linux/images/` folder if you provide the image files (or allow me to pull them from a URL).
- Create a branch for the work instead of committing to the default branch.

---

End of lab write-up.
