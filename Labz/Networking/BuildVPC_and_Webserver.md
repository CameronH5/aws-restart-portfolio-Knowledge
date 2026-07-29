# Build Your VPC and Launch a Web Server – Lab Report

## Objectives

After completing this lab, I was able to:

- Create a virtual private cloud (VPC)
- Create subnets
- Configure a security group
- Launch an Amazon Elastic Compute Cloud (Amazon EC2) instance into a VPC

## Duration

This lab took approximately **45 minutes** to complete.

## Scenario

I used Amazon VPC to build a custom network for a Fortune 100 customer, following a provided architecture diagram. I created a VPC with public and private subnets across two Availability Zones, configured route tables, set up a security group, and launched an EC2 instance running a web server.

---

## Task 1: Create Your VPC

I began by using the VPC Wizard to create a VPC with an internet gateway, two subnets (one public, one private) in a single Availability Zone, and a NAT gateway for private subnet internet access.

1. I opened the AWS Management Console and navigated to **VPC**.
2. I chose **Create VPC** and configured the following options:
   - **Resources to create**: VPC and more
   - **Name tag auto-generation**: unchecked (manual naming)
   - **IPv4 CIDR**: `10.0.0.0/16`
   - **IPv6 CIDR block**: No IPv6
   - **Tenancy**: Default
   - **Number of Availability Zones**: 1
   - **Number of public subnets**: 1
   - **Number of private subnets**: 1
   - I expanded **Customize subnets CIDR blocks** and set:
     - Public subnet CIDR: `10.0.0.0/24`
     - Private subnet CIDR: `10.0.1.0/24`
   - **NAT gateways**: In 1 AZ
   - **VPC endpoints**: None
3. In the Preview pane, I assigned names:
   - VPC: **Lab VPC**
   - Public subnet: **Public Subnet 1**
   - Private subnet: **Private Subnet 1**
   - Route tables: **Public Route Table** and **Private Route Table**

   <img width="1845" height="750" alt="Screenshot 2026-07-29 164520" src="https://github.com/user-attachments/assets/8871bda8-7c23-41c6-8432-08463bdbae7e" />
   <img width="536" height="302" alt="Screenshot 2026-07-29 164558" src="https://github.com/user-attachments/assets/6288001d-7f69-473d-bd1a-2cd7ec68c984" />
   <img width="566" height="537" alt="Screenshot 2026-07-29 164612" src="https://github.com/user-attachments/assets/998c647c-4018-45ae-9ca0-183dcb6b9a30" />
   <img width="1183" height="437" alt="Screenshot 2026-07-29 164704" src="https://github.com/user-attachments/assets/bbff5f7b-d5b3-419c-a884-a22c82fc7575" />




4. I clicked **Create VPC**. The creation workflow completed successfully with a success message and detailed resource creation steps.

   <img width="692" height="717" alt="Screenshot 2026-07-29 165201" src="https://github.com/user-attachments/assets/332624b0-90a3-421e-a1e0-d4ecced49e57" />


5. I viewed the VPC details to confirm the configuration.

   <img width="1700" height="535" alt="Screenshot 2026-07-29 165216" src="https://github.com/user-attachments/assets/24319d19-9572-42e2-86c9-2049021a9f14" />


---

## Task 2: Create Additional Subnets

To provide high availability, I created two additional subnets in a second Availability Zone.

6. In the left navigation pane, I chose **Subnets**.
7. For the second public subnet, I clicked **Create subnet** and set:
   - VPC: Lab VPC
   - Subnet name: **Public Subnet 2**
   - Availability Zone: No preference (AWS automatically chose `us-west-2b`)
   - IPv4 CIDR: `10.0.2.0/24`

   <img width="1317" height="651" alt="Screenshot 2026-07-29 165558" src="https://github.com/user-attachments/assets/38cf5dad-dd21-4596-9316-d0f2f18cc77d" />
   <img width="1515" height="677" alt="Screenshot 2026-07-29 165631" src="https://github.com/user-attachments/assets/1f09d2ea-8273-4bbf-a564-f8a21dc6daec" />



8. I clicked **Create subnet**.
9. For the second private subnet, I repeated the process:
   - Subnet name: **Private Subnet 2**
   - IPv4 CIDR: `10.0.3.0/24`

   <img width="1455" height="738" alt="Screenshot 2026-07-29 173909" src="https://github.com/user-attachments/assets/ffa7bbc5-f0fd-4768-84cd-5e656b441aa0" />


10. Both subnets were created successfully.

---

## Task 3: Associate the Subnets and Add Routes

I needed to associate the new subnets with the appropriate route tables to ensure correct routing.

11. In the left navigation pane, I chose **Route Tables**.
12. I selected **Public Route Table**.
13. Under the **Subnet associations** tab, I clicked **Edit subnet associations** and selected **Public Subnet 2** (along with Public Subnet 1, which was already associated). I then saved the associations.

    <img width="1868" height="537" alt="Screenshot 2026-07-29 173947" src="https://github.com/user-attachments/assets/2b45f922-4c67-4a5d-97af-a63ac0ff59e9" />


