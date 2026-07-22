# Creating Networking Resources in an Amazon Virtual Private Cloud (VPC) – Lab Report

## Customer Scenario Summary

Brock, the customer, requested assistance in creating a VPC with network connectivity to the internet. The requirements were:

- VPC CIDR block: `192.168.0.0/18`
- Public subnet CIDR block: `192.168.1.0/26` (later changed to `/28` in the lab instructions, as shown in the screenshots)
- The VPC must allow an EC2 instance to reach the internet (i.e., be routable to the internet).

I acted as the AWS Cloud Support Engineer and built the VPC, along with all required networking components, and verified internet connectivity by successfully pinging an external host (google.com) from the EC2 instance.

---

## Task 1: Investigate the Customer’s Needs and Build the VPC

### Step 1: Create the VPC

I navigated to the VPC dashboard and chose **Create VPC** with the following settings:

- **Resources to create**: VPC and more  
- **Name tag**: `Test VPC`  
- **IPv4 CIDR block**: `192.168.0.0/18`  
- **IPv6**: No IPv6 CIDR block  
- **Tenancy**: Default  

I left the other options as default.

<img width="1217" height="797" alt="Screenshot 2026-07-22 193532" src="https://github.com/user-attachments/assets/93f93bd8-3b63-4dd9-962e-925c361da441" />


After creation, I verified the VPC details:

- **VPC ID**: `vpc-0776f69c123912047`
- **State**: `Available`
- **IPv4 CIDR**: `192.168.0.0/18`

<img width="1697" height="515" alt="Screenshot 2026-07-22 193553" src="https://github.com/user-attachments/assets/77a81271-e20b-420e-bb1d-564d8d86c771" />


---

### Step 2: Create a Public Subnet

I created a new subnet within the VPC:

- **VPC**: `Test VPC` (vpc-0776f69c123912047)
- **Subnet name**: `Public subnet`
- **Availability Zone**: No Preference (AWS chose us-west-2d)
- **IPv4 CIDR block**: `192.168.1.0/28` (this provides 16 IPs, which met the lab’s requirements for this exercise)

<img width="1147" height="390" alt="Screenshot 2026-07-22 193621" src="https://github.com/user-attachments/assets/1c3de2ba-ee6d-4270-99e6-a51c62c2beb4" />


<img width="1462" height="702" alt="Screenshot 2026-07-22 193653" src="https://github.com/user-attachments/assets/b48ab016-e020-4df6-9f25-e6ad3a739342" />


After creation, I confirmed the subnet appeared with status `Available`.

<img width="1468" height="352" alt="Screenshot 2026-07-22 193711" src="https://github.com/user-attachments/assets/fcc311c3-b12e-4459-a3e4-6c03cc6c2527" />


---

### Step 3: Create an Internet Gateway and Attach It to the VPC

To enable internet access, I created an internet gateway:

- **Name tag**: `IGW test VPC`

<img width="1631" height="581" alt="Screenshot 2026-07-22 193910" src="https://github.com/user-attachments/assets/eef2843b-d908-423c-98b2-cb0ccb25a9fe" />


I then attached it to the VPC:

- **VPC**: `vpc-0776f69c123912047 (Test VPC)`

<img width="1187" height="326" alt="Screenshot 2026-07-22 193945" src="https://github.com/user-attachments/assets/f60d6851-81fb-444e-a622-b5c59f9f9ec3" />


---

### Step 4: Create a Route Table and Add a Route to the Internet Gateway

I created a custom route table:

- **Name**: `Public route table`
- **VPC**: `Test VPC`
<img width="1425" height="523" alt="Screenshot 2026-07-22 193812" src="https://github.com/user-attachments/assets/680a367a-8b17-42f2-8250-932274ec26a4" />


Next, I edited the routes to add a default route to the internet gateway:

- **Destination**: `0.0.0.0/0`
- **Target**: `igw-01bab6031ef65f2fc` (the internet gateway I just created)

<img width="1866" height="456" alt="Screenshot 2026-07-22 194051" src="https://github.com/user-attachments/assets/70b4db10-a361-4d19-ae03-ad7503a4d591" />


I saved the changes. The route table now had a route to the internet.

---

### Step 5: Associate the Subnet with the Route Table

I associated the public subnet with the custom route table so that instances in that subnet can use the internet gateway.

- Under **Subnet associations**, I added `subnet-005937efb7ea53a41` (Public subnet) to the route table.

<img width="1902" height="475" alt="Screenshot 2026-07-22 194134" src="https://github.com/user-attachments/assets/54af9aef-3f26-44ac-95e0-26a4de0d0a92" />


---

### Step 6: Create a Network ACL (NACL) for the Public Subnet

