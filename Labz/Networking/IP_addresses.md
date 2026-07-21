# Lab Report: Internet Protocols - Public and Private IP Addresses

---

## Objectives

In this lab, I:

- Summarized and investigated the customer scenario
- Analyzed the difference between a private and public IP address
- Developed a solution to the customer's issue in this lab
- Summarized and described my findings

---

## Lab Report

### Task 1: Investigate the Customer's Environment

#### Customer Scenario Summary

As a cloud support engineer at Amazon Web Services (AWS), I received a support ticket from a customer named Jess, who is a Cloud Administrator at a Fortune 500 company. She reported a networking issue within their AWS infrastructure. The customer had one Virtual Private Cloud (VPC) with a CIDR range of `10.0.0.0/16`. Within this VPC, they had two EC2 instances—Instance A and Instance B—both located in the same public subnet with identical AWS resource configurations. However, Instance A could not reach the internet while Instance B could. Jess suspected the issue was related to the EC2 instances themselves. Additionally, she asked for advice on using a public IP range (e.g., `12.0.0.0/16`) for a new VPC she planned to launch.

#### Accessing the AWS Management Console

I began by launching the lab environment. I clicked the **Start Lab** button at the top of the instructions and waited until the status displayed **Lab status: ready**. I then closed the Start Lab panel and clicked the **AWS** link to open the AWS Management Console in a new browser tab. I arranged the tabs side by side for easier navigation.

#### Navigating to EC2 Dashboard

Once logged into the AWS Management Console, I typed **EC2** into the search bar located at the top-left corner and selected the EC2 service from the dropdown list.

In the left navigation menu, I selected **Instances** to view the current EC2 instances in my environment. I confirmed that there were two instances: **instance A** and **instance B**.

#### Investigating Instance IP Addresses

**Instance A Investigation:**

I selected the checkbox next to **instance A** and navigated to the **Networking** tab at the bottom of the page. I noted the following IP addresses:

- **Private IPv4 address:** `10.0.10.81`
- **Public IPv4 address:** *None assigned (field was empty)*

<img width="1587" height="587" alt="Screenshot 2026-07-21 200456" src="https://github.com/user-attachments/assets/838999f9-7a3a-4df6-a376-ed0dc11044f0" />


**Instance B Investigation:**

I deselected instance A and then selected the checkbox next to **instance B**. I navigated to the **Networking** tab and noted the following IP addresses:

- **Private IPv4 address:** `10.0.10.205`
- **Public IPv4 address:** `34.222.123.94`

<img width="1562" height="683" alt="Screenshot 2026-07-21 195046" src="https://github.com/user-attachments/assets/3623f2e4-8529-44e5-895d-d949a4939195" />


**Key Observation:**

I noticed a critical difference between the two instances. Instance A had only a **Private IPv4 address** assigned, while Instance B had both a **Private IPv4 address** and a **Public IPv4 address**. This explained why Instance B could reach the internet and Instance A could not—public IP addresses are required for direct internet access from instances in a public subnet.

---

### Task 2: Analyze the Difference Between Private and Public IP Addresses

#### SSH Connectivity Test

To further validate my findings, I tested connectivity to both instances using SSH from my local machine.

**Attempt to Connect to Instance A:**

I attempted to SSH into Instance A using its private IP address:

```bash
ssh ec2-user@10.0.10.81
```

The connection **failed** and timed out. This confirmed that private IP addresses are not routable over the public internet and cannot be accessed from outside the VPC.

**Attempt to Connect to Instance B:**

I attempted to SSH into Instance B using its public IP address:

```bash
ssh ec2-user@34.222.123.94
```

The connection was **successful**. I was able to authenticate and access the instance's terminal.

<img width="797" height="371" alt="Screenshot 2026-07-21 194304" src="https://github.com/user-attachments/assets/4a8ec385-6fa0-41bd-8707-28aa08ef9916" />


#### Analysis and Conclusion

The SSH test clearly demonstrated the fundamental difference between public and private IP addresses:

- **Public IP addresses** (like `34.222.123.94`) are globally routable on the internet, allowing external access to resources.
- **Private IP addresses** (like `10.0.10.81`) are confined to the internal VPC network and cannot be accessed from the public internet without additional services (e.g., NAT Gateway, Bastion Host).

---

### Task 3: Develop a Solution to the Customer's Issue

#### Solution: Assigning a Public IP to Instance A

