# Working with Files — Lab: Create a backup

This document described the steps I completed for the lab (create a backup, log it, move it). Each command is shown and an image placeholder follows so you can insert your screenshot files (use the filenames you prefer, e.g., image5.png ... image12.png).

## 1. Inspect the CompanyA folder recursively

Command I ran:

```
ls -R CompanyA
```

<img width="678" height="426" alt="Screenshot 2026-06-18 193503" src="https://github.com/user-attachments/assets/d7d70e3e-e5a1-44fd-b286-04c972e94ff0" />


---

## 2. Create a compressed tar archive of the entire CompanyA folder

Command I ran:

```
tar -csvpzf backup.CompanyAtar.gz CompanyA
```

What I observed: tar listed each folder and file as it added them to the archive; the archive was created with the exact name I typed (backup.CompanyAtar.gz).

<img width="892" height="301" alt="Screenshot 2026-06-18 193625" src="https://github.com/user-attachments/assets/9388ce9a-0d1d-447f-b402-9ad826b91f4b" />


---

## 3. Verify the archive file existed

Command I ran:

```
ls
```

What I observed: the listing showed the archive file (backup.CompanyAtar.gz) alongside the CompanyA directory.

<img width="470" height="57" alt="Screenshot 2026-06-18 193902" src="https://github.com/user-attachments/assets/6dc6c07b-dec6-473e-bd05-620e9f17006a" />


---

## 4. Fix filename typo and rename the archive

Commands I ran:

```
mv backup.CompanyA.tar.gz
```

(Produced an error because no destination was provided.)

Then I corrected the filename with:

```
mv backup.CompanyAtar.gz backup.CompanyA.tar.gz
```

What I observed: the file was successfully renamed to backup.CompanyA.tar.gz.

<img width="897" height="265" alt="Screenshot 2026-06-18 193921" src="https://github.com/user-attachments/assets/b8685112-9923-4c95-a3cb-ed4c1edc238f" />




**Note:** I fixed the earlier filename typo (backup.CompanyAtar.gz → backup.CompanyA.tar.gz) using `mv`.

---

## 5. Change into the CompanyA folder to prepare the backup log

Command I ran:

```
cd /home/ec2-user/CompanyA
```

What I observed: my prompt changed to indicate I was in `/home/ec2-user/CompanyA`.

<img width="605" height="60" alt="Screenshot 2026-06-18 194051" src="https://github.com/user-attachments/assets/af9ef787-d99c-4908-ac93-6a4f76b76995" />


---

## 6. Record the date, time, and archive filename into the log file

Command I ran:

```
echo "18 June 2026, 19:42, backup.CompanyA.tar.gz" | sudo tee SharedFolders/backups.csv
```

What I observed: the line printed to the terminal and was written to `SharedFolders/backups.csv`.

<img width="1136" height="76" alt="Screenshot 2026-06-18 194319" src="https://github.com/user-attachments/assets/412f54e4-3ef2-4207-85ad-c601290d9514" />


---

## 7. Display the contents of the backups.csv file to verify the log entry

Command I ran:

```
cat SharedFolders/backups.csv
```

What I observed: the file contained the new log line with date/time and the backup filename.

<img width="726" height="61" alt="Screenshot 2026-06-18 194453" src="https://github.com/user-attachments/assets/623bd626-4dda-4a75-a641-41f301709457" />


---

## 8. Move the backup archive to the IA folder and verify

Commands I ran:

```
pwd
mv ../backup.CompanyA.tar.gz IA/
ls -R IA
```

What I observed: the working directory was `/home/ec2-user/CompanyA`; the archive was moved to `CompanyA/IA` and `ls` showed `backup.CompanyA.tar.gz` inside IA.

<img width="842" height="193" alt="Screenshot 2026-06-18 194634" src="https://github.com/user-attachments/assets/6ecee2ce-d644-4120-af16-a75cd2f2ac78" />


---

## Summary and notes

- I created a compressed tar archive of the CompanyA directory, logged the backup with a timestamp in `SharedFolders/backups.csv`, and moved the archive to the IA folder so the IA team could access it.
- I corrected a small filename typo (`backup.CompanyAtar.gz` → `backup.CompanyA.tar.gz`) using `mv` before moving the file.