I created a network ACL to control inbound and outbound traffic at the subnet level:

- **Name**: `Public Subnet NACL`
- **VPC**: `Test VPC`

<img width="1572" height="583" alt="Screenshot 2026-07-22 194233" src="https://github.com/user-attachments/assets/a0881bd3-9e67-46da-9a8e-7a87da028067" />


#### Inbound Rules

I edited the inbound rules to allow all traffic (this is a lab environment, for demonstration):

- **Rule number**: 100
- **Type**: All traffic
- **Source**: `0.0.0.0/0`
- **Allow/Deny**: Allow

<img width="1892" height="367" alt="Screenshot 2026-07-22 194342" src="https://github.com/user-attachments/assets/0f7ab5dd-bcd5-4187-a818-f8a587a3bc49" />


#### Outbound Rules

I also edited the outbound rules to allow all outgoing traffic:

- **Rule number**: 100
- **Type**: All traffic
- **Destination**: `0.0.0.0/0`
- **Allow/Deny**: Allow

<img width="1877" height="371" alt="Screenshot 2026-07-22 194412" src="https://github.com/user-attachments/assets/67301e93-ad9f-48ff-84e3-5f84f0fb82d0" />




---

### Step 7: Create a Security Group for the EC2 Instance

Security groups act as a stateful firewall at the instance level. I created a new security group:

- **Name**: `public security group`
- **Description**: `allow public access`
- **VPC**: `Test VPC`

<img width="1196" height="428" alt="Screenshot 2026-07-22 194621" src="https://github.com/user-attachments/assets/fe086d42-d7b9-4fa9-96f9-f8fd7b5e0ddf" />


#### Inbound Rules

I added rules to allow SSH (port 22), HTTP (port 80), and HTTPS (port 443) from anywhere (`0.0.0.0/0`). This allows me to connect via SSH and permits web traffic.

<img width="1813" height="675" alt="Screenshot 2026-07-22 194722" src="https://github.com/user-attachments/assets/5371e501-2e4c-4e08-b9e4-db453cb89fb1" />


#### Outbound Rules

I left the default outbound rule allowing all traffic to any destination.

---

### Step 8: Launch an EC2 Instance in the Public Subnet

I launched a new EC2 instance using the **Launch Instance** wizard:

- **Name**: (not specified, but I set a name later, or left default)
- **AMI**: Amazon Linux 2023 (or Amazon Linux 2)
- **Instance type**: t3.micro (or t2.micro)
- **Key pair**: `vockey` (provided by the lab)
- **Network**: `vpc-0776f69c123912047 (Test VPC)`
- **Subnet**: `subnet-005937efb7ea53a41 (Public subnet)`
- **Auto-assign public IP**: Enable
- **Security group**: `public security group` (existing)

<img width="1091" height="771" alt="Screenshot 2026-07-22 194904" src="https://github.com/user-attachments/assets/c7d3abb8-9000-4d4a-96c4-5d545ac9c1cf" />

<img width="1182" height="747" alt="Screenshot 2026-07-22 195014" src="https://github.com/user-attachments/assets/8706690f-49e8-44f4-bffd-ee57778e06c2" />


After the instance was running, I noted its public IP address (or used the public DNS). The instance received a public IP because the subnet has auto-assign public IP enabled.

---

### Step 9: Verify Internet Connectivity (Ping Google)

I connected to the instance via SSH using the key pair. From the instance terminal, I executed the `ping` command to test internet connectivity:

```bash
ping google.com
```

The output showed successful replies, confirming that the VPC network was properly configured and the instance could reach the internet.

<img width="941" height="263" alt="Screenshot 2026-07-22 195617" src="https://github.com/user-attachments/assets/2b1c62d1-a381-4f4e-893b-570cc8fd9943" />


The ping statistics confirmed **0% packet loss**, indicating full connectivity.

---

## Summary of Findings

- **VPC Created**: `Test VPC` with CIDR `192.168.0.0/18`
- **Public Subnet**: `Public subnet` with CIDR `192.168.1.0/28`
- **Internet Gateway**: Created and attached to the VPC
- **Route Table**: Custom route table with a default route to the internet gateway, associated with the public subnet
- **Network ACL**: Public subnet NACL with inbound/outbound rules allowing all traffic
- **Security Group**: `public security group` allowing SSH, HTTP, HTTPS from anywhere
- **EC2 Instance**: Launched in the public subnet with an auto-assigned public IP and the correct security group
- **Connectivity**: Successful `ping` to google.com, confirming the VPC is routable to the internet

All customer requirements were met. The VPC now has network connectivity to the internet, allowing instances in the public subnet to communicate externally.

---
