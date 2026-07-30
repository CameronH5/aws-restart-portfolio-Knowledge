# Lab Report: Troubleshooting a VPC

## Objective
The goal of this lab was to deploy network monitoring and troubleshooting tools for AWS VPCs. By the end of the lab, I successfully:
*   Created VPC Flow Logs.
*   Troubleshot VPC configuration issues impacting resource accessibility.
*   Analyzed VPC Flow Logs to identify network traffic patterns.

---

### Task 1 & 2: Initial Setup and Flow Logs Creation

**1. Configured AWS CLI**
I initiated the configuration for the AWS Command Line Interface on the CLI Host instance. I entered the provided **AWS Access Key ID**, **AWS Secret Access Key**, set the **Default region name** to `us-west-2`, and set the **Default output format** to `json`.
*Terminal Command Executed:*
```bash
aws configure
```
<img width="780" height="436" alt="Screenshot 2026-07-30 201047" src="https://github.com/user-attachments/assets/eba661b5-170c-4a92-9618-0085ae47aaec" />


**2. Created an S3 Bucket for Flow Logs**
I attempted to create an S3 bucket to serve as the destination for the VPC Flow Logs. Initially, I used `flowlog######` as the bucket name, which resulted in a parameter validation failure due to invalid bucket naming conventions.
*Initial Failed Command:*
```bash
aws s3api create-bucket --bucket flowlog###### --region 'us-west-2' --create-bucket-configuration LocationConstraint='us-west-2'
```
To resolve this, I replaced `######` with a specific set of random numbers, `12626`, which successfully created the bucket.
<img width="1671" height="102" alt="Screenshot 2026-07-30 201308" src="https://github.com/user-attachments/assets/dc490117-941f-4352-8b01-b84ab6f16a66" />

```bash
aws s3api create-bucket --bucket flowlog12626 --region 'us-west-2' --create-bucket-configuration LocationConstraint='us-west-2'
```

**3. Identified the VPC ID**
I filtered the VPCs in the `us-west-2` region to retrieve the VPC ID associated with the "VPC1" tag. The command returned the `VpcId` (`vpc-099069260cea47e72`).
<img width="1480" height="221" alt="Screenshot 2026-07-30 201357" src="https://github.com/user-attachments/assets/d72b4cd3-5d21-4698-8065-3613b6b0c5c0" />

```bash
aws ec2 describe-vpcs --query 'Vpcs[*].[VpcId,Tags[?Key==`Name`].Value,CidrBlock]' --filters "Name=tag:Name,Values='VPC1'"
```

**4. Created the VPC Flow Logs**
I enabled VPC Flow Logs for the identified VPC. I configured the logs to capture `ALL` traffic, utilizing the S3 bucket (`flowlog12626`) I just created as the log destination. The command successfully returned a `FlowLogId` (`fl-0c91c5ee5cb745a8a`).
```bash
aws ec2 create-flow-logs --resource-type VPC --resource-ids vpc-099069260cea47e72 --traffic-type ALL --log-destination-type s3 --log-destination arn:aws:s3:::flowlog12626
```
After creation, I verified the logs were set to an `ACTIVE` state using the `describe-flow-logs` command.
<img width="1897" height="203" alt="Screenshot 2026-07-30 201743" src="https://github.com/user-attachments/assets/68bb236a-6a66-4168-8606-4bce48557bb4" />
<img width="1872" height="395" alt="Screenshot 2026-07-30 201815" src="https://github.com/user-attachments/assets/070429e7-c8b5-4493-9f76-386dcf266c17" />

---

### Task 3: Troubleshooting VPC Configuration Issues

**1. Verified Network Access Failure**
I opened a new browser tab and attempted to access the public IP address `16.146.148.7` of the Café Web Server. The browser returned an error stating "This site can't be reached" (`ERR_TIMED_OUT`), confirming an underlying network configuration issue.


<img width="1190" height="737" alt="Screenshot 2026-07-30 202042" src="https://github.com/user-attachments/assets/fa447114-ea52-4fb9-89dc-3ea16e5db08f" />

**2. Gathered EC2 Instance Details**
To investigate, I used the `describe-instances` command to get detailed configuration information about the web server based on its public IP address. I retrieved the instance's state, private IP, instance ID (`i-0b17dea3442b592a0`), security groups (`sg-067c3f087309cdbd7`), subnet ID, and key name.
<img width="1028" height="552" alt="Screenshot 2026-07-30 202147" src="https://github.com/user-attachments/assets/6df1f67b-976c-4414-ac59-e6cb5db257af" />


```bash
aws ec2 describe-instances --filter "Name=ip-address,Values='16.146.148.7'" --query 'Reservations[*].Instances[*].[State,PrivateIpAddress,InstanceId,SecurityGroups,SubnetId,KeyName]'
```
<img width="1892" height="467" alt="Screenshot 2026-07-30 202241" src="https://github.com/user-attachments/assets/c9aa18e8-11c8-43bb-b950-02bcea059590" />

**3. Performed Nmap Port Scanning**
I used `nmap` to verify if any ports were open on the web server from the CLI host. First, I installed the utility, then I ran the scan on the public IP address. Nmap indicated the host seemed down, suggesting connectivity was being blocked at a lower network layer.

```bash
sudo yum install -y nmap
nmap 16.146.148.7
```

