# Configuring a VPC - Lab Report

## Objectives
By the end of this lab, I was able to do the following:
- Create a VPC with a private and public subnet, an internet gateway, and a NAT gateway.
- Configure route tables associated with subnets to local and internet-bound traffic by using an internet gateway and a NAT gateway.
- Launch a bastion server in a public subnet.
- Use a bastion server to log in to an instance in a private subnet.

---

## Task 1: Creating a VPC

I began by navigating to the AWS Management Console and searching for **VPC**. I opened the VPC Management Console and selected **Your VPCs** from the left navigation pane.

I chose **Create VPC** and configured the following options:
- **Resources to create**: VPC only
- **Name tag**: Lab VPC
- **IPv4 CIDR block**: IPv4 CIDR manual input
- **IPv4 CIDR**: 10.0.0.0/16
- **IPv6 CIDR block**: No IPv6 CIDR block
- **Tenancy**: Default

After creating the VPC, I edited the VPC settings and enabled **DNS hostnames** so that EC2 instances launched into the VPC would automatically receive a public IPv4 DNS hostname.

<img width="1157" height="805" alt="Screenshot 2026-07-30 191841" src="https://github.com/user-attachments/assets/e20a26a9-49d1-4af1-8d86-2f731d08682f" />

<img width="1906" height="578" alt="Screenshot 2026-07-30 191910" src="https://github.com/user-attachments/assets/fee3e918-dd22-4254-849e-1bc4f39f9239" />

<img width="963" height="672" alt="Screenshot 2026-07-30 191934" src="https://github.com/user-attachments/assets/844a8631-f88c-4cbf-9012-4c49473286c1" />


---

## Task 2: Creating Subnets

### Task 2.1: Creating a Public Subnet

I navigated to **Subnets** in the left navigation pane and created a new subnet with the following configuration:
- **VPC ID**: Lab VPC
- **Subnet name**: Public Subnet
- **Availability Zone**: First Availability Zone in the list
- **IPv4 CIDR block**: 10.0.0.0/24

After creating the public subnet, I selected it, chose **Actions** → **Edit subnet settings**, and enabled **Enable auto-assign public IPv4 address**.

<img width="1281" height="755" alt="Screenshot 2026-07-30 192121" src="https://github.com/user-attachments/assets/8608f82e-5cf1-4ab2-935d-23df701f85f3" />
<img width="1181" height="706" alt="Screenshot 2026-07-30 192211" src="https://github.com/user-attachments/assets/b487acf3-d78e-461d-a65f-8753db1dd9a8" />


### Task 2.2: Creating a Private Subnet

I repeated the subnet creation process to create the private subnet with these settings:
- **VPC ID**: Lab VPC
- **Subnet name**: Private Subnet
- **Availability Zone**: First Availability Zone in the list
- **IPv4 CIDR block**: 10.0.2.0/23

The CIDR block of 10.0.2.0/23 includes all IP addresses starting with 10.0.2.x and 10.0.3.x, making it twice as large as the public subnet.

<img width="1362" height="761" alt="Screenshot 2026-07-30 192330" src="https://github.com/user-attachments/assets/34578ce2-ea1e-4aed-a845-7d936fd60a35" />


---

## Task 3: Creating an Internet Gateway

I navigated to **Internet gateways** in the left navigation pane and created an internet gateway with the name **Lab IGW**. After creation, I attached it to my VPC.

<img width="1701" height="471" alt="Screenshot 2026-07-30 192430" src="https://github.com/user-attachments/assets/6a8777f5-7286-4a34-bc95-63f2a5d8756e" />


---

## Task 4: Configuring Route Tables

### Private Route Table

I navigated to **Route tables** and identified the route table associated with the Lab VPC. I renamed it to **Private Route Table**. The route table initially had one route showing that all traffic destined for 10.0.0.0/16 would be routed locally.

### Public Route Table

I created a new route table with the following settings:
- **Name**: Public Route Table
- **VPC**: Lab VPC

I then edited the routes and added:
- **Destination**: 0.0.0.0/0
- **Target**: Internet Gateway → Lab IGW

Finally, I associated this route table with the **Public Subnet** by editing the subnet associations.

<img width="1915" height="650" alt="Screenshot 2026-07-30 192853" src="https://github.com/user-attachments/assets/03a7d098-c22e-476f-958a-9ac753518c00" />

<img width="1851" height="748" alt="Screenshot 2026-07-30 192915" src="https://github.com/user-attachments/assets/3c8a137c-59f4-473d-a74e-ee6748f30bee" />


---

## Task 5: Launching a Bastion Server in the Public Subnet

I navigated to the EC2 Management Console and launched an EC2 instance with the following configuration:
- **Name**: Bastion Server
- **AMI**: Amazon Linux 2023 AMI
- **Instance type**: t3.micro
- **Key pair**: Proceed without a key pair (using EC2 Instance Connect)
- **VPC**: Lab VPC
- **Subnet**: Public Subnet
- **Auto-assign public IP**: Enable
- **Security group**: New security group named **Bastion Security Group**
  - Inbound rule: SSH from anywhere (0.0.0.0/0)

