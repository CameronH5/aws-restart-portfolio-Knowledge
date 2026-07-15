# Performing a Conditional Search

## Lab Overview and Objectives

This lab demonstrated how to use the SELECT statement and a WHERE clause to filter records with a conditional search.

After completing this lab, I was able to:

- Write a search condition by using the WHERE clause
- Use the BETWEEN operator
- Use the LIKE operator with wildcard characters
- Use the AS operator to create a column alias
- Use functions in a SELECT statement
- Use functions in a WHERE clause

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

The output confirmed that the `world` database was available, containing the sample data imported in previous labs.

<img width="807" height="431" alt="Screenshot 2026-07-06 195727" src="https://github.com/user-attachments/assets/f397fb9f-67e8-4f50-8e5a-d1d88b3c3ca7" />
<img width="1421" height="412" alt="Screenshot 2026-07-06 195737" src="https://github.com/user-attachments/assets/a2872837-9db3-45bd-a258-8501f8c31fea" />


---

## Task 2: Query the World Database with Conditional Searches

### Step 1: Filter with >= and <= Operators Using AND

I used the `WHERE` clause with the `AND` operator to find countries with populations between 50 million and 100 million:

```sql
SELECT Name, Capital, Region, SurfaceArea, Population 
FROM world.country 
WHERE Population >= 50000000 AND Population <= 100000000;
```

The query returned **14 rows** of countries with populations in this range.

<img width="1492" height="427" alt="Screenshot 2026-07-06 195807" src="https://github.com/user-attachments/assets/04800d7d-937a-4938-a075-fa80e9975219" />


---

### Step 2: Use the BETWEEN Operator

I rewrote the same query using the `BETWEEN` operator, which is more readable and inclusive:

```sql
SELECT Name, Capital, Region, SurfaceArea, Population 
FROM world.country 
WHERE Population BETWEEN 50000000 AND 100000000;
```

The result was identical—**14 rows**—demonstrating that `BETWEEN` is equivalent to using `>=` and `<=` together.

<img width="1410" height="418" alt="Screenshot 2026-07-06 195823" src="https://github.com/user-attachments/assets/cf679363-0c75-4dc5-95bb-d1a424d3a03e" />


---

### Step 3: Use the LIKE Operator with Wildcards

I used the `LIKE` operator with the `%` wildcard to find all regions containing the word "Europe" and calculated the total population:

```sql
SELECT SUM(Population) FROM world.country WHERE Region LIKE "%Europe%";
```

The query returned a total population of **634,947,800** for all European countries.

<img width="902" height="191" alt="Screenshot 2026-07-06 195844" src="https://github.com/user-attachments/assets/9b32be7d-f85c-4e12-9992-8082a9588be2" />


---

### Step 4: Use Column Aliases with AS

I added a column alias using the `AS` keyword to make the output more readable:

```sql
SELECT SUM(Population) AS "Europe Population Total" 
FROM world.country 
WHERE Region LIKE "%Europe%";
```

The output now displayed the column header as "Europe Population Total" instead of the default function name.

<img width="1206" height="157" alt="Screenshot 2026-07-06 195903" src="https://github.com/user-attachments/assets/b77cad50-28aa-4aad-8ca0-16d5337ce430" />


---

### Step 5: Calculate Multiple Aggregate Values

I calculated both the total surface area and total population for North America:

```sql
SELECT SUM(SurfaceArea) AS "N. America Surface Area", 
       SUM(Population) AS "N. America Population" 
FROM world.country 
WHERE Region = "North America";
```

The query returned:
- **North America Surface Area:** 21,500,515.00
- **North America Population:** 309,632,000

This demonstrated using multiple aggregate functions and aliases in a single query.

<img width="1677" height="165" alt="Screenshot 2026-07-06 195948" src="https://github.com/user-attachments/assets/3d14bbc0-8fba-4f8c-90df-3c9de14c266d" />


---

### Step 6: Use the LOWER Function in a WHERE Clause

I performed a case-insensitive search by using the `LOWER()` function to convert the `Region` column to lowercase before comparing:

```sql
SELECT Name, Capital, Region, SurfaceArea, Population 
FROM world.country 
WHERE LOWER(Region) LIKE "%central%";
```

This query returned all countries where the region contained the word "central" regardless of case, demonstrating how to handle case sensitivity in database searches.

<img width="1293" height="762" alt="Screenshot 2026-07-06 195921" src="https://github.com/user-attachments/assets/1091cf74-3bbc-41bc-965c-e674517a53e6" />


---

## Summary

In this lab, I successfully:

1. **Connected** to the Command Host and authenticated to the MariaDB database.

2. **Filtered data using the WHERE clause** with:
   - Comparison operators (`>=` and `<=`) combined with `AND`
   - The `BETWEEN` operator as a more readable alternative for range queries
   - The `LIKE` operator with `%` wildcards for pattern matching

3. **Used aggregate functions** including:
   - `SUM()` to calculate total population and surface area
   - Multiple aggregate functions in a single query

4. **Applied column aliases** with the `AS` keyword to improve output readability.

5. **Used functions in WHERE clauses** with:
   - `LOWER()` to perform case-insensitive pattern matching

These operations demonstrate essential conditional search techniques in SQL, including range filtering, pattern matching, aggregation, and handling case sensitivity—fundamental skills for querying and analyzing database information effectively.
