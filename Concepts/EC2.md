

### Part 1: Explain Like I'm 5

Imagine you want to play a video game or run a computer program, but you don't want to buy a physical computer to put under your desk.

* **Without AWS EC2:** You have to buy a computer from a store, wait for it to arrive, plug in all the cables, and pay for the electricity every single day—even when you turn it off and go to sleep.
* **With AWS EC2:** AWS has a giant room filled with thousands of super-fast computers. With EC2, you can rent a piece of one of those computers in **seconds**!
* You can pick how much memory or power it has.
* You can use it for 10 minutes to run a game, turn it off, and pay **only for those 10 minutes**.



It is a virtual computer in the cloud that you can start, stop, resize, or delete whenever you want!

---

### Part 2: Grown-Up / Exam-Level Explanation 👔

**Amazon EC2** provides scalable, virtual compute capacity in the AWS cloud. It eliminates the need to invest in hardware up front, allowing you to deploy applications rapidly.

An EC2 server is called an **Instance**. EC2 provides total administrative control (root/administrator access) over the underlying virtual machine (VM) operating system (Linux, Windows, macOS, etc.).

#### Why Is It Different? (Crucial Architectural Boundaries)

The exams heavily test **Compute Options selection** (EC2 vs. ECS/Fargate vs. Lambda):

* **EC2 vs. AWS Lambda (Serverless):**
* *EC2 (IaaS - Infrastructure as a Service):* You manage the OS, security patches, network routing, and software installed. Runs continuously until stopped.
* *Lambda (Serverless FaaS):* AWS manages all underlying servers. Code executes strictly on-demand (up to 15 minutes max execution time) and scales automatically.


* **EC2 vs. AWS Fargate (Containers):**
* *EC2:* You manage the underlying EC2 host instances that run your Docker containers.
* *Fargate:* Serverless compute engine for containers; you deploy containers without provisioning or managing EC2 instances.



---

### Core Technical Concepts & Terminology

#### 1. Instance Naming Convention

Exams often show instance names like **`t3.large`** or **`m5.2xlarge`**. Here is how to decode them:

* **`m`** = **Instance Family** (e.g., `M` for General Purpose, `C` for Compute, `R` for Memory).
* **`5`** = **Generation** (5th Generation; higher numbers mean newer/faster hardware).
* **`2xlarge`** = **Instance Size** (Determines vCPU count, RAM size, network bandwidth, and cost).

#### 2. Instance Families (Must Know for SAA-C03)

* **General Purpose (`A1`, `T3`, `T4g`, `M5`, `M6g`):** Balanced compute, memory, and networking. Ideal for web servers, small databases, and dev environments. *(Note: `t` instances use **CPU Burst Credits**)*.
* **Compute Optimized (`C5`, `C6g`, `C7g`):** High vCPU-to-memory ratio. Best for high-performance web servers, batch processing, media encoding, and machine learning inference.
* **Memory Optimized (`R5`, `X1`, `z1d`):** High RAM-to-vCPU ratio. Ideal for in-memory databases (Redis, Memcached), SAP HANA, and big data processing.
* **Storage Optimized (`I3`, `D2`):** High sequential read/write access to local storage. Best for data warehousing (Hadoop), NoSQL databases (Cassandra), and log analytics.
* **Accelerated Computing (`P4`, `G4`, `Inf1`):** Uses hardware GPUs or custom ASICs. Used for 3D graphics rendering, deep learning, and AI model training.

---

### EC2 Purchasing & Pricing Models (Top Exam Focus) 🎯

Choosing the right purchasing option is tested across all four domains of both exams:

```
+-----------------------------------------------------------------------------------+
|                              EC2 PURCHASING OPTIONS                               |
+-------------------+-------------------+--------------------+----------------------+
|     ON-DEMAND     |   SAVINGS PLANS   | RESERVED INSTANCE  |    SPOT INSTANCES    |
|                   |   / RESERVED      |    (CONVERTIBLE)   |                      |
| Highest flexibility| 1 or 3-year term  | Flexibility to     | Up to 90% discount   |
| Pay by the second | Up to 72% discount| change OS or family| Can be interrupted   |
| Short-term/Unpredictable| Steady-state workloads | Evolving workloads | Fault-tolerant batch |
+-------------------+-------------------+--------------------+----------------------+

```

1. **On-Demand:** Pay for compute capacity by the second/hour with no long-term commitment.
* *Best for:* Short-term, unpredictable workloads that cannot be interrupted.