<img width="1652" height="797" alt="Screenshot 2026-07-30 193039" src="https://github.com/user-attachments/assets/b55414f7-36e2-4104-bef6-2f4b9ef6d69b" />

<img width="1153" height="738" alt="Screenshot 2026-07-30 193142" src="https://github.com/user-attachments/assets/4f0ad757-1dd8-4c42-9659-14bb2c957ce6" />


---

## Task 6: Creating a NAT Gateway

I navigated to **NAT gateways** and created a NAT gateway with:
- **Name**: Lab NAT gateway
- **Subnet**: Public Subnet
- **Elastic IP**: Allocated a new Elastic IP

I then edited the **Private Route Table** and added a route:
- **Destination**: 0.0.0.0/0
- **Target**: NAT Gateway (Lab NAT gateway)

This configured the private subnet to send internet-bound traffic through the NAT gateway.

<img width="1446" height="677" alt="Screenshot 2026-07-30 193948" src="https://github.com/user-attachments/assets/4ac90e0b-6395-4ea7-940a-3978fa9cf29a" />

<img width="1875" height="482" alt="Screenshot 2026-07-30 194246" src="https://github.com/user-attachments/assets/26765a0f-e01c-4674-838c-b5c731872ed6" />


---

## Optional Challenge: Testing the Private Subnet

### Launching an Instance in the Private Subnet

I launched another EC2 instance in the private subnet:
- **Name**: Private Instance
- **AMI**: Amazon Linux 2023 AMI
- **Instance type**: t3.micro
- **Key pair**: Proceed without a key pair
- **VPC**: Lab VPC
- **Subnet**: Private Subnet
- **Auto-assign public IP**: Disable
- **Security group**: Private Instance SG
  - Inbound rule: SSH from 10.0.0.0/16 (the VPC CIDR)

I expanded the Advanced Details section and added the following User Data script:

```bash
#!/bin/bash
# Turn on password authentication for lab challenge
echo 'lab-password' | passwd ec2-user --stdin
sed -i 's/\[#]*PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
systemctl restart sshd.service
```

<img width="1576" height="755" alt="Screenshot 2026-07-30 193359" src="https://github.com/user-attachments/assets/0cf04753-8556-4c21-9231-6cff7f296498" />

<img width="1161" height="746" alt="Screenshot 2026-07-30 193444" src="https://github.com/user-attachments/assets/8ee2702c-f149-46e8-aa31-1d733cb1a4e0" />

<img width="882" height="497" alt="Screenshot 2026-07-30 193637" src="https://github.com/user-attachments/assets/d0354466-8900-436e-aa0f-b678c726b5cf" />

<img width="1211" height="772" alt="Screenshot 2026-07-30 193710" src="https://github.com/user-attachments/assets/13eb9047-962d-475c-a5d3-6f9128e24583" />



### Logging in to the Bastion Server

I selected the **Bastion Server** instance from the EC2 console and chose **Connect**. On the EC2 Instance Connect tab, I clicked **Connect** to open a terminal session.

### Logging in to the Private Instance

I copied the private IPv4 address of the **Private Instance** (10.0.3.206) from the EC2 console. From the bastion server terminal, I ran the following command:

```bash
ssh 10.0.3.206
```

When prompted, I entered `yes` to confirm the host key, and then entered the password `lab-password`.

<img width="966" height="691" alt="Screenshot 2026-07-30 194610" src="https://github.com/user-attachments/assets/c3708d65-c71e-4c4e-88f1-a7e2f465ed3e" />


### Testing the NAT Gateway

From the private instance, I tested internet connectivity:

```bash
ping -c 3 amazon.com
```

The ping failed with 100% packet loss, confirming that the private instance could not directly access the internet (which is expected without a NAT gateway configured). This confirms that the private subnet is properly isolated.

<img width="681" height="177" alt="Screenshot 2026-07-30 194655" src="https://github.com/user-attachments/assets/b63152e5-7987-427e-8066-0a8f5271cfa8" />


---

## Conclusion

In this lab, I successfully completed the following tasks:

1. **Created a VPC** with CIDR block 10.0.0.0/16
2. **Created subnets**:
   - Public Subnet (10.0.0.0/24)
   - Private Subnet (10.0.2.0/23)
3. **Created and attached an Internet Gateway** for external connectivity
4. **Configured route tables**:
   - Public Route Table with route to Internet Gateway (0.0.0.0/0 → IGW)
   - Private Route Table with route to NAT Gateway (0.0.0.0/0 → NAT)
5. **Launched a Bastion Server** in the public subnet for secure access
6. **Created a NAT Gateway** in the public subnet to allow private instances to initiate outbound internet traffic
7. **Successfully accessed a private instance** by connecting through the bastion server

This lab demonstrated the fundamental components of AWS VPC networking and provided hands-on experience with building a secure, isolated network environment with controlled internet access.
