<img width="1147" height="367" alt="Screenshot 2026-08-17 195552" src="https://github.com/user-attachments/assets/dcc1cae0-0fa7-4b1a-9dbc-b9d76c9562d4" /># Using Auto Scaling in AWS (Linux) - Lab Summary

## Lab Overview

In this lab, I used the AWS Command Line Interface (AWS CLI) to create an Amazon Elastic Compute Cloud (EC2) instance to host a web server and create an Amazon Machine Image (AMI) from that instance. I then used that AMI as the basis for launching a system that scales automatically under a variable load by using Amazon EC2 Auto Scaling. I also created an Elastic Load Balancer to distribute the load across EC2 instances created in multiple Availability Zones by the auto scaling configuration.

## Objectives

After completing this lab, I was able to do the following:

- Create an EC2 instance by using an AWS CLI command
- Create a new AMI by using the AWS CLI
- Create an Amazon EC2 launch template
- Create an Amazon EC2 Auto Scaling launch configuration
- Configure scaling policies and create an Auto Scaling group to scale in and scale out the number of servers based on a variable load

---

## Lab Execution Summary

### Task 1: Creating a New AMI for Amazon EC2 Auto Scaling

#### Task 1.1: Connecting to the Command Host Instance

I connected to the Command Host EC2 instance using EC2 Instance Connect from the AWS Management Console. Once connected, I confirmed that the instance was running in the us-west-2 region by running:

```bash
curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region
```

<img width="1147" height="367" alt="Screenshot 2026-08-17 195552" src="https://github.com/user-attachments/assets/3973ccb0-17b5-4821-835d-e0e70e4cdfc7" />
 - Confirmed region: "us-west-2"

#### Task 1.2: Configuring the AWS CLI

I configured the AWS CLI with the necessary credentials:

```bash
aws configure
```

I entered the following:
- AWS Access Key ID: (pressed Enter to use existing credentials)
- AWS Secret Access Key: (pressed Enter)
- Default region name: us-west-2
- Default output format: json

<img width="1902" height="211" alt="Screenshot 2026-08-17 200254" src="https://github.com/user-attachments/assets/64a4d097-f1fe-4808-b614-6c9ab69d32a4" />
 - AWS CLI configured with us-west-2 region

I then navigated to the user directory:
```bash
cd /home/ec2-user/
```

#### Task 1.3: Creating a New EC2 Instance

I inspected the UserData.txt script that was pre-installed:
```bash
more UserData.txt
```

This script performed initialization tasks, including:
- Updating installed software
- Installing a PHP web application
- Erasing history and security information

I then retrieved the necessary values from the AWS Management Console (KEYNAME, AMIID, HTTPACCESS, and SUBNETID) and ran the command to create a new EC2 instance:

```bash
aws ec2 run-instances --key-name vockey --instance-type t3.micro --image-id ami-0bc1eb8a42b7b5a37 --user-data file:///home/ec2-user/UserData.txt --security-group-id=03f208e4877f5603e --subnet-id subnet-030c52a20f0a3f754 --associate-public-ip-address --tag-specifications 'ResourceType=Instance,Tags={[Key:Name,Value=WebServer]}' --output text --query 'Instances[*].InstanceId'
```

<img width="1902" height="211" alt="Screenshot 2026-08-17 200254" src="https://github.com/user-attachments/assets/a9e4c1a0-1af8-40e1-9907-87a9fb1a84f4" />
 - EC2 instance creation command

I obtained the instance ID: `i-00387e83a37aa62bb`

I waited for the instance to be in a running state using:
```bash
aws ec2 wait instance-running --instance-ids i-00387e83a37aa62bb
```

<img width="995" height="62" alt="Screenshot 2026-08-17 200343" src="https://github.com/user-attachments/assets/9a1c592a-0036-42ad-bd20-86b0c12d262b" />
 - Instance running status confirmed

I retrieved the public DNS name of the new instance:
```bash
aws ec2 describe-instances --instance-id 1-00387e83a37aa62bb --query "Reservations[0].Instances[0].NetworkInterfaces[0].Association.PublicDNSName"
```

