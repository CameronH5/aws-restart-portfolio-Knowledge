# Database Table Operations

## Lab Overview and Objectives

This lab demonstrates how to use some common database and table operations.

After completing this lab, I was able to:

- Use the CREATE statement to create databases and tables
- Use the SHOW statement to view available databases and tables
- Use the ALTER statement to alter the structure of a table
- Use the DROP statement to delete databases and tables

---

## Task 1: Connect to the Command Host

I connected to the EC2 instance configured with a database client by navigating to the AWS Management Console, selecting **Services** → **Compute** → **EC2**, and choosing the **Command Host** instance. I used the **Session Manager** tab to open a terminal window.

Once connected, I ran the following commands to configure the terminal:

```bash
sudo su
cd /home/ec2-user/
```

I then connected to the relational database instance using the MySQL command-line client:

```bash
mysql -u root --password='re:St@rt!9'
```

# Lab Report: Database Table Operations

## Lab Overview and Objectives

This lab demonstrates how to use some common database and table operations.

After completing this lab, I was able to:

- Use the CREATE statement to create databases and tables
- Use the SHOW statement to view available databases and tables
- Use the ALTER statement to alter the structure of a table
- Use the DROP statement to delete databases and tables

---

## Task 1: Connect to the Command Host

I connected to the EC2 instance configured with a database client by navigating to the AWS Management Console, selecting **Services** → **Compute** → **EC2**, and choosing the **Command Host** instance. I used the **Session Manager** tab to open a terminal window.

Once connected, I ran the following commands to configure the terminal:

```bash
sudo su
cd /home/ec2-user/
```

I then connected to the relational database instance using the MySQL command-line client:

```bash
mysql -u root --password='re:St@rt!9'
```

# Lab Report: Database Table Operations

## Lab Overview and Objectives

This lab demonstrates how to use some common database and table operations.

After completing this lab, I was able to:

- Use the CREATE statement to create databases and tables
- Use the SHOW statement to view available databases and tables
- Use the ALTER statement to alter the structure of a table
- Use the DROP statement to delete databases and tables

---

## Task 1: Connect to the Command Host

I connected to the EC2 instance configured with a database client by navigating to the AWS Management Console, selecting **Services** → **Compute** → **EC2**, and choosing the **Command Host** instance. I used the **Session Manager** tab to open a terminal window.

Once connected, I ran the following commands to configure the terminal:

```bash
sudo su
cd /home/ec2-user/
```

I then connected to the relational database instance using the MySQL command-line client:

```bash
mysql -u root --password='re:St@rt!9'
```

<img width="908" height="457" alt="Screenshot 2026-07-06 183134" src="https://github.com/user-attachments/assets/3cfbd0f0-fb8d-4864-a82c-16f1788834cd" />


---

## Task 2: Create a Database and a Table

### Step 1: View Existing Databases

I ran the following query to show the existing databases:

```sql
SHOW DATABASES;
```

The output showed three databases: `information_schema`, `mysql`, and `performance_schema`.

<img width="908" height="235" alt="Screenshot 2026-07-06 1831344" src="https://github.com/user-attachments/assets/aef79d7d-2145-49da-9cb1-3b31ee73c7ec" />


---

### Step 2: Create the "world" Database

I created a new database named **world** using the following command:

```sql
CREATE DATABASE world;
```

<img width="518" height="317" alt="Screenshot 2026-07-06 183211" src="https://github.com/user-attachments/assets/582069b9-669a-4636-ba20-eaa34d26bbdf" />


---

### Step 3: Verify Database Creation

I verified that the **world** database was created successfully:

```sql
SHOW DATABASES;
```

The output now included `world` as the fourth database.

<img width="518" height="317" alt="Screenshot 2026-07-06 183211" src="https://github.com/user-attachments/assets/96767bf4-c1a4-410c-a2ca-012c92a1e728" />



---

### Step 4: Create the "country" Table

I created a table named **country** within the `world` database with the following structure:

```sql
CREATE TABLE world.country (
    'Code' CHAR(3) NOT NULL DEFAULT '',
    'Name' CHAR(52) NOT NULL DEFAULT '',
    'Continent' enum('Asia','Europe','North America','Africa','Oceania','Antarctica','South America') NOT NULL DEFAULT 'Asia',
    'Region' CHAR(26) NOT NULL DEFAULT '',
    'SurfaceArea' FLOAT(10,2) NOT NULL DEFAULT '0.00',
    'IndepYear' SMALLINT(6) DEFAULT NULL,
    'Population' INT(11) NOT NULL DEFAULT '0',
    'LifeExpectancy' FLOAT(3,1) DEFAULT NULL,
    'GNP' FLOAT(10,2) DEFAULT NULL,
    'GNPold' FLOAT(10,2) DEFAULT NULL,
    'LocalName' CHAR(45) NOT NULL DEFAULT '',
    'GovernmentForm' CHAR(45) NOT NULL DEFAULT '',
    'HeadOfState' CHAR(60) DEFAULT NULL,
    'Capital' INT(11) DEFAULT NULL,
    'Code2' CHAR(2) NOT NULL DEFAULT '',
    PRIMARY KEY ('Code')
);
```

