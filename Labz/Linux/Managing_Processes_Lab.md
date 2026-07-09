# Managing Processes Lab - Complete Documentation

## Objectives
In this lab, I completed the following objectives:
- Created a new log file for process listings
- Used the `top` command to display system performance and active processes
- Established a repetitive task (cron job) that runs auditing commands once a day

---

## Task 2: Exercise - Create List of Processes

**Objective:** Create a log file from process listings that filters for root user or "[Io]" processes.

**Commands Executed:**
```bash
# Confirm working directory
pwd

# Navigate to the appropriate directory
cd /home/ec2-user/companyA

# Create a log file listing all processes running as root or containing "[Io]"
sudo ps -aux | grep -v root | sudo tee SharedFolders/processes.csv

# Validate the work by viewing the created file
cat SharedFolders/processes.csv
```

What I did and what happened:
- I confirmed my working directory was `/home/ec2-user`.
- I navigated to the `/home/ec2-user/companyA` directory as instructed.
- I executed the command `sudo ps -aux | grep -v root | sudo tee SharedFolders/processes.csv`. This command listed all running processes, filtered out those not running as root, and saved the output to `SharedFolders/processes.csv` using `tee`.
- The log file was successfully created and contained a comprehensive list of all current processes running on the machine, including system daemons and user services such as rpcbind, dbus, lsmd, chrony, rngd, postfix, and others.
- The processes displayed included USER, PID, %CPU, %MEM, VSZ, RSS, TTY, STAT, START, TIME, and COMMAND columns.
- I validated my work by executing `cat SharedFolders/processes.csv`, which displayed the contents of the file I had just created.

<img width="455" height="60" alt="Screenshot 2026-06-19 152159" src="https://github.com/user-attachments/assets/240311c3-4d5d-4b03-8d34-6e366acc4268" />

<img width="802" height="443" alt="Screenshot 2026-06-19 152146" src="https://github.com/user-attachments/assets/e2f58305-1d85-4d88-955b-a070c18c4591" />



Notes:
- The `ps -aux` command displays all running processes with detailed information (user, PID, CPU percentage, memory usage, VSZ, RSS, TTY, STAT, START, TIME, and COMMAND).
- The `grep -v root` filter excluded processes running as the root user, allowing me to focus on non-root user-level and system processes.
- The `tee` command printed the output to the terminal while simultaneously writing it to the file `SharedFolders/processes.csv`.

---

## Task 3: Exercise - List the Processes Using the top Command

**Objective:** Use the `top` command to display active processes and observe system performance metrics.

**Commands Executed:**
```bash
# Run the top command to display system performance and active processes
top
```

What I did and what happened:
- I executed the `top` command to display the current system performance and list of active processes and threads.
- The `top` output showed critical system information including:
  - **System uptime:** 7 minutes, 1 user logged in
  - **Load average:** 0.00, 0.05, 0.05
  - **Task summary:** 86 total tasks, 1 running, 47 sleeping, 0 stopped, 0 zombie
  - **CPU usage:** 0.0 us, 0.0 sy, 0.0 ni, 100.0 id, 0.0 wa, 0.0 hi, 0.0 st
  - **Memory information:** 962708 KiB total, 271488 KiB free, 73116 KiB used, 618104 KiB buffers/cache
  - **Swap:** 0 KiB total, 0 KiB free, 0 KiB used, 745792 KiB available
- The process table displayed detailed information for each running process, including PID, USER, PR (priority), NI (nice value), VIRT (virtual memory), RES (resident memory), SHR (shared memory), %CPU, %MEM, TIME+, and COMMAND.
- I observed various system processes including systemd, kthreadd, kworker processes, migration processes, rcu_sched, rcu_bh, and other kernel and user-space services.

<img width="931" height="602" alt="Screenshot 2026-06-19 152256" src="https://github.com/user-attachments/assets/2890b9e2-ce0b-4627-8a6b-92eadc9da598" />



Notes:
- The `top` command is used to display system performance and lists the processes and threads that are active in the system.
- The output of the `top` command provides information about:
  - **Total number of tasks** and their current states (running, sleeping, stopped, zombie).
  - **CPU usage percentages** for user time (us), system time (sy), nice time (ni), idle time (id), and other metrics.
  - **KiB memory used** and the KiB swap available.
  - **Per-process information** including the percentage of CPU and KiB memory each process is using.
- The system was running efficiently with 100% CPU idle time, indicating low system load at the time the command was run.

---

## Task 4: Exercise - Create a Cron Job

**Objective:** Create a cron job that creates an audit file with #### to cover all .csv files and runs once a day.

**Commands Executed:**
```bash
# Navigate to the working directory
cd /home/ec2-user/companyA

# Check for existing crontab
sudo crontab -l

# Create a cron job that creates an audit file with #### to cover all .csv files
sudo crontab -e

# In the crontab editor, add the following lines:
SHELL=/bin/bash
PATH=/usr/bin:/bin:/usr/local/bin:/usr/local/bin
MAILTO=root
0 * * * * ls -la $(find .) | sed -e 's/,/####.csv/g' > /home/ec2-user/companyA/SharedFolders/filteredAudit.csv

# Press ESC, then type :wq and press Enter to save and close

# Validate the cron job by viewing the crontab file
sudo crontab -l
```