<img width="1897" height="652" alt="Screenshot 2026-07-30 202430" src="https://github.com/user-attachments/assets/a6e16b58-0961-42ab-a5cb-9d38cd4462d3" />
<img width="833" height="145" alt="Screenshot 2026-07-30 202504" src="https://github.com/user-attachments/assets/56fa7f23-e229-4591-90eb-9f16045e69c6" />



**4. Diagnosed and Fixed the Missing Route Table Entry**
I analyzed the subnet and route table configuration for the instance. I determined that while outbound security group rules allowed egress, the default route table lacked a route to an Internet Gateway. I created a route to direct traffic destined for `0.0.0.0/0` to the `igw-0c90f70517057b49f` (Internet Gateway).
<img width="1636" height="125" alt="Screenshot 2026-07-30 203824" src="https://github.com/user-attachments/assets/ceb5e10f-1b9d-4dab-ad02-1d614007ae5d" />

```bash
aws ec2 create-route --route-table-id 'rtb-089e2c228b80ca823' --gateway-id 'igw-0c90f70517057b49f' --destination-cidr-block '0.0.0.0/0'
```
After updating the route table, I refreshed the web browser and successfully resolved the first issue, reaching the web server's "Hello From Your Web Server" page.


---

### Task 4: Downloading and Analyzing Flow Logs

**1. Downloaded and Extracted Flow Logs**
I created a local directory named `flowlogs` on the CLI Host to store the logs and navigated into it. After confirming the S3 bucket name via `aws s3 ls`, I recursively copied all flow logs from S3 into the local directory.
<img width="415" height="111" alt="Screenshot 2026-07-30 205003" src="https://github.com/user-attachments/assets/76c055d2-51b0-4b48-b0ba-5e684b2872de" />

```bash
mkdir flowlogs
cd flowlogs
aws s3 ls
aws s3 cp s3://flowlog12626/ . --recursive
```
I then navigated into the deeply nested directory structure generated by VPC Flow Logs (`AWSLogs/505876179756/vpcflowlogs/us-west-2/2026/07/30/`). I listed the files to confirm the `log.gz` files were present.
<img width="1896" height="532" alt="Screenshot 2026-07-30 205040" src="https://github.com/user-attachments/assets/9612d63b-306a-48e7-a80a-114c455aa507" />
<img width="908" height="62" alt="Screenshot 2026-07-30 205404" src="https://github.com/user-attachments/assets/3117ae9c-2c5a-4031-8193-fc74ee1df38f" />

```bash
cd AWSLogs/505876179756/vpcflowlogs/us-west-2/2026/07/30/
ls
gunzip *.gz
```
After extraction, a second `ls` command confirmed the files were successfully converted into plain `.log` text files.
<img width="1776" height="177" alt="Screenshot 2026-07-30 205426" src="https://github.com/user-attachments/assets/fda3b095-4736-4d95-85ea-1d1718191aac" />
<img width="1730" height="192" alt="Screenshot 2026-07-30 205502" src="https://github.com/user-attachments/assets/fdb73e67-4e40-479d-986e-e97332a93a6d" />

**2. Analyzed VPC Flow Logs for REJECT Events**
I used the `grep` command to filter the logs for any network packets that were rejected. Initially, a broad scan found `675` rejected records.
<img width="570" height="80" alt="Screenshot 2026-07-30 205650" src="https://github.com/user-attachments/assets/29d87bcb-87d8-4cfa-9c15-b710aea9220c" />

```bash
grep -rn REJECT . | wc -l
```
To isolate the rejected SSH attempts I made during troubleshooting, I refined the search by looking for port 22 (`grep -rn 22`) alongside the `REJECT` keyword. I then further filtered this down to my specific public source IP address (which was identified via the AWS Management Console during the lab), `165.255.10.110`.
<img width="1892" height="95" alt="Screenshot 2026-07-30 205925" src="https://github.com/user-attachments/assets/bf1b32bc-d0fa-4a39-a9e2-982c02bdf8fa" />

```bash
grep -rn 22 . | grep REJECT | grep 165.255.10.110
```
The resulting log entry clearly showed a `REJECT OK` action for a packet originating from my IP (`165.255.10.110`), heading to the instance's IP (`10.0.1.206`) on destination port `22`.

**3. Validated the Elastic Network Interface ID**
Finally, I cross-referenced the Network Interface ID (`eni-06a26507bfd7fa83a`) listed in the flow log output with the actual resources attached to the public IP `16.146.148.7`. I ran the following command to confirm the interface belonged to the web server.
<img width="1897" height="198" alt="Screenshot 2026-07-30 210115" src="https://github.com/user-attachments/assets/faf639ed-e50b-4aa7-8409-7d4341adf5cd" />

```bash
aws ec2 describe-network-interfaces --filters "Name=association.public-ip,Values='16.146.148.7'" --query 'NetworkInterfaces[*].[NetworkInterfaceId,Association.PublicIp]'
```
The output successfully verified that the Network Interface ID in the log matched the ENI associated with the web server.

### Conclusion
Through this lab, I successfully set up comprehensive network monitoring with VPC Flow Logs and utilized those logs alongside AWS CLI diagnostic tools to identify and resolve real-world network misconfigurations involving route tables, security groups, and Network ACLs.
