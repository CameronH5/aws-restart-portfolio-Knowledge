# Build Your DB Server and Interact With Your DB Using an App

## Lab Objectives
I completed this lab to learn how to:
* Launch an Amazon RDS DB instance with high availability.
* Configure the DB instance to permit connections from my web server.
* Open a web application and interact with the database.

---

## Task 1: Create a Security Group for the RDS DB Instance
To allow the web server to securely access the database instance, I started by setting up a dedicated security group. 

I navigated to the **VPC** service on the AWS Management Console and clicked on **Security groups** in the left navigation pane. I then initiated the **Create security group** workflow. I configured the group with the name **DB Security Group** and provided a description, ensuring it was placed within the **Lab VPC**. 

For the **Inbound rules**, I added a new rule to allow inbound database requests. I selected **MySQL/Aurora (3306)** as the type, kept the source type as **Custom**, and then searched for and selected the existing **Web Security Group**. This effectively configured the database security group to permit inbound traffic on port 3306 from any EC2 instance that is associated with the web server's security group. I finished by clicking **Create security group**.

<img width="1856" height="757" alt="Screenshot 2026-07-13 184338" src="https://github.com/user-attachments/assets/5e7e47be-3e44-465b-9777-81e7b5401cc0" />
<img width="1612" height="595" alt="Screenshot 2026-07-13 184358" src="https://github.com/user-attachments/assets/6098f541-3a86-4413-ab78-a3889123daaf" />


## Task 2: Create a DB Subnet Group
Next, I configured a DB subnet group to tell RDS which specific subnets it could use for the database. Because this task required high availability, I had to ensure the group included subnets in at least two Availability Zones.

To do this, I navigated to **Aurora and RDS** in the AWS console and clicked on **Subnet groups** in the left panel. I clicked **Create DB subnet group** and gave it the name and description **DB Subnet Group**, attaching it to the **Lab VPC**. 

For the **Availability Zones**, I selected the first two available zones. In the **Subnets** section, I explicitly chose **Private Subnet 1** (10.0.1.0/24) and **Private Subnet 2** (10.0.3.0/24). After verifying these selections, I clicked the **Create** button to finalize the subnet group creation.

<img width="1366" height="766" alt="Screenshot 2026-07-13 184533" src="https://github.com/user-attachments/assets/02feee89-f79a-4309-9504-0e1006291ef8" />

<img width="1838" height="511" alt="Screenshot 2026-07-13 184556" src="https://github.com/user-attachments/assets/c82149b7-ee47-480a-b8b0-de3c53b51b85" />

<img width="587" height="62" alt="Screenshot 2026-07-13 184606" src="https://github.com/user-attachments/assets/edb65040-ae00-4e0f-bbfc-b2609ddf3472" />


## Task 3: Create an Amazon RDS DB Instance
I then proceeded to configure and launch a highly available Amazon RDS for MySQL database instance. I selected **Databases** from the left navigation pane and clicked the **Create database** dropdown, choosing **Full configuration** to customize the setup.

In the engine options, I selected **MySQL**. I chose **Dev/Test** for the templates, and to ensure high availability and durability, I selected **Multi-AZ DB instance deployment (2 instances)**. I left the engine version at the default setting.

Under **Settings**, I entered `lab-db` for the DB instance identifier and set `main` as the master username. For **Credential Settings**, I chose **Self managed**, cleared the "Auto generate password" option, and manually set and confirmed the password as `lab-password`. I left the authentication option as **Password authentication**.

For **Instance configuration**, I selected **Burstable classes (includes t classes)** and chose the `db.t3.medium` instance type. I set the **Storage** to `20` GiB of **General Purpose SSD (gp3)**.

For the **Connectivity** settings, I configured:
* **Compute resource:** Don't connect to an EC2 compute resource.
* **Virtual Private Cloud (VPC):** Lab VPC.
* **DB subnet group:** db subnet group.
* **Public access:** No (keeping the database private).
* **VPC security group (firewall):** Choose existing -> **DB Security Group**.

