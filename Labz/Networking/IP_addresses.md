
# Lab Report: Internet Protocols - Public and Private IP Addresses

## Objectives

In this lab, I:

- Summarized and investigated the customer scenario.
- Analyzed the difference between private and public IP addresses.
- Developed a solution to the customer's issue.
- Summarized and described my findings.

## Lab Report

### Task 1: Investigate the Customer's Environment

**1. Customer Scenario Summary**

As a cloud support engineer at AWS, I received a ticket from a customer, Jess. She reported a networking issue where two EC2 instances (Instance A and Instance B) in the same VPC (CIDR: `10.0.0.0/16`) and subnet had different internet connectivity. Instance B could reach the internet, but Instance A could not. Jess also asked for advice on using a public CIDR block (e.g., `12.0.0.0/16`) for a new VPC.

**2. Investigation of AWS Environment**

I began by accessing the AWS Management Console and navigating to the Amazon EC2 dashboard, as instructed. I then reviewed the instances to replicate the customer's issue.

**3. Analysis of Public and Private IP Addresses**

I selected **instance A** and noted its network details. I observed that the instance had only a **Private IPv4 address (`10.0.10.41`)**, and the **Public IPv4 address** field was empty.

<img width="1601" height="513" alt="Screenshot 2026-07-21 195128" src="https://github.com/user-attachments/assets/49c9f6bb-2dbc-401d-8662-859d05ac54b6" />


Next, I selected **instance B**. I observed that while it also had a **Private IPv4 address (`10.0.10.205`)**, it was also assigned a **Public IPv4 address (`34.222.123.94`)**.

<img width="1562" height="683" alt="Screenshot 2026-07-21 195046" src="https://github.com/user-attachments/assets/3166e895-e83e-4e45-a36f-f993e57a3a80" />


This confirmed the customer's suspicion. The key difference was that instance B had a public IP address assigned, while instance A only had a private IP. This explained why instance B could reach the internet and instance A could not, as a public IP is required for direct outbound internet access from a public subnet without a NAT gateway or similar service.

### Task 2: Analyze the Connectivity Difference

**1. Attempting SSH Connection**

To further test the connectivity difference, I used an SSH client from my local machine. I attempted to connect to both instances using their respective IPv4 addresses.

- **Connection to Instance A:** I attempted to connect to the private IP address `10.0.10.41`. The connection failed, timing out. This was expected, as private IP addresses are not routable over the public internet.

- **Connection to Instance B:** I attempted to connect to the public IP address `34.222.123.94`. The connection was successful, as shown in the terminal output below.

<img width="797" height="371" alt="Screenshot 2026-07-21 194304" src="https://github.com/user-attachments/assets/077a6d8a-bbb2-45d5-95cb-c1abc40a44e7" />


**2. Analysis and Conclusion**

The SSH test successfully demonstrated the fundamental difference between public and private IP addresses. The successful connection to instance B confirmed it had a public IP and was accessible from outside the VPC. The failed connection to instance A confirmed it only had a private IP, which is confined to the internal network of the VPC and cannot be reached directly from the internet.

### Task 3: Solution and Recommendations

**1. Solution to Customer's Issue**

Based on my investigation, I determined that the root cause of the issue was that instance A was not assigned a public IP address. The solution for the customer is to:

- Stop the instance.
- Attach or enable a public IP address (or assign an Elastic IP address) to instance A.
- Start the instance. It should then be able to reach the internet.

**2. Recommendation on Using Public CIDR for VPCs**

Regarding the customer's question about using a public range of IP addresses (like `12.0.0.0/16`) for a VPC, I summarized my findings and recommendations to her. I advised against this practice for the following reasons:

- **Routing Conflicts:** A public IP range is globally routable on the internet. Using it internally can cause significant routing conflicts and communication issues with the actual public resources.
- **Unintended Traffic:** Replies to traffic from such a VPC could be sent to the real public IP addresses on the internet rather than back to the VPC, causing application failures.
- **Best Practices:** Industry standards and AWS best practices recommend using the designated private IP address ranges (RFC 1918) for VPCs to ensure proper and predictable network behavior.

### Conclusion

In this lab, I successfully investigated a customer's networking issue, identified the cause as a missing public IP address on an EC2 instance, and provided a solution. I also analyzed the differences between public and private IP addresses and provided a well-reasoned recommendation against using public IP ranges for VPCs. This exercise highlighted the importance of understanding IP addressing in cloud environments for effective troubleshooting and architecture design.
