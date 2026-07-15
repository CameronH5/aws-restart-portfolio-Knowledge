# Migrating to Amazon RDS

## Lab Overview
In this lab, I migrated a café web application from using a local database on an EC2 instance to a fully managed Amazon Relational Database Service (Amazon RDS) database instance. I began by generating some data on the existing local database, built the required RDS components and architecture, migrated the data to the new RDS instance, and finally reconfigured the café application to connect to Amazon RDS. 

## Task 1: Generating order data on the café website
First, I browsed the café website and placed some orders to create data that would later be migrated to the new RDS instance. 
1. I opened the **CafeInstanceURL** in a new browser window. 
2. I navigated to the **Menu** page, added at least one of every item to the order, and then submitted the order. 
3. I proceeded to the **Order History** page to record the exact number of orders I placed, noting that I would compare this number after the database migration was complete to verify the data integrity.

<img width="1302" height="736" alt="Screenshot 2026-07-15 102445" src="https://github.com/user-attachments/assets/92a11553-aaba-4712-a8f6-976a0389182d" />

<img width="1272" height="813" alt="Screenshot 2026-07-15 102459" src="https://github.com/user-attachments/assets/a4d8e35d-1ed8-4746-9cfd-dc01c3eff3c0" />


## Task 2: Creating an Amazon RDS instance by using the AWS CLI

### Task 2.1: Connecting to the CLI Host instance
To begin the infrastructure build, I established a connection to the **CLI Host** EC2 instance. I used the AWS Management Console to select the instance and utilized **EC2 Instance Connect** to securely open a terminal session.

![Screenshot: EC2 Instance Connect interface for the CLI Host](your_image_20.png)

### Task 2.2: Configuring the AWS CLI
From within the EC2 Instance Connect terminal, I configured the AWS CLI by running the `aws configure` command. I pasted the `AccessKey`, `SecretKey`, set the `LabRegion` as the default region, and `json` as the default output format. 

<img width="1272" height="813" alt="Screenshot 2026-07-15 102459" src="https://github.com/user-attachments/assets/6bc696a9-1f2c-49c1-8eae-c2373e2a953d" />
<img width="1715" height="787" alt="Screenshot 2026-07-15 102653" src="https://github.com/user-attachments/assets/c04a5b0a-b2ab-4585-a4f6-03e91e4e44e3" />
<img width="743" height="138" alt="Screenshot 2026-07-15 102821" src="https://github.com/user-attachments/assets/8301831d-cb63-412f-a077-af78333da76a" />


### Task 2.3: Creating prerequisite components
Next, I ran a series of AWS CLI commands to create the networking components and security groups necessary for the RDS instance.
*   **Created the Security Group:** I ran `aws ec2 create-security-group` with the group name `CafeDatabaseSG` and the VPC ID I recorded earlier. 
*   **Added an Inbound Rule:** I executed `aws ec2 authorize-security-group-ingress` to configure an inbound rule that allows TCP connections on port 3306 (MySQL) from instances within the `CafeSecurityGroup`. I confirmed the rule was applied properly by running `aws ec2 describe-security-groups`.

<img width="630" height="167" alt="Screenshot 2026-07-15 103025" src="https://github.com/user-attachments/assets/363e740e-3fef-43cb-9022-e5bbdc917839" />

<img width="737" height="106" alt="Screenshot 2026-07-15 103232" src="https://github.com/user-attachments/assets/b75ae0a4-d7dc-4174-ae49-d7d0c374cfa9" />

<img width="688" height="522" alt="Screenshot 2026-07-15 103310" src="https://github.com/user-attachments/assets/be3b3415-728c-46dd-823e-57d044c9a74d" />


*   **Created Private Subnet 1:** Using `aws ec2 create-subnet`, I created the first private subnet with a CIDR block of `10.200.2.0/23` in the same Availability Zone as the CafeInstance (`us-west-2a`).
*   **Created Private Subnet 2:** I created a second private subnet in a different Availability Zone (`us-west-2b`) with a non-overlapping CIDR block of `10.200.10.0/23`.
*   **Created the DB Subnet Group:** I executed `aws rds create-db-subnet-group` to define a DB subnet group named `CafeDB Subnet Group`, adding both private subnet IDs into a single group that the RDS instance could use.

<img width="936" height="463" alt="Screenshot 2026-07-15 103512" src="https://github.com/user-attachments/assets/ae1ea73e-5b14-42d6-8f3b-4555c54a1fa9" />

<img width="917" height="468" alt="Screenshot 2026-07-15 103640" src="https://github.com/user-attachments/assets/5f82b9d5-c681-4f86-bfba-2bc0cee29240" />

<img width="961" height="666" alt="Screenshot 2026-07-15 104032" src="https://github.com/user-attachments/assets/6a2c9734-3ff8-4e60-b557-41016a9658dd" />


### Task 2.4: Creating the Amazon RDS MariaDB instance
I initiated the creation of the RDS database instance using `aws rds create-db-instance`. I configured the instance with the identifier `CafeDBInstance`, the MariaDB engine, a `db.t3.micro` instance class, and the security group and subnet group I had just created, ensuring it was not publicly accessible. 
After running the command, I monitored the status of the new database instance by repeatedly running `aws rds describe-db-instances`. The status progressed from `creating`, to `backing-up`, and eventually to `available`. Once available, I recorded the database **Endpoint Address** returned in the output, as it was required for later steps.

<img width="617" height="228" alt="Screenshot 2026-07-15 105107" src="https://github.com/user-attachments/assets/c5ad3446-0e10-430d-9284-6427ea37c21f" />

