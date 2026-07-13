

# Introduction to Amazon Aurora

## Lab Overview
In this lab, I explored the core functionalities of Amazon Aurora.
**Topics covered and actions performed:**
* Created an Aurora database instance.
* Connected to a pre-created Amazon Elastic Compute Cloud (Amazon EC2) instance.
* Configured the Amazon EC2 instance to connect to the Aurora database.
* Queried the Aurora database instance by creating tables, inserting data, and filtering records.

---

## Task 1: Create an Aurora Instance
I began by launching an Aurora database instance. I navigated to the **RDS** service in the AWS Management Console, selected **Databases** from the left pane, and clicked **Create database**.

I configured the instance using the **Standard create** method. For the database engine, I chose **Aurora (MySQL Compatible)** and selected the version specified as default for major version 8.0. To meet the lab requirements, I chose the **Dev/Test** template. 
<img width="952" height="702" alt="Screenshot 2026-07-13 192633" src="https://github.com/user-attachments/assets/c9931a13-ea7e-4fd7-8553-ad4b4eb02324" />
<img width="1798" height="177" alt="Screenshot 2026-07-13 192646" src="https://github.com/user-attachments/assets/ccf8a354-1fad-4fda-8346-1d60033d7f4f" />



Next, in the **Settings** section, I defined the database cluster identifier as `aurora`, set the master username as `admin`, and manually configured the master password and confirmed it as `admin123`.
<img width="1622" height="625" alt="Screenshot 2026-07-13 192818" src="https://github.com/user-attachments/assets/8b2869fc-66ea-41da-9778-ce04212c7bd6" />
<img width="1186" height="250" alt="Screenshot 2026-07-13 192827" src="https://github.com/user-attachments/assets/041e6a4a-c921-4b5f-95a8-35ba7763c360" />


For the **Instance configuration**, I selected **Burstable classes (includes t classes)** and chose the `db.t3.medium` instance type. In the **Availability & durability** section, I chose `Don't create an Aurora Replica` because this is a lab environment.
<img width="1847" height="402" alt="Screenshot 2026-07-13 193014" src="https://github.com/user-attachments/assets/de35618b-2bbb-47c1-bc68-5224fcb766b4" />
<img width="802" height="187" alt="Screenshot 2026-07-13 193033" src="https://github.com/user-attachments/assets/8c50df68-0146-47e1-b57f-4d29bab2c458" />



Under the **Connectivity** section, I configured the instance to use the `LabVPC`, selected the `dbsubnetgroup` for the DB subnet group, and set **Public access** to `No`. For the VPC security group, I chose to **Choose existing**, removed the default security group, and selected `DBSecurityGroup`. 
<img width="1620" height="782" alt="Screenshot 2026-07-13 193136" src="https://github.com/user-attachments/assets/f4d8f282-da3b-4f37-85a7-4ad81a06a4dd" />
<img width="1301" height="276" alt="Screenshot 2026-07-13 193220" src="https://github.com/user-attachments/assets/610e9cfa-ed42-4106-95fa-dddb52f5c30c" />



In the **Monitoring** section, I ensured that `Enable Enhanced monitoring` was unchecked. I expanded the **Additional configuration** section and entered `world` as the initial database name. I cleared the check box for `Enable encryption` (per the instructions) and enabled auto minor version upgrade. Finally, I scrolled to the bottom and clicked **Create database**. 
<img width="887" height="162" alt="Screenshot 2026-07-13 193355" src="https://github.com/user-attachments/assets/0bb05049-319e-4caa-8299-9ffb57187a72" />
<img width="1235" height="181" alt="Screenshot 2026-07-13 193423" src="https://github.com/user-attachments/assets/839d29f4-842f-44cb-8bec-10aa8545eb8c" />
<img width="1202" height="156" alt="Screenshot 2026-07-13 193639" src="https://github.com/user-attachments/assets/28dfd597-fb77-408f-b156-417ebf5d9076" />