14. Next, I selected **Private Route Table**.
15. I edited its subnet associations and selected **Private Subnet 2** (along with Private Subnet 1). I saved the changes.

    <img width="1455" height="738" alt="Screenshot 2026-07-29 173909" src="https://github.com/user-attachments/assets/100c9005-797a-4c35-b222-29bd31cdf75e" />
    <img width="1867" height="576" alt="Screenshot 2026-07-29 174004" src="https://github.com/user-attachments/assets/93c82229-badf-454c-9a4c-b9757ef94f3b" />



16. I verified that the private route table had a route to the NAT gateway for internet-bound traffic (0.0.0.0/0 → nat-...).

    <img width="1882" height="571" alt="Screenshot 2026-07-29 173812" src="https://github.com/user-attachments/assets/8bb3aa8f-2ac3-41db-84ac-36e8eefa6cdc" />


---

## Task 4: Create a VPC Security Group

I created a security group to act as a firewall for the web server, allowing HTTP traffic.

17. In the left navigation pane, I chose **Security Groups**.
18. I clicked **Create security group** and configured:
   - **Security group name**: Web Security Group
   - **Description**: Enable HTTP access
   - **VPC**: Lab VPC
19. Under **Inbound rules**, I added a rule:
   - **Type**: HTTP
   - **Source**: Anywhere IPv4 (0.0.0.0/0)
   - **Description**: Permit web requests

  <img width="1815" height="722" alt="Screenshot 2026-07-29 174133" src="https://github.com/user-attachments/assets/27af9d7a-0719-49c3-b2bd-5e3aa2fef842" />


20. I clicked **Create security group**.

---

## Task 5: Launch a Web Server Instance

Finally, I launched an EC2 instance with a user-data script to install Apache and serve a web application.

21. I navigated to the **EC2** console and chose **Launch instances**.
22. I provided the following settings:
   - **Name**: Web Server 1
   - **AMI**: Amazon Linux 2 AMI (HVM)
   - **Instance type**: t3.micro
   - **Key pair**: vockey

   <img width="1270" height="755" alt="Screenshot 2026-07-29 174242" src="https://github.com/user-attachments/assets/6469e9db-4e3f-4bb1-9dd9-496e61f07b7f" />
    



23. In **Network settings**, I edited:
   - **VPC**: Lab VPC
   - **Subnet**: Public Subnet 2
   - **Auto-assign public IP**: Enable
   - **Firewall (security groups)**: Select existing security group → **Web Security Group**

   <img width="1221" height="801" alt="Screenshot 2026-07-29 174346" src="https://github.com/user-attachments/assets/cedb4651-9e21-47ea-b323-06c05159ef0f" />
   <img width="1107" height="288" alt="Screenshot 2026-07-29 174413" src="https://github.com/user-attachments/assets/1b3608d2-062f-42d4-9f78-9e078193f695" />


24. I expanded **Advanced details** and pasted the following user data script into the **User data** field:

    ```bash
    #!/bin/bash
    #Install Apache Web Server and PHP
    yum install -y httpd mysql php
    #Download Lab files
    wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-RESTRT-1/267-lab-NF-build-vpc-web-server/s3/lab-app.zip
    unzip lab-app.zip -d /var/www/html/
    #Turn on web server
    chkconfig httpd on
    service httpd start
    ```

    <img width="1008" height="471" alt="Screenshot 2026-07-29 174444" src="https://github.com/user-attachments/assets/0998787a-c51e-43e4-b8a2-9ecd87054cca" />


25. I clicked **Launch instance**.
26. I viewed the instance in the Instances list and waited until its status checks passed (2/2).

    <img width="1115" height="188" alt="Screenshot 2026-07-29 174510" src="https://github.com/user-attachments/assets/b7b72be6-73bb-48d6-8db1-f5cd451f13de" />


27. After the instance was ready, I copied its **Public IPv4 DNS** from the Details tab.
28. I opened a new browser tab, pasted the DNS address, and pressed Enter.

    The web server responded with the success page showing instance metadata and CPU load, confirming the application was deployed correctly.

    <img width="928" height="326" alt="Screenshot 2026-07-29 180226" src="https://github.com/user-attachments/assets/083badf6-5024-4942-bcd6-88ae0b138072" />


---

## Final Architecture

The complete architecture matched the customer’s request: a VPC with two public and two private subnets across two Availability Zones, an internet gateway, a NAT gateway, correctly routed tables, and a security group allowing HTTP access, all hosting a functional web server.

<img width="1003" height="537" alt="Screenshot 2026-07-29 164233" src="https://github.com/user-attachments/assets/633c85bb-760f-4764-9d6a-0c97f5917738" />


---

## Summary

I successfully built a custom VPC, created and associated subnets, configured a security group, and launched a web server instance. All steps were performed according to the lab instructions, and the final verification confirmed that the web server was accessible and operational.