<img width="1350" height="400" alt="Screenshot 2026-07-06 183300" src="https://github.com/user-attachments/assets/3ff28aa2-ebae-4590-92b1-9e4178cfdfc3" />


---

### Step 5: Verify the Table Creation

I switched to the `world` database and listed all tables:

```sql
USE world;
SHOW TABLES;
```

The output confirmed that the `country` table was created.

<img width="722" height="293" alt="Screenshot 2026-07-06 183349" src="https://github.com/user-attachments/assets/f7c3bc87-232f-4e4b-9625-5319b4687a2f" />


---

### Step 6: View Table Columns

I used the `SHOW COLUMNS` query to list all columns and their properties in the `country` table:

```sql
SHOW COLUMNS FROM world.country;
```

The output showed all 15 columns with their data types, nullability, keys, and default values. I noticed that the Continent column was incorrectly spelled as "Continent".

<img width="1415" height="436" alt="Screenshot 2026-07-06 183423" src="https://github.com/user-attachments/assets/baa93e3e-4969-4c64-a931-7e542122ec33" />


---

### Step 7: Alter the Table to Fix Column Name

I used the `ALTER TABLE` command to fix the incorrectly spelled `Continent` column. I renamed it from "Continent" to "Continent":

```sql
ALTER TABLE world.country RENAME COLUMN Continent TO Continent;
```

<img width="855" height="91" alt="Screenshot 2026-07-06 183503" src="https://github.com/user-attachments/assets/2c449ca1-b2e5-4664-9564-68da64e082fd" />


---

### Step 8: Verify the Column Name Correction

I verified that the column name was corrected by running the following query:

```sql
SHOW COLUMNS FROM world.country;
```

The output confirmed that the column was now correctly named "Continent".

<img width="1415" height="433" alt="Screenshot 2026-07-06 183530" src="https://github.com/user-attachments/assets/ab2906d6-44cd-4a60-85c6-f60ab1fd46fc" />


---

## Additional Operations

### Step 9: Create and Drop the "city" Table

I created a temporary table named `city`:

```sql
CREATE TABLE world.city ('Name' CHAR(52), 'Region' CHAR(26));
```

I verified the table structure:

```sql
SHOW COLUMNS FROM world.city;
```

<img width="917" height="278" alt="Screenshot 2026-07-06 183649" src="https://github.com/user-attachments/assets/3a6fdac6-5792-40c3-bfbc-0ff5ccde8436" />


I then dropped the `city` table:

```sql
DROP TABLE world.city;
```

<img width="511" height="506" alt="Screenshot 2026-07-06 183901" src="https://github.com/user-attachments/assets/492451c2-d4fa-4c9d-875a-c37527412c7d" />


---

### Step 10: Drop the "country" Table and "world" Database

I dropped the `country` table:

```sql
DROP TABLE world.country;
```

I verified that no tables remained:

```sql
SHOW TABLES;
```

Finally, I dropped the `world` database:

```sql
DROP DATABASE world;
```

I confirmed that the database was removed:

```sql
SHOW DATABASES;
```

The output showed only the three default databases: `information_schema`, `mysql`, and `performance_schema`.

<img width="908" height="235" alt="Screenshot 2026-07-06 1831344" src="https://github.com/user-attachments/assets/95b9d5f1-8204-4957-aeb9-87e016ec5d52" />



---

## Summary

In this lab, I successfully:

1. **Connected** to the Command Host EC2 instance and authenticated to the MySQL/MariaDB database.
2. **Created** a database named `world` and a table named `country` with a detailed schema.
3. **Viewed** databases and tables using `SHOW DATABASES` and `SHOW TABLES`.
4. **Inspected** table structure using `SHOW COLUMNS`.
5. **Altered** a table schema by renaming a misspelled column from "Continent" to "Continent".
6. **Created and dropped** a temporary `city` table.
7. **Dropped** the `country` table and the `world` database.

These operations demonstrate fundamental database management skills including creation, modification, and deletion of database objects.

---

## Task 2: Create a Database and a Table

### Step 1: View Existing Databases

I ran the following query to show the existing databases:

```sql
SHOW DATABASES;
```

The output showed three databases: `information_schema`, `mysql`, and `performance_schema`.

![Show databases](Screenshot%202026-07-06%20183134.png)

---

### Step 2: Create the "world" Database

I created a new database named **world** using the following command:

```sql
CREATE DATABASE world;
```

