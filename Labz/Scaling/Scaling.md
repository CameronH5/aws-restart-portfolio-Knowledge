# Scaling and Load Balancing Your Architecture - Lab Summary

## Overview

In this lab, I used Elastic Load Balancing (ELB) and Amazon EC2 Auto Scaling to load balance and automatically scale my infrastructure. ELB automatically distributes incoming application traffic across multiple EC2 instances, providing fault tolerance. Auto Scaling helped me maintain application availability by automatically adjusting the number of EC2 instances based on defined conditions.

---

## Objectives

After completing this lab, I successfully accomplished the following:

- Created an AMI from an EC2 instance
- Created a load balancer
- Created a launch template and an Auto Scaling group
- Configured an Auto Scaling group to scale new instances within private subnets
- Used Amazon CloudWatch alarms to monitor infrastructure performance

---

## Task 1: Creating an AMI for Auto Scaling

I created an AMI from the existing Web Server 1 instance to save the contents of the boot disk so that new instances could be launched with identical content.

**Actions performed:**

1. Navigated to the AWS Management Console and opened the Amazon EC2 Management Console
2. In the Instances section, I selected the **Web Server 1** instance which was in a **Running** state
3. From the Actions dropdown, I chose **Image and templates > Create image**
4. Configured the following options:
   - Image name: `web_server_AMI`
   - Image description: `Lab_AMI for web_server`
5. Clicked **Create image**

<img width="1327" height="580" alt="Screenshot 2026-08-17 182439" src="https://github.com/user-attachments/assets/b7c18e67-381d-4089-895b-94e99c5fb206" />



---

## Task 2: Creating a Load Balancer

I created an Application Load Balancer to distribute traffic across multiple EC2 instances and Availability Zones.

**Actions performed:**

1. Navigated to **Load Balancers** in the EC2 console
2. Clicked **Create load balancer** and chose **Application Load Balancer**
3. Configured basic settings:
   - Load balancer name: `LabELB`

<img width="1357" height="630" alt="Screenshot 2026-08-17 182616" src="https://github.com/user-attachments/assets/7eab546d-9d4a-4213-9de4-b96347b16293" />


4. Network mapping:
   - VPC: **Lab VPC**
   - Selected both Availability Zones
   - First Availability Zone: **Public Subnet 1** (10.0.0.0/24)
   - Second Availability Zone: **Public Subnet 2** (10.0.1.0/24)

<img width="1855" height="731" alt="Screenshot 2026-08-17 182650" src="https://github.com/user-attachments/assets/41fdcb89-25ab-4397-8361-571137915208" />


5. Security groups:
   - Removed the default security group
   - Selected **Web Security Group**

<img width="1817" height="266" alt="Screenshot 2026-08-17 182745" src="https://github.com/user-attachments/assets/8050c39a-d688-47cf-8fcb-db16916a478b" />


6. Created a target group:
   - Target type: **Instances**
   - Target group name: `lab-target-group`

<img width="1471" height="710" alt="Screenshot 2026-08-17 183016" src="https://github.com/user-attachments/assets/2d35932b-7d98-4b00-91a4-368c0e820c1a" />

<img width="1867" height="675" alt="Screenshot 2026-08-17 183117" src="https://github.com/user-attachments/assets/d2ecd380-993b-4bce-ad10-3738364ef4c1" />

7. After the target group was created, I refreshed the listeners section and selected `lab-target-group` from the Forward to dropdown

<img width="1688" height="487" alt="Screenshot 2026-08-17 183316" src="https://github.com/user-attachments/assets/b3558220-e4a7-4967-902e-1e12de4f435b" />


8. Clicked **Create load balancer**
9. Copied the DNS name of the load balancer and saved it in a text editor for later use

<img width="1897" height="327" alt="Screenshot 2026-08-17 183351" src="https://github.com/user-attachments/assets/5fd6d43a-11bf-41af-9102-393050e81d53" />



---

## Task 3: Creating a Launch Template

I created a launch template for my Auto Scaling group to specify configuration information for EC2 instances, including the AMI, instance type, security group, and other settings.

**Actions performed:**

1. Navigated to **Launch Templates** in the EC2 console
2. Clicked **Create launch template**
3. Configured:
   - Launch template name: `lab-app-launch-template`
   - Template version description: `Web server for the load test app`
   - Enabled Auto Scaling guidance

<img width="1230" height="598" alt="Screenshot 2026-08-17 183552" src="https://github.com/user-attachments/assets/dc735752-9b03-46bf-9612-718f70ad6748" />


4. Application and OS Images:
   - Chose the **My AMIs** tab
   - Selected **Web Server AMI** (ami-047b98af391dc5c84)

<img width="1292" height="675" alt="Screenshot 2026-08-17 183628" src="https://github.com/user-attachments/assets/16749947-9f3b-4ee3-b13d-9c893f2f001d" />


5. Instance type: Selected **t3.micro**

<img width="1251" height="218" alt="Screenshot 2026-08-17 183731" src="https://github.com/user-attachments/assets/a6ed8533-274c-44f8-873c-8df8e2e01cb7" />


6. Key pair: Set to **Don't include in launch template**
7. Network settings:
   - Security group: **Web Security Group**

<img width="1177" height="513" alt="Screenshot 2026-08-17 183811" src="https://github.com/user-attachments/assets/7c0a22fb-6b4d-4353-9689-1647834c0399" />


8. Clicked **Create launch template**
9. Received confirmation: "Successfully created lab-app-launch-template"

