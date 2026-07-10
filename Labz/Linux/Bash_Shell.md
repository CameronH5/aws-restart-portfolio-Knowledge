
## Bash Shell — Create an alias and update PATH

### Objectives

In this section, I created and used an alias to back up a folder and I updated the `PATH` environment variable so I could run a script by name.

### What I did:

1. Confirmed my working directory

- I ran `pwd` to confirm I was in my home folder (`/home/ec2-user`).

2. Created the backup alias and produced the archive

- I defined an alias named `backup` that invoked `tar` with the create, verbose, gzip, and file options. For example:

```bash
alias backup='tar -cvzf'
```

- I then used that alias to create a compressed archive of the `CompanyA` folder by running:

```bash
backup backup_companyA.tar.gz CompanyA
```

- The `tar` command listed all of `CompanyA`’s contents as it added them to the archive.

<img width="725" height="373" alt="Screenshot 2026-06-19 160259" src="https://github.com/user-attachments/assets/fdff382f-2358-4782-b933-cfcf2a2e4582" />


3. Verified the tarball was created

- After creating the archive I ran `ls` and confirmed `backup_companyA.tar.gz` and the `CompanyA` directory were present in the working folder.

<img width="380" height="82" alt="Screenshot 2026-06-19 160317" src="https://github.com/user-attachments/assets/4116b23f-e36d-4a0a-b25a-9c0cd6c43fd3" />


4. Navigated to CompanyA/bin and ran the script directly

- I changed into the bin folder:

```bash
cd /home/ec2-user/CompanyA/bin
```

- From that directory I executed the script with:

```bash
./hello.sh
```

- The script printed: `Hello ec2-user`.

<img width="610" height="87" alt="Screenshot 2026-06-19 160445" src="https://github.com/user-attachments/assets/39be6ba1-1a0d-41a0-a51d-283599350f19" />


5. Moved up one level and demonstrated running with a relative path

- I moved to the parent directory using `cd ..` (returning to `/home/ec2-user/CompanyA`).

- From there I ran the script with the relative path `./bin/hello.sh` and it again printed `Hello ec2-user`.

<img width="382" height="26" alt="Screenshot 2026-06-19 160507" src="https://github.com/user-attachments/assets/a73c17bf-4187-4fd3-8a56-f35d0404499d" />
<img width="522" height="37" alt="Screenshot 2026-06-19 160541" src="https://github.com/user-attachments/assets/2b385545-a711-4c54-953d-ef9acb86d88b" />


6. Attempted to run the script by name and diagnosed PATH

- From `/home/ec2-user/CompanyA` I attempted to run `hello.sh` by typing `hello.sh`. The shell returned `-bash: hello.sh: command not found` because `/home/ec2-user/CompanyA/bin` was not in my `PATH`.

- I displayed the `PATH` value with `echo $PATH` and confirmed that `/home/ec2-user/CompanyA/bin` was not present in the PATH list.

<img width="457" height="47" alt="Screenshot 2026-06-19 160559" src="https://github.com/user-attachments/assets/d8cc5b57-d4ff-40a4-b516-3faff2db4e8d" />

<img width="807" height="67" alt="Screenshot 2026-06-19 160620" src="https://github.com/user-attachments/assets/3fecf76c-ab0f-4903-9cca-8e749235b1be" />




7. Updated PATH and ran the script by name

- I appended the CompanyA `bin` directory to `PATH` for the session with:

```bash
PATH=$PATH:/home/ec2-user/CompanyA/bin
```

- After updating `PATH`, I re-ran `hello.sh` by typing `hello.sh` and it printed `Hello ec2-user`, confirming the shell now found the executable in the newly added `PATH` entry.

<img width="755" height="95" alt="Screenshot 2026-06-19 160729" src="https://github.com/user-attachments/assets/5fc8aac3-b1d2-4ba0-99bc-d065d9124c11" />

---