![Create database world](Screenshot%202026-07-06%20183211.png)

---

### Step 3: Verify Database Creation

I verified that the **world** database was created successfully:

```sql
SHOW DATABASES;
```

The output now included `world` as the fourth database.

![Verify world database](Screenshot%202026-07-06%20183211.png)

---

### Step 4: Create the "country" Table

I created a table named **country** within the `world` database with the following structure:

```sql
CREATE TABLE world.country (
    'Code' CHAR(3) NOT NULL DEFAULT '',
    'Name' CHAR(52) NOT NULL DEFAULT '',
    'Continent' enum('Asia','Europe','North America','Africa','Oceania','Antarctica','South America') NOT NULL DEFAULT 'Asia',
    'Region' CHAR(26) NOT NULL DEFAULT '',
    'SurfaceArea' FLOAT(10,2) NOT NULL DEFAULT '0.00',
    'IndepYear' SMALLINT(6) DEFAULT NULL,
    'Population' INT(11) NOT NULL DEFAULT '0',
    'LifeExpectancy' FLOAT(3,1) DEFAULT NULL,
    'GNP' FLOAT(10,2) DEFAULT NULL,
    'GNPold' FLOAT(10,2) DEFAULT NULL,
    'LocalName' CHAR(45) NOT NULL DEFAULT '',
    'GovernmentForm' CHAR(45) NOT NULL DEFAULT '',
    'HeadOfState' CHAR(60) DEFAULT NULL,
    'Capital' INT(11) DEFAULT NULL,
    'Code2' CHAR(2) NOT NULL DEFAULT '',
    PRIMARY KEY ('Code')
);
```

![Create country table](Screenshot%202026-07-06%20183300.png)

---

### Step 5: Verify the Table Creation

I switched to the `world` database and listed all tables:

```sql
USE world;
SHOW TABLES;
```

The output confirmed that the `country` table was created.

![Show tables](Screenshot%202026-07-06%20183349.png)

---

### Step 6: View Table Columns

I used the `SHOW COLUMNS` query to list all columns and their properties in the `country` table:

```sql
SHOW COLUMNS FROM world.country;
```

The output showed all 15 columns with their data types, nullability, keys, and default values. I noticed that the Continent column was incorrectly spelled as "Continent".

![Show columns](Screenshot%202026-07-06%20183423.png)

---

### Step 7: Alter the Table to Fix Column Name

I used the `ALTER TABLE` command to fix the incorrectly spelled `Continent` column. I renamed it from "Continent" to "Continent":

```sql
ALTER TABLE world.country RENAME COLUMN Continent TO Continent;
```

![Alter table rename column](Screenshot%202026-07-06%20183503.png)

---

### Step 8: Verify the Column Name Correction

I verified that the column name was corrected by running the following query:

```sql
SHOW COLUMNS FROM world.country;
```

The output confirmed that the column was now correctly named "Continent".

![Verify column rename](Screenshot%202026-07-06%20183530.png)

---

## Additional Operations

### Step 9: Create and Drop the "city" Table

I created a temporary table named `city`:

```sql
CREATE TABLE world.city ('Name' CHAR(52), 'Region' CHAR(26));
```

I verified the table structure:

```sql
SHOW COLUMNS FROM world.city;
```

![Create city table](Screenshot%202026-07-06%20183649.png)

I then dropped the `city` table:

```sql
DROP TABLE world.city;
```

![Drop city table](Screenshot%202026-07-06%20183901.png)

---

### Step 10: Drop the "country" Table and "world" Database

I dropped the `country` table:

```sql
DROP TABLE world.country;
```

I verified that no tables remained:

```sql
SHOW TABLES;
```

Finally, I dropped the `world` database:

```sql
DROP DATABASE world;
```

I confirmed that the database was removed:

```sql
SHOW DATABASES;
```

The output showed only the three default databases: `information_schema`, `mysql`, and `performance_schema`.

![Drop country and world](Screenshot%202026-07-06%20183901.png)

---

## Summary

In this lab, I successfully:

1. **Connected** to the Command Host EC2 instance and authenticated to the MySQL/MariaDB database.
2. **Created** a database named `world` and a table named `country` with a detailed schema.
3. **Viewed** databases and tables using `SHOW DATABASES` and `SHOW TABLES`.
4. **Inspected** table structure using `SHOW COLUMNS`.
5. **Altered** a table schema by renaming a misspelled column from "Continent" to "Continent".
6. **Created and dropped** a temporary `city` table.
7. **Dropped** the `country` table and the `world` database.

These operations demonstrate fundamental database management skills including creation, modification, and deletion of database objects.

---

## Task 2: Create a Database and a Table

### Step 1: View Existing Databases

I ran the following query to show the existing databases:

```sql
SHOW DATABASES;
```

The output showed three databases: `information_schema`, `mysql`, and `performance_schema`.

