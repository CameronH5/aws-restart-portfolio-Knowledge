# Lab Report: Using Amazon Inspector for Vulnerability Assessment and Remediation

## Lab Overview

In this lab, I utilized Amazon Inspector to scan for vulnerabilities in AWS resources, specifically AWS Lambda functions. I learned how to activate Amazon Inspector, interpret vulnerability reports, and remediate findings. The lab simulated a scenario where developers at AnyCompany needed an automated security tool to scan for vulnerable software packages and code vulnerabilities in their AWS Lambda functions.

---

## Objectives

After completing this lab, I was able to:

- **Activate Amazon Inspector**
- **Analyze and interpret vulnerability findings**
- **Remediate the vulnerabilities found by Amazon Inspector**

---

## Task 1: Activate Amazon Inspector

In this task, I activated and ran the Amazon Inspector service to continuously scan the Lambda functions.

### Steps Performed:

**Step 6:** In the AWS Management Console, in the search bar at the top, I typed and selected **Inspector**.

**Step 7:** I opened the panel on the left and chose **Activate Inspector** to activate it for my account.

<img width="1905" height="376" alt="Screenshot 2026-08-02 143530" src="https://github.com/user-attachments/assets/5e19c58d-1d2c-423c-aecf-c7c0e597aa53" />


*Screenshot showing the Activate Inspector page with the activation button*

**Step 8:** Under **Activate Inspector**, I clicked the **Activate Inspector** button.

After activation, I saw a message at the top saying "Welcome to Inspector. Your first scan is underway."

**Step 9:** I dismissed any survey prompts by choosing **Cancel** and closed all banner messages.

**Step 10:** I refreshed the page periodically until I saw the **Dashboard > Summary > Environment coverage > Lambda functions** at **100%**.

<img width="1092" height="547" alt="Screenshot 2026-08-02 143709" src="https://github.com/user-attachments/assets/f39a60f7-1904-4ae6-b57a-4148e2a6eb76" />



The dashboard showed my account number and activation status for AWS Lambda. By default, scanning was activated for Amazon EC2, Amazon ECR, and AWS Lambda standard scanning.

---

## Task 2: Reviewing the Inspected Resources

While waiting for the scan to finish, I reviewed the current lab environment to understand what specific resources were being scanned by Amazon Inspector.

### Task 2.1: Reviewing Lambda Functions

**Step 11:** From the **Findings** section on the left, I chose **All findings**.

**Step 12:** Three rows were displayed, one for each vulnerability within the Lambda function. I observed the following key details:
- **Severity:** Medium
- **Impacted resource:** The affected Lambda function
- **Title:** The reason for the finding

<img width="1847" height="702" alt="Screenshot 2026-08-02 143727" src="https://github.com/user-attachments/assets/a6f18fe0-eb7a-4750-abf6-6537b5ff7547" />


*Screenshot showing the findings list with all vulnerabilities*

**Step 13:** I selected the radio button for finding **CVE-2023-32681 - requests**, which opened a pane containing the vulnerability information.

![Findings details pane](./Screenshot%202026-08-02%20143833.png)

*Screenshot showing the detailed vulnerability information for CVE-2023-32681*

**Step 14:** Within the information pane, under the **Vulnerability details** section, I clicked the external link next to the **Vulnerability ID**.

<img width="1551" height="577" alt="Screenshot 2026-08-02 143833" src="https://github.com/user-attachments/assets/bd3c890a-a3fd-43f5-bc11-70824e6d34b0" />


*Screenshot showing the NVD page with detailed vulnerability information*

The link opened a new browser tab to the vulnerability detail webpage from the **National Vulnerability Database (NVD)**, which is maintained by NIST. This page offered more detailed information about the vulnerability.

<img width="1787" height="830" alt="Screenshot 2026-08-02 143851" src="https://github.com/user-attachments/assets/67018040-6fc4-474c-8234-07d551c681db" />

**Step 15:** Within the information pane, I located the **Remediation** section.

<img width="570" height="143" alt="Screenshot 2026-08-02 143926" src="https://github.com/user-attachments/assets/1ce963c4-1272-4cda-aeec-3d9f8de65b53" />


*Screenshot showing the remediation recommendation*

The issue was that the **requests** package was vulnerable and outdated, and the recommendation was to upgrade the package.

---

## Task 3: Remediating the Vulnerability Findings

In this task, I analyzed the findings reported by Amazon Inspector, interpreted the vulnerability details, and updated the Lambda functions to remediate the vulnerabilities.

### Task 3.1: Remediating Lambda Function Package Vulnerabilities

**Step 16:** In the AWS Management Console search box, I searched for and selected **Lambda**.

**Step 17:** From the list of Lambda functions, I chose the **get-request** function.

**Step 18:** Within the Lambda function code editor's file browser, I selected **requirements.txt**.

**Step 19:** I removed the version number and equal signs from `requests==2.0.0` so that the line became only `requests`.

The `requirements.txt` file tells AWS Lambda which Python packages are required to run the function. When no version number is specified, the latest version of the package will be installed by default. This ensures the Lambda function uses the latest version of the package.

<img width="996" height="575" alt="Screenshot 2026-08-02 144150" src="https://github.com/user-attachments/assets/5611443d-480c-41be-af1d-dc9d00f13dce" />


*Screenshot showing the updated requirements.txt file with just "requests"*

**Step 20:** I clicked the **Deploy** button to deploy the function.

A banner was displayed with the message "Successfully updated the function get-request."

This latest deployment triggered Amazon Inspector to initiate a new scan of the function.

**Step 21:** In the AWS Management Console search box, I searched for and selected **Amazon Inspector**.

**Step 22:** In the navigation pane, under **Findings**, I chose **All findings**.

**Step 23:** In the findings dashboard, under **finding status**, I changed the selection from **Active** to **Closed**.

**Step 24:** In the list of closed findings, I saw **CVE-2023-32681 - requests**, confirming the successful remediation of the vulnerability.

<img width="1828" height="622" alt="Screenshot 2026-08-02 144347" src="https://github.com/user-attachments/assets/52d3fa6c-5713-4ab1-b3eb-44294f19e77b" />


*Screenshot showing the closed findings list with all vulnerabilities marked as Closed*

**Note:** It took a few minutes for the scan to initiate and finish. I used the refresh button to view the latest status.

**Step 25:** In the navigation pane, under **Resources coverage**, I chose **Lambda functions**.

**Step 26:** I expanded the width of the **Last scanned** column to display the full timestamp.

<img width="1830" height="675" alt="Screenshot 2026-08-02 144449" src="https://github.com/user-attachments/assets/8cd937e3-c865-41a9-b3aa-0a093990c439" />


*Screenshot showing the Lambda functions with updated scan timestamps*

I confirmed that the most recently scanned Lambda function had an updated timestamp, indicating the new scan was completed.

---

## Conclusion

Congratulations! I successfully completed the lab and demonstrated the ability to:

✅ **Activated and configured Amazon Inspector** - Successfully activated the service for my account and verified Lambda function coverage at 100%

✅ **Analyzed and interpreted vulnerability findings** - Reviewed the findings dashboard, examined vulnerability details, and accessed external vulnerability databases for additional context

✅ **Remediated the vulnerabilities found by Amazon Inspector** - Updated the Lambda function's requirements.txt file to use the latest version of the requests package, deployed the changes, and confirmed the successful remediation by verifying the vulnerability status changed to "Closed"

This lab demonstrated how Amazon Inspector can be effectively used to automatically detect and help remediate vulnerabilities in AWS Lambda functions, enabling continuous security monitoring in the development lifecycle.