Finally, under **Additional configuration**, I defined the initial database name as `lab`. To speed up the deployment, I disabled automated backups and disabled enhanced monitoring/performance insights as per the lab instructions. I clicked **Create database** to launch the instance. Because it was a Multi-AZ deployment, I waited approximately 4 minutes for the status to change to **Available**.

<img width="1861" height="728" alt="Screenshot 2026-07-13 184817" src="https://github.com/user-attachments/assets/a97d38ed-9a3a-4820-98f9-50c8a6ee05ab" />

<img width="1811" height="603" alt="Screenshot 2026-07-13 184843" src="https://github.com/user-attachments/assets/3c3f82e5-cd71-469c-9fe0-d5f5a3bbc0d4" />

<img width="1558" height="792" alt="Screenshot 2026-07-13 184952" src="https://github.com/user-attachments/assets/9ff5f411-e624-44c1-a9f6-f8b05e3e2fb4" />

<img width="1826" height="670" alt="Screenshot 2026-07-13 185101" src="https://github.com/user-attachments/assets/21322144-1cf3-4284-aa9d-0a90c08c773f" />

<img width="1392" height="792" alt="Screenshot 2026-07-13 185156" src="https://github.com/user-attachments/assets/b641b0ed-7ade-41bb-b51f-469ea10fa626" />

<img width="1808" height="765" alt="Screenshot 2026-07-13 185250" src="https://github.com/user-attachments/assets/e26c2cf0-5492-4c8f-8056-fef7c1661c98" />

<img width="1817" height="388" alt="Screenshot 2026-07-13 185337" src="https://github.com/user-attachments/assets/b8db38dc-aa69-4d9c-a40f-a8bb211b92bb" />

<img width="1842" height="531" alt="Screenshot 2026-07-13 185415" src="https://github.com/user-attachments/assets/92fa175d-89e2-4425-8196-345c1f227e2f" />

<img width="1355" height="817" alt="Screenshot 2026-07-13 185441" src="https://github.com/user-attachments/assets/464269c2-5837-4489-9883-4563c8c9a8d7" />


## Task 4: Interact with Your Database
Once the database was active, I retrieved its details to connect it to the web application. First, I clicked on the `lab-db` link, navigated to the **Connectivity & security** tab, and copied the **Endpoint** to my clipboard.

Next, I opened a new web browser tab and navigated to the **WebServer IP address** provided in the lab instructions. This loaded the web application, which displayed the EC2 metadata and the current CPU load. I clicked the **RDS** tab at the top of the web application page to open the configuration interface.

Inside the configuration screen, I pasted the copied **Endpoint**, and entered the corresponding database details:
* **Database:** `lab`
* **Username:** `main`
* **Password:** `lab-password`

After clicking **Submit**, the web application returned a status message showing it was executing a command to copy the database structure and data into the RDS instance. The command executed was `mysql -u main -plab-password -h [my-endpoint] lab < sql/addressbook.sql`. 

After a few seconds, the application redirected me to the **Address Book**, confirming it was successfully connected to my RDS database. Finally, I tested the application's functionality by adding, editing, and removing contacts. I verified that the data was properly persisted to the Multi-AZ database environment.

<img width="1817" height="767" alt="Screenshot 2026-07-13 190057" src="https://github.com/user-attachments/assets/08e854f6-e208-48ab-a310-47e6c7c082dc" />

<img width="1367" height="552" alt="Screenshot 2026-07-13 190200" src="https://github.com/user-attachments/assets/d19d0f2b-0dab-406b-8b6a-f9a51486f43b" />

<img width="1457" height="537" alt="Screenshot 2026-07-13 190248" src="https://github.com/user-attachments/assets/053270b7-594a-4ef6-a656-ae251ed4a852" />
<img width="996" height="383" alt="Screenshot 2026-07-13 190313" src="https://github.com/user-attachments/assets/113955fb-3f9e-4612-833a-33a1179b4c82" />

