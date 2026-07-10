
# Managing Log Files Lab Report

## Overview
In this lab, I reviewed system log files on a Linux EC2 instance. Specifically, I utilized common Linux tools to examine a sample secure log file and used the `lastlog` application to review the previous login times for all users on the machine.

---

## Task 2: Review secure log files

**Objective:** Use common Linux tools to review the `secure` log files and the `lastlog` Linux application to review previous logins.

### Step 2.1: Navigate to the Working Directory
First, I validated that I was in the correct folder. I executed the `pwd` and `cd` commands to ensure I was currently working within the `companyA` home folder.

```bash
pwd
cd companyA
```
*(Note: Per the lab instructions, I moved into the companyA directory before executing the log file commands.)*

### Step 2.2: Review the Secure Log File
Next, I used the `less` command to open and review the sample secure log file located at `/tmp/log/secure`. This file displays authentication failures and security-related events. I executed the following command:

```bash
sudo less /tmp/log/secure
```
<img width="687" height="31" alt="Screenshot 2026-06-20 104510" src="https://github.com/user-attachments/assets/f5e68034-bdc8-403a-b1c5-62f3514764bb" />


### Step 2.3: Exit the Program
After reviewing the authentication logs listed in the `less` program, I exited the viewer by pressing the appropriate command.

```bash
q
```

### Step 2.4: Review Last Login Times
To conclude the task, I used the `lastlog` Linux application to view a detailed list of all users on the machine, along with their most recent login dates, times, and source IP addresses. I ran the following command:

```bash
sudo lastlog
```
<img width="722" height="292" alt="Screenshot 2026-06-20 104534" src="https://github.com/user-attachments/assets/a9aa2206-52de-4e77-bd1f-5b4c7bf3c063" />
<img width="796" height="160" alt="Screenshot 2026-06-20 104545" src="https://github.com/user-attachments/assets/9a97fe46-a522-49ba-9fb4-77f6ee9c0c8f" />


```