<img width="1917" height="437" alt="Screenshot 2026-08-17 183845" src="https://github.com/user-attachments/assets/da8b869c-4f81-4288-99f4-52f3371d78c3" />


---

## Task 4: Creating an Auto Scaling Group

I used the launch template to create an Auto Scaling group with specific capacity settings and scaling policies.

**Actions performed:**

1. Selected **lab-app-launch-template** and chose **Create Auto Scaling group** from the Actions menu

<img width="1616" height="670" alt="Screenshot 2026-08-17 183954" src="https://github.com/user-attachments/assets/31a667d7-87ab-4c87-a99d-69f542a5db62" />


2. Configured:
   - Auto Scaling group name: `Lab Auto Scaling Group`

3. Instance launch options:
   - VPC: **Lab VPC**
   - Availability Zones and subnets: **Private Subnet 1** (10.0.1.0/24) and **Private Subnet 2** (10.0.3.0/24)

<img width="910" height="535" alt="Screenshot 2026-08-17 184324" src="https://github.com/user-attachments/assets/f7acdba4-796d-4ce7-8f8b-7165b2cfc9c7" />


4. Advanced options:
   - Load balancing: **Attach to an existing load balancer**
   - Selected **Choose from your load balancer target groups**
   - Target group: **lab-target-group | HTTP**
   - Health check type: **ELB**

<img width="1762" height="645" alt="Screenshot 2026-08-17 184355" src="https://github.com/user-attachments/assets/71039f06-c018-4867-b933-313e3c7d67c8" />


5. Group size and scaling:
   - Desired capacity: `2`
   - Minimum capacity: `2`
   - Maximum capacity: `4`
   - Scaling policy: **Target tracking scaling policy**
   - Metric type: **Average CPU utilization**
   - Target value: `50`

<img width="1317" height="647" alt="Screenshot 2026-08-17 184513" src="https://github.com/user-attachments/assets/73aa8b8d-51f9-4097-83c6-286f1565919c" />
<img width="1425" height="582" alt="Screenshot 2026-08-17 184604" src="https://github.com/user-attachments/assets/e1c13ec8-fab6-49b2-ac28-5bbc389a9de1" />


6. Added tags:
   - Key: `Name`
   - Value: `Lab Instance`

<img width="1843" height="531" alt="Screenshot 2026-08-17 184706" src="https://github.com/user-attachments/assets/cb443d73-91c2-4640-811e-2121fc486609" />


7. Clicked **Create Auto Scaling group**

<img width="1902" height="326" alt="Screenshot 2026-08-17 185051" src="https://github.com/user-attachments/assets/0f3f154d-f553-4d47-ae01-06457b18106e" />


---

## Task 5: Verifying Load Balancing

I verified that the load balancer was working correctly by checking the instances and accessing the application.

**Actions performed:**

1. Navigated to **Instances** in the EC2 console
2. Observed two new instances named **Lab Instance** launched by Auto Scaling

<img width="1915" height="337" alt="Screenshot 2026-08-17 185225" src="https://github.com/user-attachments/assets/af779a49-cb2e-4847-86f5-77e51491a58e" />

<img width="1876" height="712" alt="Screenshot 2026-08-17 185617" src="https://github.com/user-attachments/assets/1533a3e8-248c-4d24-a24b-051ce24d5ced" />

3. Navigated to **Target Groups** and selected `lab-target-group`
4. Verified that both instances showed a **healthy** Health status
5. Opened a new web browser tab and pasted the load balancer DNS name
6. Confirmed the Load Test application appeared, confirming the load balancer was routing traffic correctly

---

## Task 6: Testing Auto Scaling

I tested the Auto Scaling functionality by generating load on the instances to trigger scaling.

**Actions performed:**

1. Navigated to **CloudWatch** in the AWS Management Console
2. In the Alarms section, viewed the two alarms created by Auto Scaling
3. Observed the **AlarmHigh** alarm in an **OK** state initially
4. Returned to the Load Test application browser tab
5. Chose **Load Test** next to the AWS logo to generate high CPU loads
6. Returned to CloudWatch and observed the following within 5 minutes:
   - **AlarmLow** changed to **OK** state
   - **AlarmHigh** changed to **In alarm** state
   - The CPU percentage chart showed increasing levels crossing the 50% threshold
7. Navigated back to EC2 Instances and observed more than two instances named **Lab Instance** running
8. Confirmed Auto Scaling created new instances in response to the alarm

---

## Task 7: Terminating Web Server 1

I terminated the Web Server 1 instance since it was no longer needed after creating the AMI.

**Actions performed:**

1. Selected **Web Server 1** (ensuring it was the only instance selected)
2. From the Instance state dropdown, chose **Terminate instance**
3. Clicked **Terminate** to confirm

---

## Conclusion

In this lab, I successfully learned how to:

- **Create an AMI** from an EC2 instance to enable consistent instance launches
- **Create a load balancer** to distribute traffic across multiple instances and Availability Zones
- **Create a launch template** and **Auto Scaling group** to automatically manage EC2 capacity
- **Configure Auto Scaling** to scale instances within private subnets
- **Use CloudWatch alarms** to monitor infrastructure performance and trigger automatic scaling based on CPU utilization

The lab demonstrated how Elastic Load Balancing and Auto Scaling work together to create a fault-tolerant, scalable architecture that can automatically adapt to changing demand while maintaining application availability and optimizing costs.
