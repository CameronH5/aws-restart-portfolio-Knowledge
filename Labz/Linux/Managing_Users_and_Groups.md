# Lab: Managing Users and Groups

In this lab, I created new users with default passwords, created groups and assigned users to them, and logged in as different users to understand sudoers permissions and security logging.

## Objectives

In this lab, I accomplished the following:
- Created new users with a default password
- Created groups and assigned the appropriate users
- Logged in as different users to explore permissions and sudoers capabilities

---

# Task 2: Create Users

In this section, I created users based on the following information:

| First Name | Last Name | User ID | Job Role | Starting Password |
|---|---|---|---|---|
| Alejandro | Rosalez | arosalez | Sales Manager | P@ssword1234! |
| Efua | Owusu | eowusu | Shipping | P@ssword1234! |
| Jane | Doe | jdoe | Shipping | P@ssword1234! |
| Li | Juan | ljuan | HR Manager | P@ssword1234! |
| Mary | Major | mmajor | Finance Manager | P@ssword1234! |
| Mateo | Jackson | mjackson | CEO | P@ssword1234! |
| Nikki | Wolf | nwolf | Sales Representative | P@ssword1234! |
| Paulo | Santos | psantos | Shipping | P@ssword1234! |
| Sofia | Martinez | smartinez | HR Specialist | P@ssword1234! |
| Saanvi | Sarkar | ssarkar | Finance Specialist | P@ssword1234! |

I ensured that I spelled the user IDs correctly so that these users could use the default credentials to log in.

## Step 21: Verify Current Directory

I validated that I was in the home folder of my current user by entering `pwd` and pressing Enter.

```
[ec2-user@ip-10-0-10-100 ~]$ pwd
/home/ec2-user
```

This confirmed I was in the `/home/ec2-user` directory.

## Step 22-23: Create First User (Alejandro Rosalez)

I added the first user from the list, Alejandro Rosalez, by entering `sudo useradd arosalez` and pressing Enter. This step created the user arosalez. Then I entered `sudo passwd arosalez` and pressed Enter to set the password.

```
[ec2-user@ip-10-0-10-100 ~]$ sudo useradd arosalez
[ec2-user@ip-10-0-10-100 ~]$ sudo passwd arosalez
Changing password for user arosalez.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
[ec2-user@ip-10-0-10-100 ~]$
```
<img width="616" height="150" alt="Screenshot 2026-06-18 173642" src="https://github.com/user-attachments/assets/dd3c8158-a816-4740-8a79-ff444bfefe16" />

I entered the password `P@ssword1234!` twice as required.

## Step 24: Validate User Creation

I validated that the users had been created by entering `sudo cat /etc/passwd | cut -d: -f1` and pressing Enter to look at the contents of the `/etc/passwd` file.

```
[ec2-user@ip-10-0-10-100 ~]$ sudo cat /etc/passwd | cut -d: -f1
.......
ec2-user
arosalez
```
<img width="633" height="480" alt="Screenshot 2026-06-18 173936" src="https://github.com/user-attachments/assets/4f8c5ea9-fa05-4f09-9a10-093652cd64c6" />


This command helped visualize the created users. The output showed that the first user `arosalez` was successfully added to the system.

## Step 25: Create Remaining Users

I used the `sudo useradd <user ID>` and `sudo passwd <user ID>` commands to add the remaining users from the table. For each user, I followed the same process as I did for Alejandro Rosalez, replacing `<User ID>` with each User ID in the table at the beginning of this task.

## Step 26: Validate All Users Created

I validated that all the users had been created by entering `sudo cat /etc/passwd | cut -d: -f1` and pressing Enter.

```
[ec2-user@ip-10-0-10-100 ~]$ sudo cat /etc/passwd | cut -d: -f1
.......
ec2-user
arosalez
eowusu
jdoe
mmajor
mjackson
nwolf
psantos
smartinez
ssarkar
```
<img width="360" height="503" alt="Screenshot 2026-06-18 175131" src="https://github.com/user-attachments/assets/19cc7b31-06cb-4267-ada4-0a635fff7473" />


The output confirmed that all 10 new users had been successfully created and added to the system.

---

# Task 3: Create Groups

In this section, I created groups of users and added users to the groups. The groups I created were:

- Sales
- HR
- Finance
- Personnel
- CEO
- Shipping
- Managers

Once I created these groups, I added the users to the proper groups based on the information provided in the Task 2 table.

