# Introduction to AWS Identity and Access Management (IAM) - Lab Report

## Lab Overview
In this lab, I explored the core concepts of AWS Identity and Access Management (IAM). The primary objective was to understand how to securely manage access to AWS resources by creating password policies, setting up user groups with specific permissions, assigning users to those groups, and verifying the effects of those permissions.

## Objectives Achieved
After completing this lab, I successfully:
*   Created and applied an IAM password policy.
*   Explored pre-created IAM users and user groups.
*   Inspected IAM policies as applied to the pre-created user groups.
*   Added users to user groups with specific capabilities active.
*   Located and used the IAM sign-in URL.
*   Experimented with the effects of policies on service access.

---

## Task 1: Created an Account Password Policy
I started by setting a strict password policy for the AWS account. I navigated to the IAM dashboard, selected **Account settings**, and chose **Change password policy**. 
I configured the policy to require a minimum password length of **10 characters**. I also checked the options to require at least one uppercase letter, one lowercase letter, one number, and one non-alphanumeric character. Finally, I set the password expiration to **90 days** and prevented the reuse of the last **5 passwords** before saving the changes.

<img width="1753" height="798" alt="Screenshot 2026-08-03 190734" src="https://github.com/user-attachments/assets/f3560090-bc51-4012-adeb-5b6e3637b117" />


---

## Task 2: Explored Users and User Groups
Next, I navigated to the **Users** section in the IAM console to explore the pre-created users for this lab: `user-1`, `user-2`, and `user-3`. 
I checked `user-1` and found they had no permissions and were not a member of any user groups.

I then reviewed the **User groups** section. I examined the existing groups:
*   **EC2-Support**: I viewed its permissions and saw it was attached to a managed AWS policy called `AmazonEC2ReadOnlyAccess`. This provided read-only permissions to EC2 resources.
*   **S3-Support**: I examined this group and found it attached to the `AmazonS3ReadOnlyAccess` policy, providing read-only access to S3 resources.
*   **EC2-Admin**: I inspected this group and noted it had a **Customer inline policy** named `EC2-Admin-Policy`. Unlike the others, this custom policy granted permissions to list, describe, start, and stop EC2 instances.

---

## Task 3: Added Users to User Groups
Based on the business scenario provided in the lab, I was tasked with giving specific users access based on their job roles. 

First, I added `user-1` to the **S3-Support** group. I did this by navigating into the `S3-Support` group, going to the **Users** tab, selecting **Add users**, and checking the box for `user-1`.

<img width="1887" height="751" alt="Screenshot 2026-08-03 191130" src="https://github.com/user-attachments/assets/d4dd2357-239d-4a47-abde-2e380075d9e4" />


Next, I added `user-2` to the **EC2-Support** group using the same process. This granted `user-2` the read-only EC2 permissions required for a support role.

<img width="1877" height="766" alt="Screenshot 2026-08-03 191222" src="https://github.com/user-attachments/assets/f6964a4b-fd93-489b-9bbd-782ad575924e" />


Finally, I added `user-3` to the **EC2-Admin** group. This gave `user-3` the permissions to manage EC2 instances, start, and stop them.

<img width="1896" height="735" alt="Screenshot 2026-08-03 191205" src="https://github.com/user-attachments/assets/f7186f03-85c2-46c3-ad2c-3c9a3972ed49" />


---

## Task 4: Signed In and Tested User Permissions
To validate that the permissions were working correctly, I navigated to the IAM Dashboard and copied the **IAM users sign-in URL** to my text editor.

### Tested `user-1` (S3 Support)
I opened a private browser window, pasted the sign-in URL, and logged in as `user-1` using the provided password. 
Once logged in, I navigated to the **Amazon S3** console. I was successfully able to view the S3 buckets available in the account.

<img width="1427" height="892" alt="Screenshot 2026-08-03 191449" src="https://github.com/user-attachments/assets/b21c13ef-2c82-427d-b004-0fe055868b32" />


To test the limitations, I navigated to the **Amazon EC2** console. I immediately saw an error message stating: **"You are not authorized to perform this operation."**

<img width="1701" height="915" alt="Screenshot 2026-08-03 191523" src="https://github.com/user-attachments/assets/4338dd60-e6d0-434e-92a2-ff324c6785b4" />


### Tested `user-2` (EC2 Support)
I signed out of `user-1` and signed back into the private window as `user-2`. 
I navigated to the EC2 console. Since `user-2` has read-only access, I was able to successfully view the EC2 instance list.

<img width="1905" height="985" alt="Screenshot 2026-08-03 191735" src="https://github.com/user-attachments/assets/6cd5ab4a-862b-4721-bdb6-babfee51b3e4" />


I then attempted to stop the running EC2 instance to test the permissions further. As expected, I received a **"Failed to stop the instance"** error message because the read-only policy prevents modifications to the EC2 state.

<img width="1882" height="567" alt="Screenshot 2026-08-03 191812" src="https://github.com/user-attachments/assets/50a619fa-8014-486b-8f65-43eadf2fc5ec" />


### Tested `user-3` (EC2 Admin)
Finally, I signed out of `user-2` and signed in as `user-3`. 
I returned to the EC2 console, selected the same running instance, chose **Instance state** > **Stop instance**, and confirmed the action. Because `user-3` is part of the `EC2-Admin` group, the instance stopped successfully.

<img width="1807" height="298" alt="Screenshot 2026-08-03 192045" src="https://github.com/user-attachments/assets/89d6b380-e7ed-4c50-9aa6-61f1f5e003fb" />


## Conclusion
In conclusion, I successfully demonstrated the ability to manage access within AWS using IAM. I created a custom password policy, assigned users to groups with specific AWS-managed and custom inline policies, and verified the principles of least privilege by confirming that users could only access the specific services they were assigned to and could only perform allowed actions (e.g., `user-3` could stop an instance while `user-2` could not).
```
