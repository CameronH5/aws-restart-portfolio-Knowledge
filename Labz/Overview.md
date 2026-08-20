
# AWS Labz Portfolio: Cloud Infrastructure & Services

Welcome to my **AWS Labz** repository. This document serves as an overview of the hands-on labs I have completed to hone my skills in cloud computing. Utilizing the Amazon Web Services (AWS) platform, I have worked through practical scenarios covering core services, best practices, and architectural design.

The labs are organized into the following key domains based on the AWS Well-Architected Framework and common cloud engineering responsibilities:

---

## 1. ☁️ Compute
*Focus: Virtualization, Serverless, and Containerization.*

In this section, I explored the foundational building blocks of AWS cloud computing.

- **EC2 Instances:** Launched and configured Virtual Machines (Amazon Linux 2 and Windows) with specific instance types, user-data scripts, and security groups.
- **Elastic Load Balancing (ELB):** Implemented Application Load Balancers (ALB) to distribute traffic across multiple EC2 instances in different Availability Zones (AZs) for high availability.
- **AWS Lambda:** Developed serverless functions triggered by S3 events and API Gateway to handle event-driven processing.
- **Auto Scaling Groups:** Configured dynamic scaling policies to handle varying traffic loads automatically.

---

## 2. 🗄️ Database
*Focus: Relational, NoSQL, and In-Memory Data Storage.*

I practiced managing, securing, and optimizing data persistence layers.

- **Amazon RDS:** Deployed PostgreSQL and MySQL databases with Multi-AZ configurations for disaster recovery and automated backups.
- **Amazon DynamoDB:** Created NoSQL tables with global secondary indexes to handle high-throughput, low-latency workloads.
- **Amazon ElastiCache:** Set up Redis clusters to reduce database load and cache session state for web applications.
- **Database Migration:** Performed data migration from on-premises (simulated) to RDS using the AWS Database Migration Service (DMS).

---

## 3. 🐧 Linux
*Focus: OS Administration and Shell Scripting.*

These labs focused on interacting with the underlying operating system of EC2 instances.

- **Bash Scripting:** Wrote scripts to automate software installation (Apache/Nginx), system updates, and log rotation.
- **SSH Configuration:** Managed secure shell access, key-pair authentication, and bastion host configurations for private subnets.
- **Permissions:** Implemented file permissions, user management, and `sudo` access control on Amazon Linux 2.
- **Performance Monitoring:** Used Linux system commands (`top`, `htop`, `df`, `netstat`) to monitor resource utilization and troubleshoot application issues.

---

## 4. 🌐 Networking
*Focus: VPC, Subnets, Routing, and Connectivity.*

This section covers the network architecture that isolates and secures resources.

- **Amazon VPC:** Designed and deployed custom Virtual Private Clouds (VPCs) with CIDR blocks, public/private subnets, and Internet/ NAT Gateways.
- **Route 53:** Created DNS records (A, CNAME, Alias) to route domain traffic to specific endpoints.
- **Network ACLs & Security Groups:** Implemented stateful and stateless firewall rules to control traffic flow at the instance and subnet level.
- **VPC Peering:** Established connections between two VPCs to allow resource sharing across accounts or environments.

---

## 5. 📦 S3 (Simple Storage Service)
*Focus: Object Storage, Lifecycle Policies, and Hosting.*

I used S3 for durable storage and static website hosting.

- **Static Website Hosting:** Configured an S3 bucket to serve static HTML/CSS assets.
- **Lifecycle Policies:** Set up rules to automatically transition objects to Glacier for cost optimization and delete obsolete files.
- **Versioning:** Enabled versioning to retain, retrieve, and restore previous versions of objects.
- **Cross-Region Replication:** Configured replication rules to automatically copy objects from one region to another for compliance and latency reduction.

---

## 6. 📈 Scaling
*Focus: Elasticity and Performance Optimization.*

I focused on building systems that scale seamlessly.

- **Scaling Policies:** Implemented simple scaling, step scaling, and target tracking policies within Auto Scaling Groups.
- **RDS Scaling:** Upgraded instance types vertically (scale-up) and used Read Replicas to offload read traffic (scale-out).
- **AWS Global Accelerator:** Tested traffic acceleration through AWS's global network infrastructure.
- **CloudFront:** Set up a Content Delivery Network (CDN) to cache content at edge locations, reducing global latency.

---

## 7. 🔒 Security
*Focus: Identity Management, Access Control, and Best Practices.*

Security is paramount in the cloud. These labs covered the primary security services and controls.

- **IAM (Identity & Access Management):** Created users, groups, and roles following the principle of least privilege. Implemented *MFA* for root accounts.
- **IAM Policies:** Written custom JSON policies to grant specific permissions to services.
- **AWS Shield & WAF:** Configured Web Application Firewalls to protect against common web exploits (SQL Injection, XSS).
- **KMS (Key Management Service):** Encrypted S3 buckets, EBS volumes, and RDS snapshots using Customer-Managed Keys (CMK).
- **Secrets Manager:** Stored and rotated database credentials securely without hard-coding them into application code.

---

## 💡 Summary of Achievements

Throughout these labs, I have developed a robust understanding of the AWS ecosystem. The exercises were designed to simulate real-world engineering challenges—from deploying a secure, three-tier architecture to implementing cost-saving strategies.

**Key Skills Gained:**
- Proficiency in AWS Console, CLI, and SDKs.
- Automation using Infrastructure as Code (manual scripts and templates).
- Designing fault-tolerant and scalable architectures.
- Implementing enterprise-grade security policies.

---

## 🛠️ Tools & Technologies Used

- **OS:** Windows, Mac OS, Amazon Linux 2
- **CLI:** AWS CLI v2, Git Bash
- **Services:** EC2, S3, VPC, RDS, Lambda, IAM, CloudFront
- **Languages:** Bash, YAML (for future CloudFormation), Python (for Lambda)