What I did and what happened:
- I navigated to the `/home/ec2-user/companyA` directory.
- I executed `sudo crontab -l` to check for any existing crontab entries. The output indicated "no crontab for root - using an empty one", which meant I was starting with a fresh crontab file.
- I executed `sudo crontab -e` to open the crontab editor and create a new cron job.
- I entered the following crontab configuration:
  - `SHELL=/bin/bash` — specified that the bash shell should be used to execute the cron job commands
  - `PATH=/usr/bin:/bin:/usr/local/bin:/usr/local/bin` — specified the directories where the system should look for executable commands
  - `MAILTO=root` — specified that cron job output and any error messages should be emailed to root
  - `0 * * * * ls -la $(find .) | sed -e 's/,/####.csv/g' > /home/ec2-user/companyA/SharedFolders/filteredAudit.csv` — the cron job command that runs at minute 0 of every hour, which lists directory contents with `ls -la`, finds all files with `find .`, pipes the output to `sed` to replace commas with `####.csv`, and redirects the results to the filteredAudit.csv file
- The system displayed "crontab: installing new crontab" message, confirming that the crontab was successfully created and installed.
- I saved and closed the file by pressing ESC, then typing `:wq` and pressing Enter to save the changes.
- I validated my work by executing `sudo crontab -l` to display the contents of the installed crontab file.
<img width="788" height="146" alt="Screenshot 2026-06-19 153229" src="https://github.com/user-attachments/assets/fc1c16e4-ed9b-4139-be97-3eecb776b450" />

<img width="882" height="137" alt="Screenshot 2026-06-19 153214" src="https://github.com/user-attachments/assets/86fd1278-a542-426c-83cc-95800e904a3c" />


What I also did:
- I executed `top -hv` to display the help and version information for the `top` command.
- The output showed that `procps-ng version 3.3.10` was installed and displayed the usage syntax and options available for the `top` command.

<img width="831" height="126" alt="Screenshot 2026-06-19 152351" src="https://github.com/user-attachments/assets/d032c294-fb74-4b92-a183-c38abafcd665" />


Notes:
- The `crontab` is a command that runs a task on a regular basis at a specified time. This command maintains the list of tasks to run in a crontab file, which you create in this task.
- The crontab file includes the following fields: minute, hour, day of month (DOM), month (MON), day of Week (DOW), and command (CMD).
- The cron job I created runs once per hour (the first `0` represents the minute 0, and the `*` for hour means every hour).
- The format `0 * * * *` means:
  - `0` — at minute 0 (top of the hour)
  - `*` — every hour
  - `*` — every day of the month
  - `*` — every month
  - `*` — every day of the week
- For the cron job that creates the audit file, the command uses `find .` to locate all files in the current directory and its subdirectories, pipes that list to `sed` to replace commas with `####.csv`.
- When you run the `sudo crontab -e` command, you are taken to an editor (vi/vim by default) where you can enter the cron job configuration.
- The `SHELL` variable specifies which shell to use for executing the cron job commands.
- The `PATH` variable specifies the directories where the system should look for executable commands, ensuring that commands like `ls`, `sed`, and `find` are available.
- The `MAILTO` variable specifies the email address where cron job output should be sent; setting it to `root` will send all output to the root user's mailbox.
- The `crontab -l` command displays the current crontab entries, allowing you to verify that your cron job has been properly installed.

---

## Commands Recap (what I ran)

- `pwd` — confirmed my working directory was `/home/ec2-user`. (Image 4)
- `cd /home/ec2-user/companyA` — navigated to the working directory. (Image 8)
- `sudo ps -aux | grep -v root | sudo tee SharedFolders/processes.csv` — created a process listing log file. (Image 4)
- `cat SharedFolders/processes.csv` — validated the process log file contents. (Image 4)
- `top` — displayed system performance and active processes. (Image 5)
- `top -hv` — displayed the help and version information for the `top` command. (Image 9)
- `sudo crontab -l` — checked for existing crontab entries and later validated the installed cron job. (Image 8)
- `sudo crontab -e` — opened the crontab editor to create and install a new cron job. (Image 8)

---

## Summary of Outcomes

- I created a log file (`processes.csv`) that listed all non-root processes currently running on the machine and saved it to the SharedFolders directory using the `ps` command piped to `grep` and `tee`. This provided a snapshot of system processes at a specific point in time.
- I used the `top` command to display detailed system performance metrics, including CPU usage, memory utilization, task counts, and a per-process breakdown of resource usage. The system was running efficiently with low load and high CPU idle time (100%), indicating minimal system load.
- I established and installed a cron job configured to run every hour at minute 0. The cron job executes a command that uses `find` to locate files, pipes them through `sed` to replace commas with `####.csv`, and redirects the output to an audit file. The crontab now includes environment variables (SHELL, PATH, and MAILTO) that control how the cron job executes and where output is directed.

