# Creating a Website on S3

## Lab Report

### Objectives
After completing this lab, I was able to:

- Run AWS CLI commands that use IAM and Amazon S3 services
- Deploy a static website to an S3 bucket
- Create a script that uses the AWS CLI to copy files in a local directory to Amazon S3

---

## Step-by-Step Implementation

### 1. Setting Up the AWS CLI Environment

I connected to the CLI Host EC2 instance and configured the AWS CLI with the necessary credentials. I ran the `aws configure` command and entered the Access Key ID, Secret Access Key, default region (`us-west-2`), and output format (`json`).

```bash
aws configure
```

The configuration was successful with:
- **AWS Access Key ID**: `AKIAT5JFPNONU7ITM5W4`
- **AWS Secret Access Key**: `wLYgab4ZMuwFqyEifdtG7GzmnQrI1NYVVRw3Ipy`
- **Default region name**: `us-west-2`
- **Default output format**: `json`

<img width="1917" height="641" alt="Screenshot 2026-06-30 200925" src="https://github.com/user-attachments/assets/e2bd162a-619f-43ee-bd54-9d1c244625d3" />


---

### 2. Creating an S3 Bucket for the Website

I attempted to create an S3 bucket using the `aws s3api create-bucket` command. The bucket name was intended to be `chugo246`, but I made a syntax error by including angle brackets (`< >`) around the bucket name.

```bash
aws s3api create-bucket --bucket <chugo246> --region us-west-2 --create-bucket-configuration LocationConstraint=us-west-2
```

The command failed with the error:
```
-bash: chugo246: No such file or directory
```

This error occurred because the shell interpreted the angle brackets as redirection operators rather than literal characters. The correct syntax would be to use the bucket name without brackets:

```bash
aws s3api create-bucket --bucket chugo246 --region us-west-2 --create-bucket-configuration LocationConstraint=us-west-2
```


---

### 3. Creating an IAM User

I created a new IAM user named `awsS3user` using the AWS CLI. This user would later be used to manage S3 resources.

```bash
aws iam create-user --user-name awsS3user
```

The command returned the user details:

```json
{
    "User": {
        "UserName": "awsS3user",
        "Path": "/",
        "CreateDate": "2026-06-30T18:07:56Z",
        "UserId": "AIDAT5JFPNONZDMMFDN5J",
        "Arn": "arn:aws:iam::269050932123:user/awsS3user"
    }
}
```

<img width="1917" height="641" alt="Screenshot 2026-06-30 200925" src="https://github.com/user-attachments/assets/95f5fed2-8f51-4c60-a54c-9fd710fc4510" />

---

### 4. Creating a Login Profile for the IAM User

To allow the `awsS3user` to sign in to the AWS Management Console, I created a login profile with a password.

```bash
aws iam create-login-profile --user-name awsS3user --password Training123!
```

The command confirmed successful creation:

```json
{
    "LoginProfile": {
        "UserName": "awsS3user",
        "CreateDate": "2026-06-30T18:10:14Z",
        "PasswordResetRequired": false
    }
}
```

<img width="1157" height="212" alt="Screenshot 2026-06-30 201030" src="https://github.com/user-attachments/assets/51da2bfe-2816-44f5-8e76-bb51464ee7e6" />


---

### 5. Accessing the IAM User Console

I attempted to sign in to the AWS Management Console as the `awsS3user` using the IAM user sign-in page. The sign-in page prompted for the Account ID or alias, IAM username (`awsS3user`), and the password (`Training123!`).

The IAM user sign-in page displayed the standard AWS login interface with options to remember the account, show the password, and navigate to the sign-in page.

<img width="1417" height="795" alt="Screenshot 2026-06-30 201909" src="https://github.com/user-attachments/assets/813d499a-4332-4cec-b62d-7f2d5323b38f" />


---

### 6. AWS Management Console Navigation

After signing in, the AWS Management Console home page displayed the recently visited services section. The interface showed the region selector and various AWS service options.

<img width="1901" height="641" alt="Screenshot 2026-06-30 201321" src="https://github.com/user-attachments/assets/721b085c-cc04-4a6a-a2ef-074fcc923659" />


---

### 7. Attaching the Amazon S3 Full Access Policy to the IAM User

I attached the `AmazonS3FullAccess` managed policy to the `awsS3user` IAM user to grant them the necessary permissions to create and manage S3 buckets and objects.

First, I listed the available S3 policies to verify the correct policy ARN:

```bash
aws iam list-policies --query "Policies[?contains(PolicyName,'S3')]"
```

The output showed the available policies, including:
- `AmazonS3FullAccess` (ARN: `arn:aws:iam::aws:policy/AmazonS3FullAccess`)
- `AmazonS3ReadOnlyAccess` (ARN: `arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess`)

<img width="1168" height="537" alt="Screenshot 2026-06-30 203157" src="https://github.com/user-attachments/assets/ac386d3f-fce3-460e-8e92-20dd59fe8e14" />


I then attached the `AmazonS3FullAccess` policy to the `awsS3user`:

```bash
aws iam attach-user-policy --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess --user-name awsS3User
```

