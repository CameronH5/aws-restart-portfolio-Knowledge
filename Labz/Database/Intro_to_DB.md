

# Introduction to Amazon DynamoDB

In this lab, I performed a hands-on walkthrough to get started with Amazon DynamoDB. The topics covered included creating a table, adding data, querying the data, modifying an item, and finally deleting the table.

## Task 1: Create an Amazon DynamoDB table

I started by navigating to the Amazon DynamoDB service in the AWS Management Console. I initiated the creation of a new table by clicking the **Create table** button. 

To uniquely identify each entry in the table, I defined a primary key composed of two parts:
*   **Partition key:** I entered `Artist` and selected the `String` data type.
*   **Sort key - optional:** I entered `Song` and selected the `String` data type.

I then scrolled down and selected **Create table**. A green notification appeared confirming that the table was successfully created. I waited a brief moment for the table's status to change to **Active** before proceeding to the next task.

<img width="1375" height="762" alt="Screenshot 2026-07-14 145048" src="https://github.com/user-attachments/assets/1065c14d-e347-4707-ae40-1bfe7986cf1a" />

<img width="1893" height="377" alt="Screenshot 2026-07-14 145119" src="https://github.com/user-attachments/assets/715190a4-07f7-42ad-a476-4ddeee1e3eea" />


## Task 2: Enter data into an Amazon DynamoDB table

In this task, I added three unique items to my `Music` table. A key thing I noted was that DynamoDB is schemaless—meaning while the partition and sort keys are required, I could add different optional attributes to each item without having to pre-define a schema.

**First Item:**
I clicked on the `Music` table, chose **Actions**, and then selected **Create item**. 
1.  For the required fields, I entered `Pink Floyd` as the **Artist** and `Money` as the **Song**.
2.  I clicked **Add new attribute** twice to include additional details. First, I added a `String` attribute named `Album` with the value `The Dark Side of the Moon`. Then I added a `Number` attribute named `Year` with the value `1973`.
3.  I clicked **Create item** to save it to the database.

<img width="1686" height="453" alt="Screenshot 2026-07-14 145222" src="https://github.com/user-attachments/assets/98481628-1c83-444c-ad1e-ee83254d150a" />
<img width="1691" height="547" alt="Screenshot 2026-07-14 145358" src="https://github.com/user-attachments/assets/1056b7ff-6bb6-414f-9ac1-c6d45d5d2bfb" />


**Second and Third Items:**
I repeated the process to populate the table with two more items:
*   **John Lennon - Imagine:** Included attributes `Album` (String: `Imagine`), `Year` (Number: `1971`), and an additional attribute `Genre` (String: `Soft rock`).
*   **Psy - Gangnam Style:** Included attributes `Album` (String: `Psy 6 (Six Rules), Part 1`), `Year` (Number: `2011`), and a new attribute not present in previous items, `LengthSeconds` (Number: `219`).

This successfully demonstrated the flexibility of a NoSQL database, as each item contained a different set of attributes.

<img width="1222" height="215" alt="Screenshot 2026-07-14 145802" src="https://github.com/user-attachments/assets/a1165f90-7e68-4032-9d68-0e3dbd214b13" />


## Task 3: Modify an existing item

After creating my data, I noticed an error in the third item. The year for the song "Gangnam Style" by Psy was incorrectly listed as `2011`. 

To fix this, I navigated to the DynamoDB dashboard, selected **Tables**, and chose **Explore Items**. I selected the `Music` table, located the `Psy` entry, and clicked into it. I changed the `Year` attribute from `2011` to `2012` and clicked **Save changes**. The item was instantly updated in the table list.

[Image: Screenshot showing the updated table view with Psy's Year changed to 2012]

## Task 4: Query an Amazon DynamoDB table

Next, I learned about the two primary methods of retrieving data: `Query` and `Scan`.

**Using a Query:**
I started by performing a Query, which is the most efficient way to retrieve data since it uses the primary key. 
1.  In the "Scan or query items" section, I selected **Query**.
2.  For the partition key, I entered `Psy`, and for the sort key, I entered `Gangnam Style`.
3.  I clicked **Run**. The result appeared instantly, returning the exact one item I was looking for. This showed how fast indexed lookups are in DynamoDB.

<img width="1551" height="623" alt="Screenshot 2026-07-14 150000" src="https://github.com/user-attachments/assets/84b0b56a-aa7f-43fc-8eda-d19143af533b" />

<img width="1241" height="248" alt="Screenshot 2026-07-14 150013" src="https://github.com/user-attachments/assets/301602ce-b31b-427f-a046-3ca4ec4bf43d" />


**Using a Scan:**
Afterwards, I performed a Scan. Unlike a query, a scan examines every item in the table, making it less efficient for large datasets. 
1.  I switched the radio button from Query to **Scan**.
2.  I expanded the **Filters** section.
3.  I added a filter to look for an attribute named `Year` of type `Number` with a value of `1971`.
4.  I clicked **Run**. The result successfully filtered the scanned items and displayed only the "Imagine" song by John Lennon.

<img width="1481" height="567" alt="Screenshot 2026-07-14 150126" src="https://github.com/user-attachments/assets/a3a81d39-b8f9-446f-baf8-1f84195c2b73" />

<img width="1205" height="240" alt="Screenshot 2026-07-14 150137" src="https://github.com/user-attachments/assets/eb9eecbb-ca8c-4d72-83ab-d7eeb8928ed7" />


## Task 5: Delete an Amazon DynamoDB table

Finally, I cleaned up my resources. In the DynamoDB dashboard, under **Tables**, I selected **Update settings** and chose the `Music` table. I clicked **Actions**, then selected **Delete table**. To ensure I truly wanted to delete the data, I typed `delete` into the confirmation prompt and clicked the **Delete table** button. A green notification appeared, confirming the deletion request had been submitted, and the table's status updated to **Deleting**.

<img width="1587" height="292" alt="Screenshot 2026-07-14 150232" src="https://github.com/user-attachments/assets/08fb3758-a902-4aca-98b7-32a435ba1ba7" />