After clicking create, I waited a few minutes for the database instance status to change to **Available**. A green banner at the top confirmed the successful creation of the `aurora` database.
<img width="1307" height="388" alt="Screenshot 2026-07-13 193756" src="https://github.com/user-attachments/assets/7a2c489a-2e41-4017-94f3-6e6cfc4348b6" />


## Task 2: Connect to an Amazon EC2 Linux instance
Once the database was provisioned, I switched over to the EC2 service. In the EC2 dashboard, I went to the **Instances** menu. I located the instance labeled **Command Host**, selected its check box, and clicked the **Connect** button.
I chose **Session Manager** as the connection method and clicked **Connect**. This successfully opened a terminal window, providing me with command-line access to the EC2 instance.
<img width="1115" height="251" alt="Screenshot 2026-07-13 193853" src="https://github.com/user-attachments/assets/8e63ba6d-a269-46ab-98ab-26ce1185084a" />


## Task 3: Configure the Amazon EC2 Linux instance to connect to Aurora
In the EC2 terminal, I used the `yum` package manager to install the MariaDB client. This client would allow me to connect to the Aurora instance from the EC2 command line. I ran the following command:

```bash
sudo yum install mariadb -y
```
<img width="1892" height="738" alt="Screenshot 2026-07-13 193922" src="https://github.com/user-attachments/assets/11ecd0c5-12ac-4246-986a-f376a6180b00" />


With the client installed, I needed the database endpoint. I returned to the RDS console, opened the `aurora` database details, and went to the **Connectivity & security** tab. I copied the **Endpoint name** for the Writer instance. 
I returned to the EC2 Session Manager terminal and modified the provided connection string with my specific endpoint. I established the connection to the Aurora instance using the following command:

```bash
mysql -u admin --password='admin123' -h aurora.cluster-c9dqlebhreqg.us-west-2.rds.amazonaws.com
```
The terminal output confirmed a successful connection, displaying "Welcome to the MariaDB monitor" and a `MySQL [(none)]>` prompt.
<img width="1055" height="172" alt="Screenshot 2026-07-13 194624" src="https://github.com/user-attachments/assets/c78a9998-4cd4-4923-b556-7c3db49dd8ed" />


## Task 4: Create a table and insert and query records
With the MySQL shell open, I began interacting with the database.

First, I listed the available databases to verify my Aurora instance was set up correctly. I ran:
```sql
SHOW DATABASES;
```
The output correctly listed `information_schema`, `mysql`, `performance_schema`, `sys`, and `world`.
<img width="353" height="252" alt="Screenshot 2026-07-13 194655" src="https://github.com/user-attachments/assets/30fcd2ce-8e5c-4eef-90fd-db04661a9ff3" />


I then switched my connection context to the `world` database using:
```sql
USE world;
```
<img width="263" height="55" alt="Screenshot 2026-07-13 194712" src="https://github.com/user-attachments/assets/b375068d-3b7b-4563-ae77-88493b41e2e7" />


Next, I executed a `CREATE TABLE` statement to build a `country` table. I had a minor syntax error on my first attempt (I typed `sql` at the start of the prompt), but I immediately corrected the command and successfully ran the table creation script.
<img width="1317" height="376" alt="Screenshot 2026-07-13 194859" src="https://github.com/user-attachments/assets/180e1de8-4ce8-475d-898b-494ff03437ec" />




Once the table was created, I populated it by inserting records for Gabon, Ireland, Thailand, Costa Rica, and Australia using a series of `INSERT INTO` statements.
<img width="1892" height="396" alt="Screenshot 2026-07-13 194931" src="https://github.com/user-attachments/assets/9f511e61-ccdb-4a6f-adfc-a106e0b5e785" />


Finally, to confirm my data and practice filtering, I executed a `SELECT` query to find countries with a GNP greater than 35,000 and a population greater than 10,000,000:
```sql
SELECT * FROM country WHERE GNP > 35000 and Population > 10000000;

```
The query successfully returned the two expected records: Australia and Thailand. 
<img width="1895" height="287" alt="Screenshot 2026-07-13 194954" src="https://github.com/user-attachments/assets/ef99b762-6251-4e98-8a82-dde3ff304970" />
