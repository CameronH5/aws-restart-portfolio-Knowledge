# Internet Protocols - Static and Dynamic Addresses - Lab Report

## Customer Scenario Summary

Bob, a Cloud Admin at a Fortune 500 company, contacted AWS Cloud Support regarding a networking issue with their EC2 instance called "Public Instance." The instance's IP address changes every time it is stopped and started, causing dependencies to break because a static IP address is required. Bob cannot keep the instance running continuously due to cost constraints.

## Task 1: Investigate the Customer's Environment

### Launching a Test EC2 Instance

I began by replicating the customer's issue in the AWS lab environment. I navigated to the EC2 dashboard and launched a new instance with the following configuration:

- **AMI**: Amazon Linux 2 AMI (HVM)
- **Instance Type**: t3.micro
- **Network**: Lab VPC
- **Subnet**: Public Subnet 1
- **Auto-assign Public IP**: Enabled
- **Security Group**: Linux Instance SG (existing)
- **Key Pair**: vockey (existing)

<img width="1228" height="760" alt="Screenshot 2026-07-22 181551" src="https://github.com/user-attachments/assets/410f5b6c-072a-41e2-aa93-b8ab7a60ed19" />

<img width="1075" height="152" alt="Screenshot 2026-07-22 181609" src="https://github.com/user-attachments/assets/747dbe58-7f84-4a5f-a12f-b5b0201fe557" />

<img width="1283" height="545" alt="Screenshot 2026-07-22 181659" src="https://github.com/user-attachments/assets/67cdabc7-c0fb-4d7b-a03e-a66a69120314" />

<img width="861" height="227" alt="Screenshot 2026-07-22 181733" src="https://github.com/user-attachments/assets/c8affa1a-f4f1-423e-9f62-0646786f10fa" />

After launching, I named the instance "test instance" and waited for it to reach the `2/2 checks passed` status.

<img width="1581" height="762" alt="Screenshot 2026-07-22 181950" src="https://github.com/user-attachments/assets/ac9d5fbc-f1d2-442b-aefe-534747a37c2b" />


### Testing Dynamic IP Behavior

With the instance running, I noted the networking details:

**Initial State (Running):**
- **Public IPv4**: 35.91.177.185
- **Private IPv4**: 10.0.10.217

I then stopped the instance using the **Instance state → Stop instance** action.

<img width="1417" height="237" alt="Screenshot 2026-07-22 182008" src="https://github.com/user-attachments/assets/63b31680-a10d-479b-9922-3cfc0ae016c4" />


After the instance entered the `Stopped` state, I observed that the Public IPv4 address was no longer displayed, while the Private IPv4 address remained at 10.0.10.217.

I then started the instance again using **Instance state → Start instance**.

<img width="1588" height="592" alt="Screenshot 2026-07-22 182202" src="https://github.com/user-attachments/assets/e116f7e9-d734-4c26-bf3e-347064453723" />


**After Restart:**
- **Public IPv4**: 44.243.111.234 (different from the previous address!)
- **Private IPv4**: 10.0.10.217 (unchanged)

### Key Observations

| IP Type | Behavior | Classification |
|---------|----------|----------------|
| Public IPv4 | Changed after stop/start | Dynamic |
| Private IPv4 | Remained consistent | Static |

**Conclusion:** I successfully replicated the customer's issue. The EC2 instance receives a dynamic public IP address by default, which changes whenever the instance is stopped and restarted. The private IP remains static within the VPC.

## Assigning an Elastic IP (Static Public IP)

To solve Bob's issue, I allocated an Elastic IP address (EIP) from the EC2 dashboard:

**Navigation:** EC2 Dashboard → Network & Security → Elastic IPs

<img width="1641" height="213" alt="Screenshot 2026-07-22 182303" src="https://github.com/user-attachments/assets/d82fae36-26b1-439a-8bd6-9dffb9fef810" />


I clicked **Allocate Elastic IP address** and kept all default settings.

**Allocated EIP:** 52.24.128.119

### Associating the Elastic IP

I selected the newly created EIP and chose **Actions → Associate Elastic IP address**.

<img width="1877" height="645" alt="Screenshot 2026-07-22 182432" src="https://github.com/user-attachments/assets/7f99a827-54eb-4423-a93a-83eddef1ec65" />


**Association Details:**
- **Resource Type**: Instance
- **Instance**: test instance (i-06fc0ccacbf2863a4)
- **Private IP Address**: Auto-selected (10.0.10.217)

After clicking **Associate**, I verified the changes on the Instances dashboard.

<img width="1497" height="595" alt="Screenshot 2026-07-22 182600" src="https://github.com/user-attachments/assets/51606037-ec97-4214-a726-6b026f311feb" />


**After EIP Association:**
- **Public IPv4**: 52.24.128.119 (the Elastic IP!)
- **Private IPv4**: 10.0.10.217

### Testing the Solution

I performed another stop/start cycle on the instance:

| Action | Public IPv4 | Private IPv4 |
|--------|-------------|--------------|
| Running (Initial) | 52.24.128.119 | 10.0.10.217 |
| Stopped | (no public IP shown) | 10.0.10.217 |
| Started | 52.24.128.119 | 10.0.10.217 |

**Observation:** The Elastic IP address remained persistent across the stop/start cycle. The public IP address no longer changes when the instance is stopped and started.

## Final Findings and Solution Summary

### Problem Identification
The customer's EC2 instance was using a **dynamically assigned public IP address** (default behavior). When an EC2 instance is stopped and started:
- The public IP is released back to the AWS pool
- A new public IP is assigned upon restart
- The private IP remains static

### Solution Implemented
I assigned an **Elastic IP (EIP)** to the EC2 instance, which provides a persistent, static public IP address that remains associated with the instance even across stop/start cycles.

### Benefits of the Solution
1. **Persistent IP**: The public IP address remains constant
2. **Cost-Effective**: Bob can stop the instance when not in use
3. **Reliability**: Dependencies and resources that rely on this IP address will continue to function correctly
4. **AWS Best Practice**: Elastic IPs are the recommended solution for requiring a static public IP address

### Recommendation to Customer
I would advise Bob to:
1. Allocate an Elastic IP address for the "Public Instance"
2. Associate the EIP with their instance
3. Update any external DNS records or application configurations to use the EIP
4. Keep the Elastic IP allocated even when the instance is stopped (minor cost, but worth it for stability)

## Task 2: Customer Communication Summary

As the Cloud Support Engineer, I communicated the following to Bob (the customer):

1. **Issue Identified**: The instance was using a dynamically assigned public IP (default EC2 behavior)
2. **Root Cause**: Public IPs are released when instances are stopped and new ones are assigned when started
3. **Solution Applied**: Allocated and associated an Elastic IP (52.24.128.119) to the instance
4. **Verification**: Tested the solution by stopping and starting the instance—the IP remained persistent
5. **Next Steps**: Bob can now stop the instance when not in use without breaking dependent resources

---

**Lab Completed**: Successfully analyzed, replicated, and resolved the customer's static IP issue using Elastic IP addresses in AWS.
