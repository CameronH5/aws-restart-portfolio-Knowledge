# Bash scripts

This document described what I did to complete the Bash Shell Scripts lab (Task 2). It recorded the exact commands I ran and the script contents.

## What I did 

- I validated I was in my home directory by running:
  - `pwd`


- I created an empty shell script named `backup.sh`:
  - `touch backup.sh`


- I made the script executable:
  - `sudo chmod 755 backup.sh`


- I opened the script in a text editor to edit it:
  - `vi backup.sh` (then I entered insert mode to add the script contents by pressing 'i')
 

- I attempted to run the script without a path:
  - `backup.sh` produced `-bash: backup.sh: command not found` because the current directory was not on PATH.
  - I then ran the script with a relative path:
    - `./backup.sh`
    - The script executed and `tar` printed a verbose list of archived files (for example, many lines like `/home/ec2-user/CompanyA/Management/` and `/home/ec2-user/CompanyA/Finance/Salary.csv`).
    <img width="613" height="491" alt="Screenshot 2026-06-20 082323" src="https://github.com/user-attachments/assets/651ce5ce-cb2e-4d2b-8a17-5ce3b3f3498b" />


- I verified that the archive was created in the `backups` folder:
  - `ls backups/`
  - The output listed the created archive file (for example `2022_05_18_05:55:28_05_55-backup-CompanyA.tar.gz`).
 
  <img width="463" height="77" alt="Screenshot 2026-06-20 082358" src="https://github.com/user-attachments/assets/599e0555-0d88-4315-9478-90bebf7cbfde" />


  

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

## Notes and observations 

- I observed that trying to run `backup.sh` without `./` returned `command not found` because the current directory was not on the PATH. Running `./backup.sh` executed the script correctly.
- `tar` produced verbose output listing archived files, confirming the files inside `/home/ec2-user/CompanyA` were archived.
- The final `ls backups/` confirmed the archive file existed.

