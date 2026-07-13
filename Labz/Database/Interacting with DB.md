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

![Task 1: Security Group Configuration](Screenshot_Task1_1.png)
![Task 1: Security Group Created Successfully](Screenshot_Task1_2.png)

## Task 2: Create a DB Subnet Group
Next, I configured a DB subnet group to tell RDS which specific subnets it could use for the database. Because this task required high availability, I had to ensure the group included subnets in at least two Availability Zones.

To do this, I navigated to **Aurora and RDS** in the AWS console and clicked on **Subnet groups** in the left panel. I clicked **Create DB subnet group** and gave it the name and description **DB Subnet Group**, attaching it to the **Lab VPC**. 

For the **Availability Zones**, I selected the first two available zones. In the **Subnets** section, I explicitly chose **Private Subnet 1** (10.0.1.0/24) and **Private Subnet 2** (10.0.3.0/24). After verifying these selections, I clicked the **Create** button to finalize the subnet group creation.

![Task 2: Creating the DB Subnet Group](Screenshot_Task2_1.png)
![Task 2: Selected Subnets within the Availability Zones](Screenshot_Task2_2.png)
![Task 2: DB Subnet Group Created Successfully](Screenshot_Task2_3.png)

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

![Task 3: Selecting MySQL and Full Configuration](Screenshot_Task3_1.png)
![Task 3: Selecting Multi-AZ deployment](Screenshot_Task3_2.png)
![Task 3: Configuring DB identifier and Settings](Screenshot_Task3_3.png)
![Task 3: Configuring Master Password](Screenshot_Task3_4.png)
![Task 3: Selecting Instance Configuration](Screenshot_Task3_5.png)
![Task 3: Configuring Connectivity and VPC Settings](Screenshot_Task3_6.png)
![Task 3: Choosing the Existing VPC Security Group](Screenshot_Task3_7.png)
![Task 3: Additional Configuration and Database Name](Screenshot_Task3_8.png)

## Task 4: Interact with Your Database
Once the database was active, I retrieved its details to connect it to the web application. First, I clicked on the `lab-db` link, navigated to the **Connectivity & security** tab, and copied the **Endpoint** to my clipboard.

Next, I opened a new web browser tab and navigated to the **WebServer IP address** provided in the lab instructions. This loaded the web application, which displayed the EC2 metadata and the current CPU load. I clicked the **RDS** tab at the top of the web application page to open the configuration interface.

Inside the configuration screen, I pasted the copied **Endpoint**, and entered the corresponding database details:
* **Database:** `lab`
* **Username:** `main`
* **Password:** `lab-password`

After clicking **Submit**, the web application returned a status message showing it was executing a command to copy the database structure and data into the RDS instance. The command executed was `mysql -u main -plab-password -h [my-endpoint] lab < sql/addressbook.sql`. 

After a few seconds, the application redirected me to the **Address Book**, confirming it was successfully connected to my RDS database. Finally, I tested the application's functionality by adding, editing, and removing contacts. I verified that the data was properly persisted to the Multi-AZ database environment.

![Task 4: Retrieving the RDS Endpoint](Screenshot_Task4_1.png)
![Task 4: Web Application Landing Page and CPU Load](Screenshot_Task4_2.png)
![Task 4: Application Configuring the Database Connection and Running MySQL Command](Screenshot_Task4_3.png)
![Task 4: Successfully Loaded and Interactive Address Book Application](Screenshot_Task4_4.png)