![Show databases](Screenshot%202026-07-06%20183134.png)

---

### Step 2: Create the "world" Database

I created a new database named **world** using the following command:

```sql
CREATE DATABASE world;
```

![Create database world](Screenshot%202026-07-06%20183211.png)

---

### Step 3: Verify Database Creation

I verified that the **world** database was created successfully:

```sql
SHOW DATABASES;
```

The output now included `world` as the fourth database.

![Verify world database](Screenshot%202026-07-06%20183211.png)

---

### Step 4: Create the "country" Table

I created a table named **country** within the `world` database with the following structure:

```sql
CREATE TABLE world.country (
    'Code' CHAR(3) NOT NULL DEFAULT '',
    'Name' CHAR(52) NOT NULL DEFAULT '',
    'Continent' enum('Asia','Europe','North America','Africa','Oceania','Antarctica','South America') NOT NULL DEFAULT 'Asia',
    'Region' CHAR(26) NOT NULL DEFAULT '',
    'SurfaceArea' FLOAT(10,2) NOT NULL DEFAULT '0.00',
    'IndepYear' SMALLINT(6) DEFAULT NULL,
    'Population' INT(11) NOT NULL DEFAULT '0',
    'LifeExpectancy' FLOAT(3,1) DEFAULT NULL,
    'GNP' FLOAT(10,2) DEFAULT NULL,
    'GNPold' FLOAT(10,2) DEFAULT NULL,
    'LocalName' CHAR(45) NOT NULL DEFAULT '',
    'GovernmentForm' CHAR(45) NOT NULL DEFAULT '',
    'HeadOfState' CHAR(60) DEFAULT NULL,
    'Capital' INT(11) DEFAULT NULL,
    'Code2' CHAR(2) NOT NULL DEFAULT '',
    PRIMARY KEY ('Code')
);
```

![Create country table](Screenshot%202026-07-06%20183300.png)

---

### Step 5: Verify the Table Creation

I switched to the `world` database and listed all tables:

```sql
USE world;
SHOW TABLES;
```

The output confirmed that the `country` table was created.

![Show tables](Screenshot%202026-07-06%20183349.png)

---

### Step 6: View Table Columns

I used the `SHOW COLUMNS` query to list all columns and their properties in the `country` table:

```sql
SHOW COLUMNS FROM world.country;
```

The output showed all 15 columns with their data types, nullability, keys, and default values. I noticed that the Continent column was incorrectly spelled as "Continent".

![Show columns](Screenshot%202026-07-06%20183423.png)

---

### Step 7: Alter the Table to Fix Column Name

I used the `ALTER TABLE` command to fix the incorrectly spelled `Continent` column. I renamed it from "Continent" to "Continent":

```sql
ALTER TABLE world.country RENAME COLUMN Continent TO Continent;
```

![Alter table rename column](Screenshot%202026-07-06%20183503.png)

---

### Step 8: Verify the Column Name Correction

I verified that the column name was corrected by running the following query:

```sql
SHOW COLUMNS FROM world.country;
```

The output confirmed that the column was now correctly named "Continent".

![Verify column rename](Screenshot%202026-07-06%20183530.png)

---

## Additional Operations

### Step 9: Create and Drop the "city" Table

I created a temporary table named `city`:

```sql
CREATE TABLE world.city ('Name' CHAR(52), 'Region' CHAR(26));
```

I verified the table structure:

```sql
SHOW COLUMNS FROM world.city;
```

![Create city table](Screenshot%202026-07-06%20183649.png)

I then dropped the `city` table:

```sql
DROP TABLE world.city;
```

![Drop city table](Screenshot%202026-07-06%20183901.png)

---

### Step 10: Drop the "country" Table and "world" Database

I dropped the `country` table:

```sql
DROP TABLE world.country;
```

I verified that no tables remained:

```sql
SHOW TABLES;
```

Finally, I dropped the `world` database:

```sql
DROP DATABASE world;
```

I confirmed that the database was removed:

```sql
SHOW DATABASES;
```

The output showed only the three default databases: `information_schema`, `mysql`, and `performance_schema`.

![Drop country and world](Screenshot%202026-07-06%20183901.png)

---

## Summary

In this lab, I successfully:

1. **Connected** to the Command Host EC2 instance and authenticated to the MySQL/MariaDB database.
2. **Created** a database named `world` and a table named `country` with a detailed schema.
3. **Viewed** databases and tables using `SHOW DATABASES` and `SHOW TABLES`.
4. **Inspected** table structure using `SHOW COLUMNS`.
5. **Altered** a table schema by renaming a misspelled column from "Continent" to "Continent".
6. **Created and dropped** a temporary `city` table.
7. **Dropped** the `country` table and the `world` database.

These operations demonstrate fundamental database management skills including creation, modification, and deletion of database objects.
