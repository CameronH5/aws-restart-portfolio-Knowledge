# Selecting Data from a Database

## Lab Overview and Objectives

This lab demonstrated how to use some common database operators and the SELECT statement.

After completing this lab, I was able to:

- Use the SELECT statement to query a database
- Use the COUNT() function
- Use the following operators to query a database:
  - < (less than)
  - > (greater than)
  - = (equals)
  - WHERE (filter conditions)
  - ORDER BY (sort results)
  - AND (combine conditions)

---

## Task 1: Connect to the Command Host

I connected to the Command Host EC2 instance using AWS Systems Manager Session Manager. After logging in, I configured the terminal and connected to the MariaDB database:

```bash
sudo su
cd /home/ec2-user/
mysql -u root --password='re:St@rt!9'
```

Once connected, I verified the available databases:

```sql
SHOW DATABASES;
```

The output confirmed that the `world` database was available, containing the sample data imported in the previous lab.

<img width="821" height="497" alt="Screenshot 2026-07-06 193851" src="https://github.com/user-attachments/assets/ee8b8bb2-da75-4069-a78b-cb87c1d0a94b" />


---

## Task 2: Query the World Database

### Step 1: Count Rows in the Country Table

I used the `COUNT()` function to determine the number of rows in the `country` table:

```sql
SELECT COUNT(*) FROM world.country;
```

The query returned **239 rows**, confirming that the table was populated with the sample dataset.

<img width="627" height="167" alt="Screenshot 2026-07-06 193917" src="https://github.com/user-attachments/assets/42f922c7-a4c1-4219-bb1e-26fb714f44de" />


---

### Step 2: View Table Structure

I used `SHOW COLUMNS` to understand the table schema and identify available columns for querying:

```sql
SHOW COLUMNS FROM world.country;
```

The output displayed 14 columns with their data types, nullability, and key information. I noticed that some data types had changed from the previous lab (e.g., `SurfaceArea` was now `decimal(10,2)` instead of `float`).

<img width="1407" height="416" alt="Screenshot 2026-07-06 193933" src="https://github.com/user-attachments/assets/521e8c90-590c-4981-ab02-f912cd9588bc" />


---

### Step 3: Query Specific Columns

I queried a subset of columns from the `country` table:

```sql
SELECT Name, Capital, Region, SurfaceArea, Population FROM world.country;
```

The query returned all rows with only the specified columns, making the output more readable and focused.

<img width="1242" height="462" alt="Screenshot 2026-07-06 193950" src="https://github.com/user-attachments/assets/d42ccf49-1dd6-4bcc-8d93-9b7a857a42a1" />


---

### Step 4: Use Column Aliases with AS

To make column names more user-friendly in the output, I used the `AS` keyword to rename the `SurfaceArea` column:

```sql
SELECT Name, Capital, Region, SurfaceArea AS "Surface Area", Population FROM world.country;
```

The `SurfaceArea` column was now displayed as "Surface Area" in the result set, improving readability.

<img width="1296" height="428" alt="Screenshot 2026-07-06 194056" src="https://github.com/user-attachments/assets/4de6cda9-2801-4fe8-ad02-a9382ff43d55" />


---

### Step 5: Order Results in Ascending Order

I used `ORDER BY` to sort the results by `Population` in ascending order (lowest to highest):

```sql
SELECT Name, Capital, Region, SurfaceArea AS "Surface Area", Population 
FROM world.country 
ORDER BY Population;
```

The results showed countries with the smallest populations first, including territories with 0 population such as Antarctica and French Southern territories.

<img width="1352" height="441" alt="Screenshot 2026-07-06 194043" src="https://github.com/user-attachments/assets/82651f51-512b-424c-81df-190003bbc714" />


---

### Step 6: Order Results in Descending Order

I added the `DESC` option to sort by `Population` in descending order (highest to lowest):

```sql
SELECT Name, Capital, Region, SurfaceArea AS "Surface Area", Population 
FROM world.country 
ORDER BY Population DESC;
```

The results displayed the most populous countries first, with China (1,277,558,000) and India (1,013,662,000) at the top.

<img width="1416" height="341" alt="Screenshot 2026-07-06 194126" src="https://github.com/user-attachments/assets/0602de6f-86ed-44c1-8c7b-175a0e7fe5a4" />


---

### Step 7: Filter with WHERE and Greater Than (>) Operator

I added a `WHERE` condition to filter for countries with a population greater than 50,000,000:

```sql
SELECT Name, Capital, Region, SurfaceArea AS "Surface Area", Population 
FROM world.country 
WHERE Population > 50000000 
ORDER BY Population DESC;
```

The query returned **24 rows** of countries with populations exceeding 50 million, sorted from largest to smallest.

<img width="1661" height="622" alt="Screenshot 2026-07-06 194145" src="https://github.com/user-attachments/assets/23030745-9d48-49cc-b546-ea6e7c171fa5" />


---

### Step 8: Filter with Multiple Conditions Using AND

I combined two conditions using the `AND` operator to find countries with a population between 50 million and 100 million:

```sql
SELECT Name, Capital, Region, SurfaceArea AS "Surface Area", Population 
FROM world.country 
WHERE Population > 50000000 AND Population < 100000000 
ORDER BY Population DESC;
```

The query returned **13 rows** of countries with populations in this range, including Mexico, Germany, Vietnam, and the Philippines.

<img width="1893" height="455" alt="Screenshot 2026-07-06 194204" src="https://github.com/user-attachments/assets/3d8d1f00-8962-4db6-af59-9949d5952c0e" />


---

## Summary

In this lab, I successfully:

1. **Connected** to the Command Host and authenticated to the MariaDB database.
2. **Counted rows** in the `country` table using `COUNT(*)`, confirming 239 rows.
3. **Inspected the table schema** using `SHOW COLUMNS` to understand available columns.
4. **Queried specific columns** to return a focused result set.
5. **Used column aliases** with the `AS` keyword to improve output readability.
6. **Sorted results** in both ascending (`ORDER BY`) and descending (`ORDER BY ... DESC`) order.
7. **Applied filters** using the `WHERE` clause with the greater than (`>`) operator.
8. **Combined multiple conditions** using the `AND` operator to filter population ranges.

These operations demonstrate essential data retrieval skills in SQL, including filtering, sorting, and formatting query results—fundamental techniques for database analysis and reporting.
