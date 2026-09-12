# AWS RDS MySQL Database & EC2 Integration Lab

This repository documents the step-by-step setup, configuration, and execution of relational database tasks performed using **Amazon RDS (MySQL)** and an **Amazon EC2 Linux Server**.

---

## 📋 Overview

In this lab challenge, an Amazon RDS MySQL instance was provisioned within a specified Virtual Private Cloud (VPC). An EC2 Linux client instance was configured to securely connect to the database, where SQL operations—including table creation, data insertion, data retrieval, and table joins—were executed.

---

## 🛠️ Step-by-Step Execution

### Step 1: Provisioning the Amazon RDS DB Instance
- An **Amazon RDS MySQL** database instance was launched using the **Dev/Test** template.
- The instance was configured within the assigned `Lab VPC`.
- A **Burstable class (`db.t3.micro`)** with 100 GB General Purpose SSD (gp2) storage was allocated.
- Enhanced Monitoring was explicitly disabled as per lab requirements.
- Security group rules were configured to allow inbound traffic on **port 3306** from the `LinuxServer` instance.

---

### Step 2: Connecting to the Linux Server (EC2)
- The authentication SSH key pair was downloaded from the lab interface.
- An SSH connection was established to the `LinuxServer` EC2 instance (`ec2-user@ip-10-0-2-241`).

---

### Step 3: Installing the MySQL Client & Connecting to RDS
- Package repositories were updated using `sudo yum update -y`.
- The MariaDB/MySQL client utility was installed via `sudo yum install mariadb -y`.

![Client Installation](Screenshot%202026-09-12%20154336.png)

- A secure database connection was initialized from the EC2 terminal to the RDS cluster endpoint:
  ```bash
  mysql -h database-1.cluster-cnlosbf9ovtp.us-west-2.rds.amazonaws.com -P 3306 -u admin -p
  ```

![Connecting to RDS Endpoint](Screenshot%202026-09-12%20154348.png)

---

### Step 4: Database and Schema Setup

Upon successfully authenticating into the MySQL prompt, a primary database `lab_db` was created and selected for active use:

```sql
CREATE DATABASE lab_db;
USE lab_db;
```

---

### Step 5: Creating & Populating the `RESTART` Table

#### 1. Table Creation
The `RESTART` table was created to hold student details including ID, Name, City, and Graduation Date:

```sql
CREATE TABLE RESTART (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100),
    restart_city VARCHAR(100),
    graduation_date DATETIME
);
```

![Creating RESTART Table and Inserting Records](Screenshot%202026-09-12%20154433.png)

#### 2. Data Insertion
Ten sample records were inserted into the `RESTART` table:

```sql
INSERT INTO RESTART VALUES
(1, 'Alice Smith', 'Johannesburg', '2026-06-15 10:00:00'),
(2, 'Bob Jones', 'Cape Town', '2026-06-15 10:00:00'),
(3, 'Charlie Brown', 'Durban', '2026-06-15 10:00:00'),
(4, 'Diana Prince', 'Pretoria', '2026-06-15 10:00:00'),
(5, 'Evan Wright', 'Johannesburg', '2026-06-15 10:00:00'),
(6, 'Fiona Gallagher', 'Cape Town', '2026-06-15 10:00:00'),
(7, 'George Clark', 'Durban', '2026-06-15 10:00:00'),
(8, 'Hannah Abbott', 'Pretoria', '2026-06-15 10:00:00'),
(9, 'Ian Malcolm', 'Johannesburg', '2026-06-15 10:00:00'),
(10, 'Julia Roberts', 'Cape Town', '2026-06-15 10:00:00');
```

#### 3. Data Verification
A full table scan was performed to verify record insertion:

```sql
SELECT * FROM RESTART;
```

![Querying RESTART Table](Screenshot%202026-09-12%20154453.png)

---

### Step 6: Creating & Populating the `CLOUD_PRACTITIONER` Table

#### 1. Table Creation
A second table, `CLOUD_PRACTITIONER`, was constructed to track student certification dates:

```sql
CREATE TABLE CLOUD_PRACTITIONER (
    student_id INT PRIMARY KEY,
    certification_date DATETIME
);
```

#### 2. Data Insertion
Five sample records were inserted matching specific student IDs from the `RESTART` table:

```sql
INSERT INTO CLOUD_PRACTITIONER VALUES
(1, '2026-07-10 14:30:00'),
(3, '2026-07-12 09:15:00'),
(5, '2026-07-15 11:00:00'),
(7, '2026-07-18 16:45:00'),
(9, '2026-07-20 13:20:00');
```

---

### Step 7: Performing Data Joins & Final Verification

#### 1. Querying `CLOUD_PRACTITIONER`
All rows were retrieved to ensure data integrity prior to joining:

```sql
SELECT * FROM CLOUD_PRACTITIONER;
```

#### 2. Executing Inner Join
An `INNER JOIN` operation was performed between `RESTART` and `CLOUD_PRACTITIONER` on the matching key `student_id` to display student IDs, names, and their respective AWS certification dates:

```sql
SELECT 
    r.student_id, 
    r.student_name, 
    c.certification_date
FROM RESTART r
INNER JOIN CLOUD_PRACTITIONER c 
    ON r.student_id = c.student_id;
```

![SELECT CLOUD_PRACTITIONER and INNER JOIN Query Results](Screenshot%202026-09-12%20160227.png)

---

## 🎯 Result
The inner join operation successfully returned 5 matching student records, demonstrating correct relational mapping between the EC2 client application and the Amazon RDS database instance.