2. **Savings Plans & Reserved Instances (RIs):** Commit to a specific amount of compute usage (measured in $/hr for Savings Plans) for a **1-year or 3-year term** in exchange for discounts up to 72%.
* *Standard RIs:* Highest discount, but cannot change instance family.
* *Convertible RIs:* Slightly lower discount, but allows changing instance families, OS, or tenancy.
* *Best for:* Steady-state, predictable, long-term workloads.


3. **Spot Instances:** Bid on unused EC2 capacity at discounts up to **90% off On-Demand rates**.
* *Catch:* AWS can reclaim the instance with a **2-minute notification warning** if capacity is needed elsewhere.
* *Best for:* Stateless, fault-tolerant, flexible batch processing, big data analysis, and containerized workloads. *(Never use for databases or critical stateful apps)*.


4. **Dedicated Hosts:** Physical EC2 servers fully dedicated to your use.
* *Best for:* Strict regulatory compliance or Bring-Your-Own-License (BYOL) software licenses bound to physical cores/sockets.


5. **Dedicated Instances:** Instances running on hardware dedicated to a single customer, but not tied to specific physical sockets/cores.

---

### Critical Supporting Features

#### 1. AMIs (Amazon Machine Images)

An AMI is a pre-configured template containing the Operating System, applications, and permissions required to launch an instance. You can use official AWS AMIs, buy from the AWS Marketplace, or create custom AMIs for fast, identical server rollouts.

#### 2. Storage Options: EBS vs. Instance Store (High-Frequency Distinction)

* **Amazon EBS (Elastic Block Store):** Network-attached virtual disks. Data **persists** even if the EC2 instance is stopped or terminated. Supports snapshots, encryption, and live resizing.
* **EC2 Instance Store (Ephemeral Storage):** Temporary storage physically attached to the host server hardware. Offers extremely high IOPS and low latency.
* *Critical Rule:* Data is **PERMANENTLY LOST** if the instance is **stopped or terminated** (data survives a simple OS reboot, but not a stop/start).



#### 3. EC2 Placement Groups

Controls how EC2 instances are physically placed across underlying hardware inside an Availability Zone (AZ):

* **Cluster:** Packs instances close together inside a single AZ. Achieves low-latency, high-throughput networking. *(Use case: High-Performance Computing - HPC)*.
* **Spread:** Places each instance on distinct physical hardware racks across AZs. Maximum fault isolation. *(Use case: Small numbers of critical instances like domain controllers)*.
* **Partition:** Divides the group into logical partitions across hardware racks. Instances in one partition do not share hardware with other partitions. *(Use case: Distributed workloads like Hadoop, HDFS, Cassandra)*.

#### 4. Security Groups & IAM Roles

* **Security Groups:** Act as virtual firewalls at the **instance level**. They are **stateful** (if inbound traffic is allowed, outbound response traffic is automatically allowed).
* **IAM Roles for EC2 (Instance Profiles):** Attach IAM roles directly to an EC2 instance to securely grant access to other AWS services (like S3 or DynamoDB) without hardcoding AWS access keys inside code.

---

### Complete EC2 Exam Matrix

| Feature / Concept | Primary Architectural Purpose | Exam Rule / Constraint |
| --- | --- | --- |
| **Spot Instances** | Maximum Cost Savings (up to 90%). | Workload **must** be fault-tolerant (2-min termination notice). |
| **Placement Group: Cluster** | High-performance interconnect. | Keep instances inside a **single AZ**. |
| **EBS Storage** | Persistent block storage. | Independent lifecycle from EC2; survives instance stops. |
| **Instance Store** | High-speed temporary storage. | Data lost on instance **STOP** or **TERMINATE**. |
| **IAM Instance Profile** | Secure authentication. | Never store access keys inside EC2 instances; use IAM roles. |

---

### Exam Day "Must-Know" Cheatsheet 🎯

1. **Spot vs. On-Demand vs. RI:**
* Short, unpredictable, non-interruptible $\rightarrow$ **On-Demand**.
* Long-term, steady state $\rightarrow$ **Reserved Instances / Savings Plans**.
* Batch processing, stateless, flexible $\rightarrow$ **Spot Instances**.


2. **Auto Scaling + SQS:** The standard AWS exam pattern for scaling worker EC2 instances horizontally based on queue depth (using SQS queue size metrics).
3. **Data Loss Avoidance:** Root EBS volumes can be set to "Delete on Termination" by default. Non-root EBS volumes persist by default when an instance is terminated.