<img width="1796" height="92" alt="Screenshot 2026-08-17 200434" src="https://github.com/user-attachments/assets/ee147373-fddd-4a2a-9133-bd2ead7bf75c" />
 - Public DNS retrieved: `ec2-54-245-58-152.us-west-2.compute.amazonaws.com`

I verified the web server was running by opening a browser to:
```
http://ec2-54-245-58-152.us-west-2.compute.amazonaws.com/index.php
```

<img width="1560" height="511" alt="Screenshot 2026-08-17 200723" src="https://github.com/user-attachments/assets/69c25f3e-81f0-4a7d-9bf9-f47cfa12552b" />
 - Web server running with "Generate Load" option

#### Task 1.4: Creating a Custom AMI

I created a custom AMI (Amazon Machine Image) from the running instance:

```bash
aws ec2 create-image --name WebServerAMI --instance-id i-00387e83a37aa62bb
```

<img width="1085" height="150" alt="Screenshot 2026-08-17 200824" src="https://github.com/user-attachments/assets/36924e8d-3c1f-4242-8048-be1ce48d3b9c" />
 - AMI created with ID: `ami-044b0a7988a00b0d3`

---

### Task 2: Creating an Auto Scaling Environment

#### Task 2.1: Creating an Application Load Balancer

From the EC2 Management Console, I created an Application Load Balancer named `WebServerELB`.

<img width="1386" height="707" alt="Screenshot 2026-08-17 201433" src="https://github.com/user-attachments/assets/246e0cf9-62a3-49ac-87fe-21d8c0f16a37" />
 - Creating Application Load Balancer with Basic configuration

<img width="1835" height="735" alt="Screenshot 2026-08-17 201507" src="https://github.com/user-attachments/assets/481e0756-98f1-4572-8cbc-8bd8dc1255fa" />
 - Network mapping with Lab VPC and Availability Zones selected (us-west-2a and us-west-2b with Public Subnet 1 and Public Subnet 2)

<img width="1782" height="270" alt="Screenshot 2026-08-17 201533" src="https://github.com/user-attachments/assets/20c5086c-1412-435e-991b-e11e2692dd6b" />
 - Security group selection - I selected `HTTPAccess` security group

I configured the following:
- **Load balancer name**: webServerELB
- **VPC**: Lab VPC
- **Availability Zones**: Selected both (us-west-2a with Public Subnet 1, us-west-2b with Public Subnet 2)
- **Security groups**: HTTPAccess

I created a target group named `webserver-app`:

<img width="1537" height="353" alt="Screenshot 2026-08-17 201707" src="https://github.com/user-attachments/assets/7c3f63ad-9c64-4ddc-bee3-6ca467c64db1" />
 - Health check path set to `/index.php`

<img width="1871" height="697" alt="Screenshot 2026-08-17 201920" src="https://github.com/user-attachments/assets/10b1fce3-e07b-4028-9b0b-c12b81c88968" />
 - Target group created with no targets registered initially

<img width="1666" height="682" alt="Screenshot 2026-08-17 202007" src="https://github.com/user-attachments/assets/f748b65a-90ef-40a6-abcf-3fb919e01a09" />
 - Listener configured with HTTP:80 and forwarding to webserver-app target group

The load balancer was successfully created and I copied the DNS name for later use.

#### Task 2.2: Creating a Launch Template

I created a launch template for the Auto Scaling group:

<img width="978" height="525" alt="Screenshot 2026-08-17 202153" src="https://github.com/user-attachments/assets/343766d2-1266-4aad-8aee-818432b76061" />
 - Launch template named `web-app-launch-template`

<img width="1267" height="231" alt="Screenshot 2026-08-17 202248" src="https://github.com/user-attachments/assets/bee2c219-ed1b-4e94-9b87-3627fad38eec" />
 - Instance type selected as `t3.micro`

<img width="1192" height="245" alt="Screenshot 2026-08-17 202320" src="https://github.com/user-attachments/assets/59bd3ac2-019a-45dc-8a0b-85985657addb" />
 - Security group `HTTPAccess` selected

