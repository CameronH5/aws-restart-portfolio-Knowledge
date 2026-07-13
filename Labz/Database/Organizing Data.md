

# Organizing Data

## Lab Overview and Objectives
This lab demonstrated how to use common database functions with the `GROUP BY` and `OVER` clauses. After completing this task, I was able to:
* Use the `GROUP BY` clause with the aggregate function `SUM()`.
* Use the `OVER` clause with the `RANK()` window function.
* Use the `OVER` clause with the aggregate function `SUM()` and the `RANK()` window function.

---

## Task 2: Query the World Database

Below is a step-by-step breakdown of the actions I took in the terminal to query the `world` database.

### Step 12: Show existing databases
**Action:** I logged into the MariaDB server using the root credentials.
<img width="805" height="292" alt="Screenshot 2026-07-13 182112" src="https://github.com/user-attachments/assets/4ef70ea2-c4a0-4af2-8890-888bea1622f5" />

**Query Executed:** I entered the `SHOW DATABASES;` command to verify the availability of the `world` database.
```sql
SHOW DATABASES;
```
**Result:** The terminal output displayed the `world` database alongside other default system databases.
<img width="438" height="263" alt="Screenshot 2026-07-13 182128" src="https://github.com/user-attachments/assets/1e13f517-3fff-4544-9d1c-ded4aeaca47b" />


### Step 13: Review the table schema and data
**Action:** I verified the structure and contents of the `country` table within the `world` database.
**Query Executed:** I used a `SELECT *` statement to return all records.
```sql
SELECT * FROM world.country;
```
**Result:** The terminal printed the first few rows of the table, displaying columns such as `Code`, `Name`, `Continent`, `Region`, `Population`, etc.
<img width="1902" height="443" alt="Screenshot 2026-07-13 182154" src="https://github.com/user-attachments/assets/92a9051f-8fb2-4208-8aa3-ca09dcf3f340" />


### Step 14: Filter, select, and sort records
**Action:** I filtered the data to retrieve specific columns (`Region`, `Name`, `Population`) only for the region 'Australia and New Zealand'. I then used an `ORDER BY` clause to sort the results by population in descending order.
**Query Executed:**
```sql
SELECT Region, Name, Population FROM world.country WHERE Region = 'Australia and New Zealand' ORDER BY Population desc;
```
**Result:** The query successfully returned the 5 countries in that region, with Australia (18,886,000) at the top and Cocos (Keeling) Islands (600) at the bottom.
<img width="1385" height="251" alt="Screenshot 2026-07-13 182233" src="https://github.com/user-attachments/assets/d118f318-6085-4643-91a3-48a3d6b0e4a8" />


### Step 15: Use the `GROUP BY` clause with an aggregate function
**Action:** To calculate the total population for the entire region, I grouped the data by `Region` and applied the `SUM()` aggregate function to the `Population` column. I also added an `ORDER BY` clause to sort by the total population.
**Query Executed:**
```sql
SELECT Region, SUM(Population) FROM world.country WHERE Region = 'Australia and New Zealand' GROUP BY Region ORDER BY SUM(Population) desc;
```
**Result:** The query returned a single row showing the cumulative population sum for the region, which was `22753100`.
<img width="1602" height="173" alt="Screenshot 2026-07-13 182253" src="https://github.com/user-attachments/assets/686415fb-f917-4bdf-be82-2ec0f2e5bea7" />


### Step 16: Use the `OVER` clause to create a running total
**Action:** I used a window function with the `OVER` clause to display a running total of the population. By using `PARTITION BY Region` and `ORDER BY Population`, the query generated a cumulative sum that added the population of each record to the sum of the previous records in the region.
**Query Executed:**
```sql
SELECT Region, Name, Population, SUM(Population) OVER(partition by Region ORDER BY Population) as 'Running Total' FROM world.country WHERE Region = 'Australia and New Zealand';
```
**Result:** The output displayed each country alongside its own population and a running cumulative total. The final row showed the total sum of `22753100`.
<img width="1898" height="248" alt="Screenshot 2026-07-13 182327" src="https://github.com/user-attachments/assets/2ea168ef-b116-471d-9309-b692a9ddc113" />


### Step 17: Use the `RANK()` window function
**Action:** I expanded on the previous query by adding the `RANK()` window function. This generated a rank number for each record based on the `Population`, which is useful for identifying the order of records within large groups. I kept the `Running Total` in the query as well.
**Query Executed:**
```sql
SELECT Region, Name, Population, SUM(Population) OVER(partition by Region ORDER BY Population) as 'Running Total', RANK() over(partition by region ORDER BY population) as 'Ranked' FROM world.country WHERE region = 'Australia and New Zealand';
```
**Result:** The table now displayed a `Ranked` column, with Cocos (Keeling) Islands ranked 1 (lowest population) and Australia ranked 5 (highest population).
<img width="1890" height="250" alt="Screenshot 2026-07-13 182359" src="https://github.com/user-attachments/assets/a2ac51c1-953d-45db-bd39-d290e37c182f" />


### Challenge: Rank countries within each region
**Action:** To complete the challenge of ranking countries by population from largest to smallest for *every* region in the table, I removed the `WHERE` clause and adjusted the `RANK()` window function to order by `Population desc`. I also added an overall `ORDER BY` clause to sort the final result by `Region` and then by `Ranked`.
**Query Executed:**
```sql
SELECT Region, Name, Population, RANK() OVER(partition by Region ORDER BY Population desc) as 'Ranked' FROM world.country ORDER BY Region, Ranked;
```
**Result:** The terminal returned a comprehensive list of all regions, showing the ranking of each country within its respective region. For example, in the "Australia and New Zealand" region, Australia was ranked 1 and Cocos (Keeling) Islands was ranked 5. The results displayed regions like Antarctica, Baltic Countries, British Islands, and Caribbean sequentially.
<img width="1643" height="525" alt="Screenshot 2026-07-13 182427" src="https://github.com/user-attachments/assets/fd2a7042-2baf-4cbb-bfae-eb8105b0ea84" />
