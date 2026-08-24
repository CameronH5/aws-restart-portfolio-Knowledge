# AWS SimuLearn: Connecting VPCs

I completed the **AWS SimuLearn: Connecting VPCs** learning assignment, which combined interactive problem‑solving with hands‑on practice to design and implement network connectivity between isolated cloud environments.

## What I Did

### Reviewed a Business Scenario
I analyzed a use case where an organisation needed to establish secure and reliable network connectivity between multiple Virtual Private Clouds (VPCs) to enable communication between distributed applications and services.

### Designed a VPC Connectivity Solution
I designed a networking solution using appropriate AWS services to connect VPCs. I learned how to:
- Evaluate different connectivity options including VPC Peering and AWS Transit Gateway
- Configure route tables to direct traffic between VPCs
- Understand the differences between intra-region and inter-region VPC peering
- Apply security controls using security groups to govern cross-VPC traffic

### Built and Validated the Solution in a Live AWS Console
I built the proposed solution in a guided lab within a live AWS Console environment. I worked hands‑on with **Amazon VPC, VPC Peering connections, route tables, and EC2 instances**, and performed the following tasks:
- There were already created multiple VPCs with properly configured CIDR blocks (ensuring non-overlapping IP ranges)
- Established VPC peering connections between the VPCs
- Updated route tables in each VPC to enable traffic routing through the peering connections
- There were already launched EC2 instances in different VPCs to test and validate connectivity
- Verified that instances could communicate across VPCs while maintaining security boundaries

### Completed the Assignment and Validated Outcomes
After implementing the connectivity solution, I tested the network paths to confirm that traffic flowed correctly between VPCs and that the security rules permitted only intended communication.

## Key Takeaways

- Designed and implemented network connectivity between multiple VPCs
- Created VPC peering connections and configured route tables for cross-VPC routing
- Applied security controls using security groups for inter-VPC traffic
- Gained practical hands‑on experience with VPC networking in a real AWS Console environment
- Validated network connectivity by testing communication between resources across VPCs

  <img width="1052" height="807" alt="Screenshot 2026-08-24 194600" src="https://github.com/user-attachments/assets/478495a6-393d-4258-94be-7539a84bfcb8" />