The command completed successfully with no output, indicating the policy was attached.

<img width="1356" height="40" alt="Screenshot 2026-06-30 203210" src="https://github.com/user-attachments/assets/733f48a4-b775-4517-b949-5ef9d0dd3bc5" />


---

### 8. Verifying the IAM User's Permissions in the Console

After refreshing the AWS Management Console as the `awsS3user`, I was now able to view S3 buckets. The bucket `chugo246` appeared in the list of general purpose buckets.

<img width="1912" height="552" alt="Screenshot 2026-06-30 203445" src="https://github.com/user-attachments/assets/23f03528-f949-4b1b-9ac6-e60d2659d56f" />


The bucket `chugo246` was successfully created in the `us-west-2` region.

---

### 9. Configuring Bucket Permissions for Static Website Hosting

I navigated to the **Permissions** tab of the `chugo246` bucket to configure public access settings.

#### Reviewing Block Public Access Settings

Initially, the bucket had **Block all public access** turned on, which would prevent the website from being publicly accessible.

<img width="1550" height="561" alt="Screenshot 2026-06-30 203515" src="https://github.com/user-attachments/assets/525107f8-2240-4b89-9c6d-884791754436" />


I edited the Block Public Access settings to allow public access for website hosting. I turned off **Block all public access** and confirmed the change by entering `confirm` in the confirmation field.

<img width="1567" height="590" alt="Screenshot 2026-06-30 203621" src="https://github.com/user-attachments/assets/7121700e-3cec-4ba6-9090-5600554d575b" />


<img width="723" height="351" alt="Screenshot 2026-06-30 203725" src="https://github.com/user-attachments/assets/2040f32d-b4ff-4455-aff3-085208930003" />


#### Configuring Object Ownership

I also edited the Object Ownership settings. Since I needed to enable ACLs to make objects publicly readable, I enabled ACLs and chose the **Bucket owner preferred** setting. This ensures that new objects written to the bucket with the `bucket-owner-full-control` canned ACL are owned by the bucket owner.

<img width="1512" height="692" alt="Screenshot 2026-06-30 203813" src="https://github.com/user-attachments/assets/b46a0dd6-69db-4838-bbde-25dafadd9eb3" />


A success message confirmed the Object Ownership was updated.

<img width="1538" height="71" alt="Screenshot 2026-06-30 203825" src="https://github.com/user-attachments/assets/582d9ca3-f8fa-4131-ad34-615bdc3b3c80" />


---

### 10. Extracting and Preparing the Website Files

I navigated to the directory containing the lab files and extracted the static website archive:

```bash
cd ~/sysops-activity-files
tar xvzf static-website-v2.tar.gz
```

The extraction created a `static-website/` directory containing:
- `css/` folder with `styles.css`
- `images/` folder with multiple image files
- `index.html` file

```bash
cd static-website
ls
```

The output confirmed the directory structure:
```
css   images   index.html
```

<img width="808" height="340" alt="Screenshot 2026-06-30 204514" src="https://github.com/user-attachments/assets/c37f8f52-381c-4eca-b09f-ec7768955b21" />


<img width="533" height="55" alt="Screenshot 2026-06-30 204537" src="https://github.com/user-attachments/assets/15940a57-139e-4343-8859-f97f150010ae" />


---

### 11. Enabling Static Website Hosting on the S3 Bucket

I enabled static website hosting on the `chugo246` bucket using the `aws s3 website` command:

```bash
aws s3 website s3://chugo246/ --index-document index.html
```

This configured the bucket to serve `index.html` as the default document when users access the website endpoint.

---

### 12. Uploading Website Files to the S3 Bucket

I uploaded all the website files to the S3 bucket with public read permissions using the `aws s3 cp` command with the `--recursive` and `--acl public-read` flags:

```bash
aws s3 cp /home/ec2-user/sysops-activity-files/static-website/ s3://chugo246/ --recursive --acl public-read
```

The command uploaded the following files:
- `css/styles.css`
- `images/Coffee-Shop.png`
- `images/Cafe-Owners.png`
- `index.html`
- `images/Cookies.png`
- `images/Cake-Vitrine.png`
- `images/Coffee-and-Pastries.png`
- `images/Strawberry-Tarts.png`
- `images/Strawberry-&-Blueberry-Tarts.png`
- `images/Cup-of-Hot-Chocolate.png`

<img width="811" height="358" alt="Screenshot 2026-06-30 204714" src="https://github.com/user-attachments/assets/4a1a70df-5ac6-40eb-80b2-f32f0dc8fd0d" />


---

### 13. Verifying the Uploaded Files

I verified the files were successfully uploaded to the bucket using the `aws s3 ls` command:

```bash
aws s3 ls chugo246/
```

The output showed:
```
PRE css/
PRE images/
2026-06-30 18:47:05    2980 index.html
```

<img width="652" height="101" alt="Screenshot 2026-06-30 204805" src="https://github.com/user-attachments/assets/37c74708-2c0f-4793-95ac-441dbf100a92" />


---

### 14. Accessing the Static Website

