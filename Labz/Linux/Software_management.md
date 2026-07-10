
# Software Management Lab

## Overview
In this lab, I successfully managed software packages on a Linux instance by updating the system, rolling back a recent installation using the package manager, and installing the AWS CLI. I also configured the AWS CLI to connect to my AWS account and validated the configuration by retrieving instance metadata.

---

## Task 2: Update the Linux Machine using the Package Manager

**Objective:** Utilize the `yum` package manager to update and upgrade the machine, including relevant security packages.

### Step 2.1: Navigate to the Working Directory
First, I needed to validate that I was in the correct `companyA` home folder. I executed the `pwd` and `cd` commands to navigate to the appropriate directory.

```bash
pwd
cd companyA
pwd
```


### Step 2.2: Query and Apply Updates
Next, I checked the system for available updates and applied relevant security and general updates. I ran the following commands:

1. I queried the repositories for available updates:
   ```bash
   sudo yum -y check-update
   ```
  <img width="772" height="125" alt="Screenshot 2026-06-20 083227" src="https://github.com/user-attachments/assets/7e819221-300a-4edc-a212-1db12fa7e3fe" />


2. I applied the available security-related updates:
   ```bash
   sudo yum update --security
   ```
   <img width="780" height="106" alt="Screenshot 2026-06-20 083254" src="https://github.com/user-attachments/assets/1c8a7f5e-6668-4a35-b6b7-5f26580ed01d" />


3. I upgraded the general packages to ensure the system was fully up to date:
   ```bash
   sudo yum -y upgrade
   ```
<img width="737" height="52" alt="Screenshot 2026-06-20 083326" src="https://github.com/user-attachments/assets/3b46bc0a-31a7-4973-acec-8c60d95d650a" />


### Step 2.3: Install HTTPD
To view the install process and verify the package manager works, I installed the `httpd` package.

```bash
sudo yum install httpd -y
```
<img width="917" height="858" alt="Screenshot 2026-06-20 083418" src="https://github.com/user-attachments/assets/ae757b93-65e2-4bf0-9b2a-112792420fc5" />



---

## Task 3: Roll back a Package

**Objective:** Downgrade a package that has been updated by using the package manager to view history and perform an undo.

### Step 3.1: View Package History
I began by validating my current folder again with `pwd`, then listing the recent history of the `yum` package manager to find the ID of the `httpd` installation.

```bash
pwd
sudo yum history list
```
<img width="797" height="155" alt="Screenshot 2026-06-20 083748" src="https://github.com/user-attachments/assets/116fd50e-d767-4b1e-a635-9388f32ce4ed" />


### Step 3.2: View History Details
Next, I examined the specifics of the transaction ID 1. I ran the following command to view detailed information about the `httpd` installation:

```bash
sudo yum history info 1
```
<img width="862" height="507" alt="Screenshot 2026-06-20 102117" src="https://github.com/user-attachments/assets/7ba3b2b2-221c-485c-b167-643ac8b670e5" />

### Step 3.3: Undo the Installation
Finally, I reverted the installation by executing an `undo` command, referencing the transaction ID I identified in the previous step (ID 1).

```bash
sudo yum -y history undo 1
```
<img width="736" height="282" alt="Screenshot 2026-06-20 102341" src="https://github.com/user-attachments/assets/f50d9a53-3ee1-4659-a57f-e647906ec264" />
<img width="571" height="260" alt="Screenshot 2026-06-20 102247" src="https://github.com/user-attachments/assets/c9d8d660-5d79-40f0-b50c-1db98fbad331" />



---

## Task 4: Install the AWS Command Line Interface (AWS CLI)

**Objective:** Install and verify the AWS CLI on the Red Hat/Linux machine.

### Step 4.1: Verify Python and Pip
I needed to verify that Python was installed. I ran the following command and saw that Python 3 was present:

```bash
python3 --version
```
<img width="593" height="68" alt="Screenshot 2026-06-20 102407" src="https://github.com/user-attachments/assets/37b8e821-4f85-4cb0-a307-23325a76891e" />


I then checked if `pip` was installed. 
```bash
pip3 --version
```
<img width="686" height="72" alt="Screenshot 2026-06-20 102428" src="https://github.com/user-attachments/assets/6502cbc1-95fe-46fa-a439-2b6862edb6f9" />


### Step 4.2: Download the AWS CLI Installer
Using the `curl` command, I downloaded the official AWS CLI version 2 installation file into the current directory.

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```
<img width="902" height="217" alt="Screenshot 2026-06-20 102603" src="https://github.com/user-attachments/assets/2e0973c6-c667-492c-bc55-a8871a498317" />


### Step 4.3: Install and Verify the AWS CLI
I unzipped the downloaded file and executed the installation script with `sudo` to ensure it had the necessary permissions.

```bash
unzip awscliv2.zip
sudo ./aws/install
```
<img width="582" height="68" alt="Screenshot 2026-06-20 102715" src="https://github.com/user-attachments/assets/73d65f95-7931-48a0-9bc8-dcbc49f22949" />


To verify that the CLI was installed correctly, I ran the help command:
```bash
aws help
```
<img width="900" height="538" alt="Screenshot 2026-06-20 102727" src="https://github.com/user-attachments/assets/fabcfad9-5a2a-4330-806d-3a615b5c812b" />


---

## Task 5: Configure the AWS CLI to Connect to Your AWS Account

**Objective:** Configure the AWS CLI with the appropriate credentials, region, and output format to manage AWS resources.

### Step 5.1: Initial Configuration
I initiated the configuration wizard by typing `aws configure` in the terminal. I entered the following details when prompted:
*   **AWS Access Key ID:** *(Left blank as per lab instructions)*
*   **AWS Secret Access Key:** *(Left blank as per lab instructions)*
*   **Default region name:** `us-west-2`
*   **Default output format:** `json`

```bash
aws configure
```
<img width="1077" height="240" alt="Screenshot 2026-06-20 102943" src="https://github.com/user-attachments/assets/31d92676-dd6b-471e-8fea-191c1d7cae87" />


### Step 5.2: Manually Configure Credentials File
Instead of entering the keys through the prompts, I opened the credentials file directly using the `nano` editor to paste the provided Access Key, Secret Key, and Session Token from the lab details window.

```bash
sudo nano ~/.aws/credentials
```

*(I pasted the credentials into the editor, saved by pressing `Ctrl + O`, and exited by pressing `Ctrl + X`.)*

To verify that my configuration was successful, I retrieved my Instance ID from the AWS Management Console. I then used the AWS CLI to describe an attribute of that specific EC2 instance.

```bash
aws ec2 describe-instance-attribute --instance-id i-03ebedc54ae3a3bf0 --attribute instanceType
```
<img width="1050" height="286" alt="Screenshot 2026-06-20 103534" src="https://github.com/user-attachments/assets/fefc59c3-9dbb-42a7-834b-b7c3376c8e36" />
```
