# Create Subnets and Allocate IP Addresses in an Amazon VPC – Lab Report

## Customer Scenario Summary

Paulo Santos, a startup owner and new AWS user, requested assistance in setting up a VPC. His requirements were:

- Use a private IPv4 CIDR block in the **192.x.x.x** range.
- Provide approximately **15,000 private IP addresses** within the VPC.
- Allocate a public subnet with **at least 50 IP addresses**.
- The VPC should include a public subnet, an internet gateway, and an EC2 instance as shown in the provided diagram.

I acted as the AWS Cloud Support Engineer and built a VPC that meets these requirements, documenting each step for the customer to follow.

---

## Task 1: Investigate the Customer's Needs

I first reviewed the customer's email and architecture diagram.

### Determining the Correct CIDR Blocks

To satisfy 15,000 IP addresses, I needed a VPC CIDR block with a netmask that provides at least 15,000 usable addresses. Using the private IP ranges defined in RFC 1918, I chose the **192.168.0.0/18** range, which offers **16,384 total IPs** (and 16,382 usable after reserving 5 addresses per AWS). This comfortably exceeds the required 15,000.

For the public subnet, I selected **192.168.1.0/26**, which provides **64 total IPs** (59 usable), meeting the “at least 50 IP addresses” requirement.

I referred to the public IP address calculators and RFC 1918 documentation as recommended.

---

## Building the VPC in the AWS Console

### Accessing the VPC Dashboard

I navigated to the AWS Management Console, searched for **VPC**, and selected the VPC service.

### Launching the VPC Wizard

I clicked **Create VPC** to begin the creation process.

### Selecting a VPC Configuration

I chose **VPC with a Single Public Subnet**, as this matched the customer’s architecture (one public subnet, no private subnets required).

### Configuring the VPC and Subnet CIDR Blocks

I entered the following settings:

| Field | Value |
|-------|-------|
| **IPv4 CIDR block** | `192.168.0.0/18` (16,384 IPs) |
| **IPv6 CIDR block** | No IPv6 CIDR block |
| **VPC name** | `First VPC` |
| **Public subnet's IPv4 CIDR** | `192.168.1.0/26` (64 IPs, 59 usable) |
| **Availability Zone** | No Preference |
| **Subnet name** | `Public subnet` |
| **Enable DNS hostnames** | Yes (default) |
| **Hardware tenancy** | Default |

I confirmed the subnet CIDR block fit within the VPC CIDR and was large enough for at least 50 IPs.

### Customizing Availability Zones and Subnet Count

I accepted the default of **1 public subnet** and **0 private subnets**, as the customer only needed a single public subnet.

<img width="577" height="466" alt="Screenshot 2026-07-22 185342" src="https://github.com/user-attachments/assets/4b298197-774e-4a6a-bf11-bb05ea95060f" />


The auto‑generated Name tag was set to `First VPC` to identify all associated resources.

<img width="1473" height="616" alt="Screenshot 2026-07-22 185359" src="https://github.com/user-attachments/assets/9ac28b4c-2361-41e7-a6d9-1e6f212083aa" />


### Creating the VPC

After reviewing the settings, I clicked **Create VPC**. A success message appeared, confirming that the VPC was provisioned.

### Verifying the VPC

I navigated to **Your VPCs** in the left navigation menu to confirm the new VPC was listed with the correct CIDR and state (`Available`).

<img width="1402" height="746" alt="Screenshot 2026-07-22 185457" src="https://github.com/user-attachments/assets/5cdf7fe5-d01b-4a1b-9169-24afcbc2bce1" />


The details showed:
- **VPC ID**: `vpc-0036ce2a4819dc8b3`
- **State**: `Available`
- **IPv4 CIDR**: `192.168.0.0/18`
- **DNS hostnames**: `Enabled`
- **Main route table** and **subnet** were automatically created.

The resource map confirmed the public subnet `First VPC-subnet-public1-us-west-2a` existed within the VPC.

---

## Solution Summary and Findings

### Requirements Met

| Customer Requirement | Solution Provided |
|----------------------|-------------------|
| VPC CIDR in 192.x.x.x range | `192.168.0.0/18` |
| ~15,000 private IP addresses | 16,384 total IPs (more than enough) |
| Public subnet with ≥50 IPs | `192.168.1.0/26` (64 IPs, 59 usable) |
| Single public subnet architecture | One public subnet created as requested |
| Internet gateway | Included automatically with the VPC wizard |
| EC2 instance can be launched in the subnet | Ready for future deployment |

### Key Learning Points

- **CIDR Calculation**: Understanding how to choose a VPC CIDR block that provides sufficient IP addresses while remaining within the private address space.
- **VPC Wizard**: The wizard simplifies creation of a VPC with a public subnet, an internet gateway, and a route table.
- **Subnet Sizing**: Subnet CIDR blocks must be a subset of the VPC CIDR; I ensured the public subnet’s /26 block was correctly nested within the VPC’s /18 block.
- **Private vs. Public Subnets**: Public subnets have routes to an internet gateway, allowing instances to be accessible from the internet. For this customer’s needs, only a public subnet was required.

### Walkthrough for the Customer

I prepared a step‑by‑step guide for the customer:

1. Log in to AWS Management Console.
2. Navigate to **VPC** → **Your VPCs** → **Create VPC**.
3. Choose **VPC and more** (or use the wizard).
4. Set **Name tag auto‑generate** to a meaningful name (e.g., `First VPC`).
5. For **IPv4 CIDR block**, enter `192.168.0.0/18`.
6. Disable IPv6 (or leave default).
7. Select **1 public subnet** and **0 private subnets**.
8. Set **Public subnet CIDR** to `192.168.1.0/26`.
9. Choose an Availability Zone (or leave **No Preference**).
10. Leave other settings as default and click **Create VPC**.
11. Verify the VPC appears in the list with **State: Available**.

---
