# bash scripts

This document described what I did to complete the Bash Shell Scripts lab (Task 2). It recorded the exact commands I ran, the script contents, and where to insert screenshots.

## What I did (past tense)

- I validated I was in my home directory by running:
  - `pwd`
  - Screenshot: <!-- Insert Image 3 here -->

- I created an empty shell script named `backup.sh`:
  - `touch backup.sh`
  - Screenshot: <!-- Insert Image 3 here -->

- I made the script executable:
  - `sudo chmod 755 backup.sh`
  - Screenshot: <!-- Insert Image 3 here -->

- I opened the script in a text editor to edit it:
  - `vi backup.sh` (then I entered insert mode to add the script contents)
  - Screenshot: <!-- Insert Image 3 here -->

- I attempted to run the script without a path:
  - `backup.sh` produced `-bash: backup.sh: command not found` because the current directory was not on PATH.
  - I then ran the script with a relative path:
    - `./backup.sh`
    - The script executed and `tar` printed a verbose list of archived files (for example, many lines like `/home/ec2-user/CompanyA/Management/` and `/home/ec2-user/CompanyA/Finance/Salary.csv`).
    - Screenshot: <!-- Insert Image 3 here -->

- I verified that the archive was created in the `backups` folder:
  - `ls backups/`
  - The output listed the created archive file (for example `2022_05_18_05:55:28_05_55-backup-CompanyA.tar.gz`).
  - Screenshot: <!-- Insert Image 4 here -->

## Script contents I wrote

```bash
#!/bin/bash

# Create a date-stamped filename (year_month_day_time)
DAY="$(date +%Y_%m_%d_%H_%M_%S)"
BACKUP="/home/$USER/backups/${DAY}-backup-CompanyA.tar.gz"

# Create backups directory if it does not exist
mkdir -p "/home/$USER/backups"

# Create a gzipped tar archive of the CompanyA folder
tar -czvpf "$BACKUP" "/home/$USER/CompanyA"
```

## Notes and observations (past tense)

- I observed that trying to run `backup.sh` without `./` returned `command not found` because the current directory was not on the PATH. Running `./backup.sh` executed the script correctly (this was shown in Image 3).
- `tar` produced verbose output listing archived files, confirming the files inside `/home/ec2-user/CompanyA` were archived.
- The final `ls backups/` confirmed the archive file existed (this is shown in Image 4).

## Next steps

- If you want me to add the actual screenshot files to the repository, upload the images (or provide their file paths) and I will add them and update the image links.
- I can also commit this to a new branch and open a pull request if you prefer to keep the default branch unchanged.
