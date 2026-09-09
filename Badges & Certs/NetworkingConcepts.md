# AWS SimuLearn – Networking Concepts

## Overview

The **AWS SimuLearn: Networking Concepts** module provided an interactive, scenario-based simulation centered on designing and implementing a secure, custom Virtual Private Cloud (VPC) architecture for a banking client. The goal was to establish controlled network communication between internal resources, private databases, and the internet.

## Actions Completed

* **Gathered Security & Network Requirements:** Engaged in an AI-driven consultation with a simulated banking client to identify connectivity requirements, subnet isolation strategies, and traffic filtering criteria.

* **Designed Custom VPC Architecture:** Evaluated IP addressing and network topology, structuring a custom Amazon VPC utilizing IPv4 CIDR blocks, public subnets for web tier access, and private subnets for sensitive database workloads.

* **Configured Routing & Gateways:** Provisioned an Internet Gateway (IGW) for public subnet egress/ingress, set up custom Route Tables to direct network traffic appropriately, and established secure pathways.

* **Implemented Network Security Controls:** Configured stateless Network Access Control Lists (NACLs) at the subnet boundaries and stateful Security Groups at the instance level to enforce strict ingress/egress firewall rules.

* **Simulated & Validated Network Traffic:** Deployed EC2 instances within public and private subnets, verifying through live connectivity tests that internal database instances were shielded from direct internet access while remaining reachable via internal resources.

## Key Outcomes

* Transitioned raw business networking requirements into a isolated, defense-in-depth VPC environment.

* Demonstrated hands-on technical proficiency in CIDR block planning, VPC traffic routing, and network perimeter protection using AWS security primitives.

<img width="1172" height="822" alt="image" src="https://github.com/user-attachments/assets/a842136f-0124-4a81-9714-57aca6d5f0ef" />