Based on my investigation, I determined that the root cause of the customer's issue was that **Instance A lacked a public IP address**. To resolve this, I proceeded to allocate and associate an Elastic IP address to Instance A.

**Step 1: Allocate an Elastic IP Address**

I navigated to the **Elastic IP addresses** section under the EC2 dashboard in the left navigation menu. I clicked the **Allocate Elastic IP address** button and confirmed the allocation.

<img width="1641" height="213" alt="Screenshot 2026-07-21 200247" src="https://github.com/user-attachments/assets/ad1638a1-3ff9-425e-a906-ea0d64b0fb61" />


**Step 2: Associate the Elastic IP to Instance A**

After the Elastic IP was allocated (`52.38.114.32`), I selected it and chose **Actions → Associate Elastic IP address**. I selected **Instance** as the resource type and entered the instance ID for Instance A (`i-06da8621a91852211`). I confirmed the private IP address (`10.0.10.81`) and clicked **Associate**.

<img width="1630" height="567" alt="Screenshot 2026-07-21 200441" src="https://github.com/user-attachments/assets/45ae5eb0-f2d8-4cd6-99eb-f821cec2b63e" />


**Step 3: Verify the Update**

I returned to the Instances dashboard and checked the networking details for Instance A. I confirmed that the instance now had a **Public IPv4 address** of `52.38.114.32`.

<img width="1595" height="650" alt="Screenshot 2026-07-21 200521" src="https://github.com/user-attachments/assets/a20a27df-3464-488f-a641-34ba3bd58a9f" />


**Step 4: Test Connectivity to Instance A**

I attempted to SSH into Instance A using its newly assigned public IP address:

```bash
ssh ec2-user@52.38.114.32
```

The connection was **successful**. I was able to authenticate and access the instance's terminal.

<img width="797" height="400" alt="Screenshot 2026-07-21 200628" src="https://github.com/user-attachments/assets/4ce35e64-89ed-4e4f-9677-18e190b09d75" />


**Solution Summary:**

The issue was resolved by assigning a public Elastic IP address to Instance A. This allowed the instance to communicate with the internet and be accessed externally via SSH.

---

### Task 4: Summarize and Describe Findings

#### Recommendation Regarding Public CIDR for VPCs

Jess also asked about using a public IP range (e.g., `12.0.0.0/16`) for a new VPC. I advised against this approach for the following reasons:

1. **Routing Conflicts:** Public IP ranges are globally routable on the internet. Using them internally can cause significant routing conflicts and communication issues with actual public resources.

2. **Unintended Traffic:** Replies to traffic from such a VPC could be sent to the real public IP addresses on the internet rather than back to the VPC, causing application failures.

3. **Best Practices:** Industry standards and AWS best practices recommend using the designated private IP address ranges (RFC 1918) for VPCs to ensure proper and predictable network behavior. The recommended private ranges are:
   - `10.0.0.0/8`
   - `172.16.0.0/12`
   - `192.168.0.0/16`

#### Final Recommendations to the Customer

1. **For the Current Issue:** Assign a public IP address (preferably an Elastic IP) to Instance A to enable internet connectivity.

2. **For Future VPCs:** Always use private IP ranges for VPCs to avoid routing conflicts, ensure proper network behavior, and maintain security best practices.

---

## Group Activity Summary

As part of the group activity, I presented my findings to my partner, who acted as Jess (the customer). I explained:

- The difference between public and private IP addresses
- Why Instance A could not reach the internet (missing public IP)
- How I resolved the issue by assigning an Elastic IP
- Why using public CIDR ranges for VPCs is not recommended

We discussed the importance of proper IP address planning in AWS architectures and how this knowledge can help avoid similar issues in the future.

---

## Recap

In this lab, I investigated the customer's environment and applied troubleshooting techniques that allowed me to resolve the customer's issue. Within the scenario, I discovered that the customer's EC2 instance (Instance A) needed a public IP address to connect to the internet. This was tested by using an SSH utility to connect to the instance. Private IP addresses are used within the VPC and cannot establish a connection to the internet. As the lab recap noted, using a public range of IP addresses for a VPC can result in complications from having replies back from other unrelated resources.

---

## References

- AWS Documentation: EC2 Instance IP Addressing
- RFC 1918: Address Allocation for Private Internets
- Lab Instructions: Internet Protocols - Public and Private IP Addresses