<img width="1263" height="272" alt="Screenshot 2026-07-15 105144" src="https://github.com/user-attachments/assets/8cc175f6-f07d-4b9c-86b1-fe0f89398db3" />

<img width="1896" height="426" alt="Screenshot 2026-07-15 105545" src="https://github.com/user-attachments/assets/29f68637-1324-41fd-898f-71683cda5f61" />

<img width="1902" height="240" alt="Screenshot 2026-07-15 105722" src="https://github.com/user-attachments/assets/4da6c1af-a1a2-4fdb-9cd8-4a2083f65016" />


## Task 3: Migrating application data to the Amazon RDS instance
With the RDS infrastructure created, I switched my session to connect to the **CafeInstance** (rather than the CLI Host) using EC2 Instance Connect to access the source database.
1. **Generated a Backup:** On the local instance, I used `mysqldump` to export the existing local `cafe_db` database to a file named `cafedb-backup.sql`.
2. **Downloaded the CA Certificate:** To connect to the RDS instance with SSL encryption, I used `curl` to download the RDS CA certificate bundle (`global-bundle.pem`).
3. **Restored the Backup:** I connected to the Amazon RDS database using the `mysql` command-line tool, specifying the endpoint address I recorded earlier along with the SSL certificate. By using input redirection (`< cafedb-backup.sql`), I restored the database schema and data into the new RDS instance.
4. **Verified the Migration:** To ensure the migration was successful, I opened an interactive SQL session to the Amazon RDS instance and executed the SQL statement `select * from product;`. The output successfully displayed the same 9 rows representing the café menu items that were in the original database.

<img width="808" height="181" alt="Screenshot 2026-07-15 110327" src="https://github.com/user-attachments/assets/5f65529f-1e02-4a4f-90be-c657f34d97e4" />

<img width="1031" height="668" alt="Screenshot 2026-07-15 110356" src="https://github.com/user-attachments/assets/2bcb5782-d724-4627-ba68-d10e678e23ce" />

<img width="1277" height="123" alt="Screenshot 2026-07-15 110506" src="https://github.com/user-attachments/assets/88b7500e-6e78-4a14-b74a-08696ebeacad" />

<img width="758" height="115" alt="Screenshot 2026-07-15 110627" src="https://github.com/user-attachments/assets/d4678e03-6aed-4cdc-b5e8-03e13233784b" />

<img width="803" height="326" alt="Screenshot 2026-07-15 110816" src="https://github.com/user-attachments/assets/9e08bb13-7b8d-4fd3-8ca3-6109d36e6c5a" />

<img width="1721" height="351" alt="Screenshot 2026-07-15 110843" src="https://github.com/user-attachments/assets/fa76799a-78b2-46a8-b66e-1ae2f8887874" />

<img width="330" height="83" alt="Screenshot 2026-07-15 110858" src="https://github.com/user-attachments/assets/c17bf7ea-1241-4218-9010-3fe793ee3325" />

## Task 4: Configuring the website to use the Amazon RDS instance
After successfully migrating the data, I pointed the café application to the new database. 
1. I navigated to **AWS Systems Manager** in the AWS Management Console and opened the **Parameter Store**.
2. I selected the `/cafe/dbUrl` parameter and clicked **Edit**. 
3. In the value field, I replaced the existing connection string with the endpoint address of my newly created Amazon RDS instance, then saved the changes.

<img width="1270" height="623" alt="Screenshot 2026-07-15 111012" src="https://github.com/user-attachments/assets/a2b69cce-979b-42a0-90b4-cb7ed1602094" />

<img width="731" height="650" alt="Screenshot 2026-07-15 111043" src="https://github.com/user-attachments/assets/2d8c4499-91c6-4cd6-adff-954015fb7661" />


4. To test the configuration, I returned to the café website browser window and refreshed the **Order History** page. The page correctly displayed the same initial order that I had placed in Task 1, proving the application was successfully reading from the managed RDS database.

<img width="1307" height="805" alt="Screenshot 2026-07-15 111138" src="https://github.com/user-attachments/assets/04efb923-cf2d-4c42-b171-ae75d1e2d549" />


## Task 5: Monitoring the Amazon RDS database
Finally, I explored the monitoring capabilities of Amazon RDS.
1. In the AWS Management Console, I navigated to the **RDS Management Console** and selected the `cafedbinstance`. I accessed the **Monitoring** tab, which displayed various performance metrics from CloudWatch, such as `CPUUtilization`, `DatabaseConnections`, and `FreeableMemory`.
2. I opened an interactive SQL session to the RDS database from the CafeInstance terminal. Immediately, I observed the **DatabaseConnections** metric graph spike to a value of 1, confirming a live connection.
3. I closed the connection by entering `exit` in the terminal. After waiting about a minute for the metric sampling interval, I refreshed the **DatabaseConnections** graph, which successfully dropped back down to 0.

<img width="1712" height="518" alt="Screenshot 2026-07-15 111455" src="https://github.com/user-attachments/assets/bb1c4e70-01cb-41b6-9b80-f0cdbe3f7dd0" />

<img width="473" height="390" alt="Screenshot 2026-07-15 111726" src="https://github.com/user-attachments/assets/bac6f1b1-5a4f-47fa-899a-bd5951dee4b2" />

<img width="473" height="378" alt="Screenshot 2026-07-15 111927" src="https://github.com/user-attachments/assets/e375624d-1701-40ff-90da-7279012923b3" />


### Conclusion
In this lab, I successfully created an Amazon RDS MariaDB instance by using the AWS CLI, migrated data from a local MariaDB database running on an EC2 instance to the newly created Amazon RDS instance, and monitored the performance of the Amazon RDS database instance using Amazon CloudWatch metrics.