**Note:** Managers are personnel, but not all personnel are managers. Some users belonged to multiple groups.

## Step 27: Verify Current Directory

I validated that I was in the home folder of my current user by entering `pwd` and pressing Enter.

## Step 28: Create Sales Group

I created the Sales group by entering `sudo groupadd Sales` and pressing Enter.

## Step 29: Verify Group Creation

I verified that the group was added by entering `cat /etc/group` and pressing Enter.

```
...
ec2-user:x:1000:
.......
Sales:x:1014
...
```
<img width="397" height="518" alt="Screenshot 2026-06-18 175315" src="https://github.com/user-attachments/assets/36cc9963-cace-4a4a-a9c7-ebd4f6d645ff" />


This output showed the `/etc/group` file contents and confirmed the Sales group was created.

## Step 30: Create Remaining Groups

I used the `sudo groupadd <Group>` command to add the remaining groups. I replaced `<Group Name>` with HR, Finance, Shipping, Managers, and CEO to create these groups.

## Step 31: Verify All Groups Created

I verified that all the groups had been added by entering `cat /etc/group` and pressing Enter.

```
Sales:x:1010:
HR:x:1011:
Finance:x:1012:
Shipping:x:1013:
Managers:x:1014:
CEO:x:1015:
```
<img width="383" height="187" alt="Screenshot 2026-06-18 175531" src="https://github.com/user-attachments/assets/9e2b8944-5187-4ee6-bd6a-3534d277b4a3" />


The output confirmed that all groups had been successfully created.

## Step 32: Add First User to Sales Group

I added the user arosalez to the Sales group by entering `sudo usermod -a -G Sales arosalez` into the terminal and pressing Enter.

## Step 33: Verify User Added to Group

I verified that the user was added by entering `cat /etc/group` and pressing Enter.

```
...
Sales:x:1014:arosalez
...
```


This confirmed that arosalez was now a member of the Sales group.

## Step 34: Add Remaining Users to Groups

I used the `sudo usermod -a -G <Group Name> <user ID>` command to add the remaining users to the appropriate groups. Using the information from the Task 2 table, I replaced `<Group Name>` with the Group Name and `<user ID>` with each User ID in the User IDs columns:

| Group Name | User IDs | Group Name | User IDs | Group Name | User IDs |
|---|---|---|---|---|---|
| Sales | arosalez, nwolf | HR | ljuan, smartinez | Finance | mmajor, ssarkar |
| Shipping | eowusu, jdoe, psantos | Managers | arosalez, ljuan, mmajor | CEO | mjackson |

<img width="462" height="232" alt="Screenshot 2026-06-18 180937" src="https://github.com/user-attachments/assets/a308296d-49ce-4729-af46-bf807cd3407c" />


## Step 35: Add ec2-user to All Groups

I added ec2-user to all groups.


## Step 36: Check Group Memberships

I checked the group memberships by entering `sudo cat /etc/group` into the terminal and pressing Enter.

```
nwolf:x:1006:
psantos:x:1007:
smartinez:x:1008:
ssarkar:x:1009:
Sales:x:1010:arosalez,nwolf,ec2-user
HR:x:1011:ljuan,smartinez,ec2-user
Finance:x:1012:mmajor,ssarkar,ec2-user
Shipping:x:1013:jdoe,eowusu,psantos,ec2-user
Managers:x:1014:arosalez,ljuan,mmajor,ec2-user
CEO:x:1015:mjackson,ec2-user
```
<img width="561" height="207" alt="Screenshot 2026-06-18 181135" src="https://github.com/user-attachments/assets/62238868-16d7-46e9-a135-c51bcb5cc6ca" />

The output displayed all groups with their respective members, including ec2-user who was now part of all groups.

---

# Task 4: Log in using the New Users

Now that I had created users and groups in the machine, I logged in as a new user. I also saw what a sudoer is, what this enables, and how commands issued using sudo are logged in the `/var/log/secure` file.

## Step 37: Log in as arosalez

I entered `su arosalez` to switch to the arosalez user.

## Step 38: Enter Password

For the password, I entered `P@ssword1234!` and pressed Enter. I was now logged in as arosalez.

```
[ec2-user@ip-10-0-10-100 ~]$ su arosalez
Password:
[arosalez@ip-10-0-10-100 ec2-user]$ pwd
/home/ec2-user
[arosalez@ip-10-0-10-100 ec2-user]$
```