I configured the following:
- **Launch template name**: web-app-launch-template
- **Description**: A web server for the load test app
- **Auto Scaling guidance**: Selected
- **AMI**: WebServerAMI (from My AMIs tab)
- **Instance type**: t3.micro
- **Key pair**: Don't include in launch template
- **Security group**: HTTPAccess

#### Task 2.3: Creating an Auto Scaling Group

I created an Auto Scaling group using the launch template:

<img width="1198" height="581" alt="Screenshot 2026-08-17 202548" src="https://github.com/user-attachments/assets/8fab15f4-7e8f-43ab-b726-094740b984a3" />
 - Auto Scaling group named "Web App Auto Scaling Group"

<img width="965" height="420" alt="Screenshot 2026-08-17 202627" src="https://github.com/user-attachments/assets/c041d74d-25e4-4513-ae87-e59328071012" />
 - Network configuration with Private Subnet 1 and Private Subnet 2 selected

<img width="1353" height="652" alt="Screenshot 2026-08-17 202657" src="https://github.com/user-attachments/assets/42e8a82a-ffcc-4b81-b9c4-2c2381a981dd" />
 - Configured to attach to existing load balancer, selecting target group webserver-app, and enabled Elastic Load Balancing health checks

<img width="1162" height="667" alt="Screenshot 2026-08-17 202736" src="https://github.com/user-attachments/assets/2ab28afe-cb53-4671-ada8-954117a49c04" />
 - Group size configured with Desired capacity: 2, Minimum capacity: 2, Maximum capacity: 4

<img width="1415" height="557" alt="Screenshot 2026-08-17 202829" src="https://github.com/user-attachments/assets/ff95d216-cab3-484c-86f1-ec7927729605" />
 - Target tracking scaling policy with Average CPU utilization and target value of 50%

I configured the Auto Scaling group with:
- **Name**: Web App Auto Scaling Group
- **VPC**: Lab VPC
- **Subnets**: Private Subnet 1 (10.0.2.0/24) and Private Subnet 2 (10.0.4.0/24)
- **Load balancer**: Attached to existing - webserver-app target group
- **Health checks**: Elastic Load Balancing enabled
- **Desired capacity**: 2
- **Minimum capacity**: 2
- **Maximum capacity**: 4
- **Scaling policy**: Target tracking with 50% average CPU utilization
- **Tags**: Name = WebApp

---

### Task 3: Verifying the Auto Scaling Configuration

I verified that two new EC2 instances (WebApp) were being created as part of the Auto Scaling group:

<img width="1587" height="140" alt="Screenshot 2026-08-17 203019" src="https://github.com/user-attachments/assets/dbe05e13-a37e-42ac-9da3-98cf305d29a7" />
 - Two WebApp instances initializing

I waited for the instances to show 2/2 checks passed and for the health status in the target group to change to "healthy".

---

### Task 4: Testing the Auto Scaling Configuration

I tested the auto scaling configuration by:
1. Opening a browser to the load balancer DNS name
2. Choosing "Start Stress" on the web page

This triggered the stress application, causing CPU utilization to spike to 100% on the instance that serviced the request.

After a few minutes, I observed that:
- The Auto Scaling group added new instances due to Amazon CloudWatch detecting that average CPU utilization exceeded 50%
- The scale-up policy was invoked in response to the high CPU load

---

## Conclusion

In this lab, I successfully:

- Created an EC2 instance by using AWS CLI commands, including configuring the instance with user data to install a web server
- Created a new AMI (WebServerAMI) from the running instance using the AWS CLI
- Configured an Application Load Balancer (WebServerELB) to distribute traffic across multiple Availability Zones
- Created an Amazon EC2 launch template (web-app-launch-template) with the custom AMI
- Configured an Auto Scaling group (Web App Auto Scaling Group) with target tracking scaling policies based on CPU utilization
- Verified that the Auto Scaling group scales out when CPU utilization exceeds 50% and scales in when load decreases

The lab demonstrated how to build a complete, scalable web application infrastructure using Amazon EC2 Auto Scaling, Elastic Load Balancing, and the AWS CLI.
