# Working with Files — Lab: Create a backup

This document described the steps I completed for the lab (create a backup, log it, move it). Each command is shown and an image placeholder follows so you can insert your screenshot files (use the filenames you prefer, e.g., image5.png ... image12.png).

## 1. Inspect the CompanyA folder recursively

Command I ran:

```
ls -R CompanyA
```

Screenshot: ![Image 5 — ls -R CompanyA](image5.png)

---

## 2. Create a compressed tar archive of the entire CompanyA folder

Command I ran:

```
tar -csvpzf backup.CompanyAtar.gz CompanyA
```

What I observed: tar listed each folder and file as it added them to the archive; the archive was created with the exact name I typed (backup.CompanyAtar.gz).

Screenshot: ![Image 6 — tar output](image6.png)

---

## 3. Verify the archive file existed

Command I ran:

```
ls
```

What I observed: the listing showed the archive file (backup.CompanyAtar.gz) alongside the CompanyA directory.

Screenshot: ![Image 7 — ls showing archive](image7.png)

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

Screenshot: ![Image 8 — mv error and rename](image8.png)


**Note:** I fixed the earlier filename typo (backup.CompanyAtar.gz → backup.CompanyA.tar.gz) using `mv`.

---

## 5. Change into the CompanyA folder to prepare the backup log

Command I ran:

```
cd /home/ec2-user/CompanyA
```

What I observed: my prompt changed to indicate I was in `/home/ec2-user/CompanyA`.

Screenshot: ![Image 9 — cd into CompanyA](image9.png)

---

## 6. Record the date, time, and archive filename into the log file

Command I ran:

```
echo "18 June 2026, 19:42, backup.CompanyA.tar.gz" | sudo tee SharedFolders/backups.csv
```

What I observed: the line printed to the terminal and was written to `SharedFolders/backups.csv`.

Screenshot: ![Image 10 — echo | tee output](image10.png)

---

## 7. Display the contents of the backups.csv file to verify the log entry

Command I ran:

```
cat SharedFolders/backups.csv
```

What I observed: the file contained the new log line with date/time and the backup filename.

Screenshot: ![Image 11 — cat backups.csv output](image11.png)

---

## 8. Move the backup archive to the IA folder and verify

Commands I ran:

```
pwd
mv ../backup.CompanyA.tar.gz IA/
ls -R IA
```

What I observed: the working directory was `/home/ec2-user/CompanyA`; the archive was moved to `CompanyA/IA` and `ls` showed `backup.CompanyA.tar.gz` inside IA.

Screenshot: ![Image 12 — pwd, mv, and ls IA output](image12.png)

---

## Summary and notes

- I created a compressed tar archive of the CompanyA directory, logged the backup with a timestamp in `SharedFolders/backups.csv`, and moved the archive to the IA folder so the IA team could access it.
- I corrected a small filename typo (`backup.CompanyAtar.gz` → `backup.CompanyA.tar.gz`) using `mv` before moving the file.


## Next steps / how you can finalize this document

- Replace the image placeholders (image5.png … image12.png) with the actual screenshot file names once you upload them to the repository or provide their filenames.
- If you want, I can also add the image files into the repository for you if you upload them here or provide a branch and confirm commit.

---

*Generated: created by assistant on request.*