The trailing `ec2-user` indicated that I was located in the ec2-user home directory, at `/home/ec2-user`.

## Step 39: Verify Current Directory

I entered `pwd` and pressed Enter to ensure that I was in the `/home/ec2-user` directory.

<img width="467" height="123" alt="Screenshot 2026-06-18 181307" src="https://github.com/user-attachments/assets/7f12a749-05cd-4bdf-9aab-c1350d194a0e" />

## Step 40: Attempt to Create File (Without Sudo)

I entered `touch myfile.txt` and pressed Enter.

```
[arosalez@ip-10-0-10-100 ec2-user]$ touch myfile.txt
touch: cannot touch 'myfile.txt': Permission denied
[arosalez@ip-10-0-10-100 ec2-user]$
```

<img width="557" height="76" alt="Screenshot 2026-06-18 181351" src="https://github.com/user-attachments/assets/bd45361e-eaa1-45f6-a025-39e7ecccf895" />

I received this message because the user arosalez does not have permission to write files in the ec2-user home folder.

## Step 41: Attempt to Create File (With Sudo)

I tried as an admin using the sudo command. I entered `sudo touch myfile.txt` and pressed Enter.

```
[arosalez@ip-10-0-10-100 ec2-user]$ sudo touch myfile.txt

We trust you have received the usual lecture from the local system
Administrator. It usually boils down to these three things:

#1) Respect the privacy of others.
#2) Think before you type.
#3) With great power comes great responsibility.

[sudo] password for arosalez:
arosalez is not in the sudoers file.  This incident will be reported.
[arosalez@ip-10-0-10-100 ec2-user]$
```

<img width="748" height="300" alt="Screenshot 2026-06-18 181448" src="https://github.com/user-attachments/assets/42641b43-2c94-44fd-8524-2bbda2296f28" />

I entered the password `P@ssword1234!` and pressed Enter. I received this message because the user arosalez is not on the list of the sudoers file. Sudoers are users who have special rights to run commands with elevated privileges. Only a few users should receive this permission.

## Step 42: Exit arosalez User Session

I entered `exit` and pressed Enter to switch to the previous user, ec2-user.

<img width="701" height="75" alt="Screenshot 2026-06-18 181656" src="https://github.com/user-attachments/assets/24e5482f-c2c5-47cb-b7bb-fcd8cc5d7c0a" />

## Step 43: View Secure Log File

I entered `sudo cat /var/log/secure` and pressed Enter to display the content of the secure file. I scrolled to the bottom of the file using the down arrow.

```
[ec2-user@ip-10-0-10-100 ~]$ sudo cat /var/log/secure
...
Jun 18 16:14:29 ip-10-0-10-100 sudo: arosalez : user NOT in sudoers ; TTY=pts/0
; PWD=/home/ec2-user ; USER=root ; COMMAND=/bin/touch#040myfile.txt
Jun 18 16:15:48 ip-10-0-10-100 su: pam_unix(su:session): session closed for user
arosalez
Jun 18 16:16:16 ip-10-0-10-100 sudo: ec2-user : TTY=pts/0 ; PWD=/home/ec2-user ;
USER=root ; COMMAND=/bin/cat#040/var/log/secure
```
<img width="878" height="195" alt="Screenshot 2026-06-18 181637" src="https://github.com/user-attachments/assets/68d9c4c7-1586-4b7e-8271-53c97962ac31" />


The output showed how a sudo and not permitted action was logged into the `/var/log/secure` file. I could see:
- The unsuccessful sudo attempt by arosalez trying to create a file
- The attempted command (`/bin/touch#040myfile.txt`)
- The session closure for the arosalez user
- The sudo command issued by ec2-user to view the secure log file

This demonstrated that all sudo commands and security-related events are tracked and logged for auditing purposes.

---

## Summary

Through this lab, I gained practical experience with:
- Creating multiple user accounts with default passwords using `sudo useradd` and `sudo passwd`
- Creating user groups using `sudo groupadd`
- Assigning users to groups using `sudo usermod -a -G`
- Switching between users using the `su` command
- Understanding file permissions and why certain users cannot write to directories
- Understanding the sudoers file and sudo privileges
- Viewing the `/etc/passwd` file to confirm user creation
- Viewing the `/etc/group` file to confirm group creation and membership
- Viewing the `/var/log/secure` file to understand security logging and sudo tracking
- Recognizing that sudo commands and failed permission attempts are logged for security auditing