The bucket was now configured as a static website. The website endpoint was displayed in the S3 console under the **Properties** tab, **Static website hosting** section:

```
http://chugo246.s3-website-us-west-2.amazonaws.com
```

<img width="1525" height="430" alt="Screenshot 2026-06-30 204847" src="https://github.com/user-attachments/assets/22cc5988-08f3-4dd4-9b88-a4c38727745d" />


I accessed the website endpoint in a web browser, and the Café website loaded successfully, displaying the content from `index.html` along with the CSS styling and images.

<img width="1785" height="971" alt="Screenshot 2026-06-30 204933" src="https://github.com/user-attachments/assets/e1461b59-df0b-452a-8a66-079e23423545" />


---

### 15. Creating a Deployment Script

To automate future updates to the website, I created a shell script named `update-website.sh`:

```bash
cd ~
touch update-website.sh
```

I edited the script using `vi`:

```bash
vi update-website.sh
```

I wrote the following script to sync the local website files to the S3 bucket with public read permissions:

```bash
#!/bin/bash
aws s3 cp /home/ec2-user/sysops-activity-files/static-website/ s3://chugo246/ --recursive --acl public-read
```

I made the script executable:

```bash
chmod +x update-website.sh
```

<img width="587" height="61" alt="Screenshot 2026-06-30 205058" src="https://github.com/user-attachments/assets/943ed669-dc28-43ea-a112-f6923866757c" />


<img width="725" height="77" alt="Screenshot 2026-06-30 205228" src="https://github.com/user-attachments/assets/8043bfcb-4812-4786-8889-d2556b11ef8c" />


---

### 16. Updating the Website Content

I modified the `index.html` file to update the website content. I added new text to the page, including:

> "Tea Coffee Latte Hot Chocolate Yes, we have it!"
> "Our tarts are always a customer favorite!"
> "About Us"

<img width="811" height="503" alt="Screenshot 2026-06-30 205254" src="https://github.com/user-attachments/assets/21eecf30-d16d-400c-b66b-fd9d21a71c49" />


<img width="782" height="328" alt="Screenshot 2026-06-30 205426" src="https://github.com/user-attachments/assets/9a97a5a9-5948-4352-9e86-6d5cb6a44572" />


<img width="307" height="262" alt="Screenshot 2026-06-30 205457" src="https://github.com/user-attachments/assets/7e4dd9a0-a3d0-4097-81af-36fd37aba1ce" />


---

### 17. Running the Deployment Script

After making changes to the website, I ran the `update-website.sh` script to deploy the updated files to S3:

```bash
./update-website.sh
```

The script executed the `aws s3 cp` command, uploading the updated `index.html` and any other changed files to the bucket.

<img width="885" height="598" alt="Screenshot 2026-06-30 211230" src="https://github.com/user-attachments/assets/62f46e66-2829-4181-b624-d8ad9df57d41" />


<img width="875" height="598" alt="Screenshot 2026-06-30 211339" src="https://github.com/user-attachments/assets/8ad49bf0-6008-4d82-b03e-be9b60fbb99c" />

<img width="868" height="421" alt="Screenshot 2026-06-30 211513" src="https://github.com/user-attachments/assets/64d33d14-7eb5-42cc-931e-2f52038d1cec" />

---

### 18. Verifying the Updated Website

I refreshed the website in my browser to verify the changes were deployed. The updated content appeared, showing the new text and confirming the deployment script worked correctly.

<img width="1403" height="442" alt="Screenshot 2026-06-30 211526" src="https://github.com/user-attachments/assets/402b3e97-7a78-4e24-b077-b3db95284ab5" />


---

## What I Learned

During this lab, I learned the following key concepts:

1. **IAM Permissions Management**: How to attach managed policies to IAM users using the AWS CLI, granting them the necessary permissions to interact with S3 services.

2. **S3 Bucket Configuration for Static Website Hosting**: How to:
   - Enable static website hosting on an S3 bucket
   - Configure Block Public Access settings to allow public access
   - Set Object Ownership to enable ACL-based permissions

3. **Website Deployment**: How to upload static website files (HTML, CSS, images) to an S3 bucket using `aws s3 cp` with recursive and ACL options.

4. **Scripting for Automation**: How to create and use bash scripts to automate the deployment process, making updates quick and consistent.

5. **Command Syntax**: The correct syntax for S3 commands, including the importance of proper path formatting and the use of flags like `--recursive`, `--acl public-read`, and `--index-document`.

6. **Troubleshooting**: How to identify and correct command syntax errors, such as removing angle brackets from bucket names and ensuring proper path references.

---

## Conclusion

In this lab, I successfully:

- **Ran AWS CLI commands** that used IAM and Amazon S3 services to create users, attach policies, and manage buckets
- **Deployed a static website** to an S3 bucket by enabling static website hosting and uploading all necessary files with public read permissions
- **Created a deployment script** using a bash script with AWS CLI commands to copy files from a local directory to Amazon S3

The Café website is now live and accessible at the S3 website endpoint. Any future updates to the website content can be easily deployed by modifying the local files and running the `update-website.sh` script, making the deployment process efficient and repeatable.
