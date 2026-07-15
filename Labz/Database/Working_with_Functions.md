# Working with Functions

## Lab Overview and Objectives

This lab demonstrated how to use some common database functions with the SELECT statement and WHERE clause.

After completing this lab, I was able to:

- Use aggregate functions SUM(), MIN(), MAX(), and AVG() to summarize data
- Use the SUBSTRING_INDEX() function to split strings
- Use the LENGTH() and TRIM() functions to determine the length of a string
- Use the DISTINCT() function to filter duplicate records
- Use functions in the SELECT statement and WHERE clause

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

The output confirmed that the `world` database was available, containing the sample data from previous labs.

---

## Task 2: Query the World Database with Functions

### Step 1: Use Aggregate Functions

I used multiple aggregate functions in a single query to summarize the population data from all 239 records in the `country` table:

```sql
SELECT SUM(Population), AVG(Population), MAX(Population), MIN(Population), COUNT(Population) 
FROM world.country;
```

The query returned:
- **SUM(Population):** 6,078,749,450 (total population of all countries)
- **AVG(Population):** 25,434,098.1172 (average population per country)
- **MAX(Population):** 1,277,558,000 (largest population - China)
- **MIN(Population):** 0 (smallest population - territories with 0 population)
- **COUNT(Population):** 239 (number of rows with a population value)

<img width="1371" height="180" alt="Screenshot 2026-07-06 201452" src="https://github.com/user-attachments/assets/47faa887-aab0-474d-94fa-ba035693c289" />


---

### Step 2: Use SUBSTRING_INDEX() to Split Strings

I used the `SUBSTRING_INDEX()` function to split region names at the first space character, returning only the first word of each region:

```sql
SELECT Region, SUBSTRING_INDEX(Region, " ", 1) FROM world.country;
```

The query returned two columns: the full region name and the first word extracted from it. For example:
- "Southern and Central Asia" → "Southern"
- "Central Africa" → "Central"
- "Caribbean" → "Caribbean"
- "Australia and New Zealand" → "Australia"

<img width="912" height="493" alt="Screenshot 2026-07-06 201520" src="https://github.com/user-attachments/assets/6d97eb33-138d-47af-b3e5-44a88f618c33" />


---

### Step 3: Use SUBSTRING_INDEX() in a WHERE Clause

I used `SUBSTRING_INDEX()` as part of a condition in the `WHERE` clause to filter records where the first word of the region name is "Southern":

```sql
SELECT Name, Region 
FROM world.country 
WHERE SUBSTRING_INDEX(Region, " ", 1) = "Southern";
```

The query returned **34 rows** of countries whose region begins with "Southern", including:
- Afghanistan (Southern and Central Asia)
- Albania (Southern Europe)
- India (Southern and Central Asia)
- South Africa (Southern Africa)
- Italy (Southern Europe)

<img width="1103" height="822" alt="Screenshot 2026-07-06 201538" src="https://github.com/user-attachments/assets/6caa6ff2-4003-4762-a6c5-8b2074cf5af4" />


---

```sql
SELECT REGION 
FROM world.country 
WHERE LENGTH(TRIM(Region)) < 10;
```

<img width="863" height="747" alt="Screenshot 2026-07-06 201559" src="https://github.com/user-attachments/assets/8ab24c15-c717-42fe-82dc-361db18f4dc8" />


### Step 4: Use LENGTH(), TRIM(), and DISTINCT()

I used `LENGTH()` and `TRIM()` together to find region names with fewer than 10 characters after removing leading/trailing spaces. I then used `DISTINCT()` to remove duplicate entries:

```sql
SELECT DISTINCT(Region) 
FROM world.country 
WHERE LENGTH(TRIM(Region)) < 10;
```

The query returned **3 distinct regions**:
- Caribbean
- Polynesia
- Melanesia

This demonstrated how `TRIM()` removes whitespace, `LENGTH()` counts characters, and `DISTINCT()` eliminates duplicates from the result set.


<img width="987" height="206" alt="Screenshot 2026-07-06 201617" src="https://github.com/user-attachments/assets/e5dd62bd-b3c1-47d0-8571-bfe61aa0f360" />


---

### Step 5: Multiple SUBSTRING_INDEX() with Aliases

I used `SUBSTRING_INDEX()` twice with different delimiters to split a region name into parts, using aliases to make the output more readable:

```sql
SELECT Name, 
       SUBSTRING_INDEX(Region, "/", 1) AS "Region Name 1",
       SUBSTRING_INDEX(Region, "/", -1) AS "Region Name 2" 
FROM world.country 
WHERE Region = "Micronesia/Caribbean";
```

The query returned:
- **Name:** United States Minor Outlying Islands
- **Region Name 1:** Micronesia (first part before "/")
- **Region Name 2:** Caribbean (last part after "/")

This demonstrated splitting a string using a different delimiter ("/" instead of a space) and retrieving both the first and last parts with positive and negative index values.

<img width="1896" height="183" alt="Screenshot 2026-07-06 201635" src="https://github.com/user-attachments/assets/1a84d525-7655-4b92-8803-783141302f55" />



---

## Summary

In this lab, I successfully:

1. **Connected** to the Command Host and authenticated to the MariaDB database.

2. **Used aggregate functions** to summarize data:
   - `SUM()` to calculate total population
   - `AVG()` to find average population
   - `MAX()` and `MIN()` to find highest and lowest values
   - `COUNT()` to count records

3. **Used string manipulation functions**:
   - `SUBSTRING_INDEX()` to split strings at a specified delimiter
   - Used `SUBSTRING_INDEX()` in both the SELECT clause and WHERE clause
   - Split strings using different delimiters (space and slash)

4. **Used text processing functions**:
   - `TRIM()` to remove leading/trailing whitespace
   - `LENGTH()` to determine string length

5. **Used `DISTINCT()`** to eliminate duplicate records from query results.

6. **Applied column aliases** with `AS` to improve output readability.

These operations demonstrate essential SQL function capabilities for data summarization, string manipulation, and text processing—critical skills for analyzing and transforming database information effectively.
