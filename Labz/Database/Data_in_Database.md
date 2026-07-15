# Insert, Update, and Delete Data in a Database

## Lab Overview and Objectives

This lab demonstrated how to insert, update, delete, and import rows of data using structured query language (SQL).

After completing this lab, I was able to:

- Insert rows into a table
- Update rows in a table
- Delete rows from a table
- Import rows from a database backup file

---

## Task 1: Connect to a Database

I connected to the Command Host EC2 instance using AWS Systems Manager Session Manager. After logging in, I ran the following commands to configure the environment and connect to the MariaDB database:

```bash
sudo su
cd /home/ec2-user/
mysql -u root --password='re:St@rt!9'
```

Once connected, I verified the existing databases:

```sql
SHOW DATABASES;
```

The output confirmed that the `world` database already existed (from the previous lab).

<img width="803" height="480" alt="Screenshot 2026-07-06 185650" src="https://github.com/user-attachments/assets/a7beeb68-9e30-4b52-94a0-8a9905dc5a9c" />


---

## Task 2: Insert Data into a Table

### Step 1: Verify the `country` Table Exists

I first checked the structure of the `country` table by running:

```sql
SELECT * FROM world.country;
```

Initially, the table was empty (no rows were present because the table had been dropped earlier, but in this lab it was pre‑created).

### Step 2: Insert Two Rows

I inserted two rows into the `world.country` table using the `INSERT INTO` statement. The values were provided in the same order as the table schema.

```sql
INSERT INTO world.country VALUES ('IRL', 'Ireland', 'Europe', 'British Islands', '70273.00', 1921, 3775100, 76.8, 75921.00, 73132.00, 'Ireland/Eire', 'Republic', 1447, 'IE');
INSERT INTO world.country VALUES ('AUS', 'Australia', 'Oceania', 'Australia and New Zealand', '7741220.00', 1901, 18886000, 79.8, 351182.00, 392911.00, 'Australia', 'Constitutional Monarchy, Federation', 135, 'AU');
```

### Step 3: Verify the Inserts

I ran a `SELECT` query with a `WHERE` clause to confirm that both rows were added:

```sql
SELECT * FROM world.country WHERE Code IN ('IRL', 'AUS');
```

The output showed the two rows as expected.

<img width="1901" height="557" alt="Screenshot 2026-07-06 185735" src="https://github.com/user-attachments/assets/3ff5f721-04b2-4f0c-ae2a-c1a3cadd74bb" />


---

## Task 3: Update Rows in a Table

### Step 1: Set Population to 0 for All Rows

I updated the `Population` column for every row in the `country` table (without a `WHERE` clause, so all rows were affected).

```sql
UPDATE world.country SET Population = 0;
```

The query affected 2 rows (the ones I had inserted). I verified the change with:

```sql
SELECT * FROM world.country;
```

The output showed that both rows now had `Population = 0`.

<img width="1905" height="406" alt="Screenshot 2026-07-06 185812" src="https://github.com/user-attachments/assets/dfd8f22d-d776-406d-ab8b-2148e5fde7de" />


### Step 2: Update Both Population and SurfaceArea

I then updated both the `Population` and `SurfaceArea` columns for all rows in the table.

```sql
UPDATE world.country SET Population = 100, SurfaceArea = 100;
```

Again, both rows were updated. I verified the changes:

```sql
SELECT * FROM world.country;
```

The output confirmed that `Population` and `SurfaceArea` were now set to 100 for both rows.

<img width="1907" height="412" alt="Screenshot 2026-07-06 185835" src="https://github.com/user-attachments/assets/fa1fc3e0-10a7-4dbb-a0b6-9f5c46f86f72" />


---

## Task 4: Delete Rows from a Table

### Step 1: Disable Foreign Key Checks

Because the `country` table might be referenced by other tables (such as `city` and `countrylanguage`), I temporarily disabled foreign key checks to allow the deletion.

```sql
SET FOREIGN_KEY_CHECKS = 0;
```

### Step 2: Delete All Rows

I deleted all rows from the `country` table using a `DELETE` statement without a `WHERE` clause.

```sql
DELETE FROM world.country;
```

The query affected 2 rows.

### Step 3: Verify Deletion

I ran a `SELECT` query to confirm that the table was empty:

```sql
SELECT * FROM world.country;
```

The result was an empty set.

<img width="510" height="216" alt="Screenshot 2026-07-06 185900" src="https://github.com/user-attachments/assets/7d0403f3-ab04-4bba-a775-b9af54ef191f" />


---

## Task 5: Import Data Using an SQL File

### Step 1: Exit the MySQL Client

I exited the MariaDB client with the `QUIT` command.

```sql
QUIT;
```

<img width="357" height="85" alt="Screenshot 2026-07-06 185914" src="https://github.com/user-attachments/assets/f99c3c63-2b76-4dbc-99e8-ecf75366a26d" />


### Step 2: Verify the SQL Backup File

I checked that the SQL backup file `world.sql` was present in the `/home/ec2-user/` directory.

```bash
ls /home/ec2-user/world.sql
```

The file existed.

### Step 3: Import the SQL File

I imported the SQL file using the MySQL client in batch mode. This file contained statements to create additional tables (`city` and `countrylanguage`) and insert sample data into all three tables.

```bash
mysql -u root --password='re:St@rt!9' < /home/ec2-user/world.sql
```

The import ran without errors.

### Step 4: Reconnect to the Database

After the import, I reconnected to the MariaDB client.

```bash
mysql -u root --password='re:St@rt!9'
```

<img width="1043" height="270" alt="Screenshot 2026-07-06 185946" src="https://github.com/user-attachments/assets/e5d0835f-1a07-48a7-81e0-4102d8ab8a01" />


### Step 5: Verify the Imported Tables

I switched to the `world` database and listed all tables.

```sql
USE world;
SHOW TABLES;
```

The output showed three tables: `city`, `country`, and `countrylanguage`.

<img width="683" height="331" alt="Screenshot 2026-07-06 190021" src="https://github.com/user-attachments/assets/73be3b98-319a-4ad6-86d8-10c530eae775" />

### Step 6: Verify the Data in the `country` Table

I queried the `country` table to confirm that the sample data was loaded.

```sql
SELECT * FROM country;
```

The result showed 237 rows (the full world database sample), confirming a successful import.

<img width="683" height="331" alt="Screenshot 2026-07-06 190021" src="https://github.com/user-attachments/assets/39ffc745-8346-45a4-bf44-ee30bf886c15" />


The final output confirmed that 237 rows were returned.

<img width="1918" height="477" alt="Screenshot 2026-07-06 190059" src="https://github.com/user-attachments/assets/b83a8087-e76e-4e29-8d7e-291cf21f7165" />
<img width="325" height="133" alt="Screenshot 2026-07-06 190107" src="https://github.com/user-attachments/assets/85a39f67-fb61-4f71-92ed-a3e29d2e95bd" />


---

## Summary

In this lab, I successfully:

1. **Connected** to the Command Host and authenticated to the MariaDB database.
2. **Inserted** two rows into the `country` table using `INSERT INTO`.
3. **Updated** rows in the table: first setting `Population` to 0, then setting both `Population` and `SurfaceArea` to 100.
4. **Deleted** all rows from the table after disabling foreign key checks.
5. **Imported** a full dataset from an SQL backup file, which added two additional tables and populated all three with sample data.

These operations demonstrate essential data manipulation skills in SQL, including inserting, updating, deleting, and bulk importing data, which are critical for database administration and application development.
