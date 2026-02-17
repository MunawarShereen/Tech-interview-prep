## 1. SQL vs. NoSQL

* **SQL (Relational Databases / RDBMS):**
* **Structure:** Used for **tabular data** (rows and columns) with a predefined schema.
* **Usage:** Best for complex queries and transactional systems where data integrity is critical (e.g., Banking, E-commerce).
* **Examples:** MySQL, PostgreSQL, Oracle, SQL Server.
* **Scaling:** Typically scales vertically (adding more power to a single server).


* **NoSQL (Non-Relational):**
* **Structure:** Used for **unstructured or semi-structured data** (documents, key-value pairs, graphs).
* **Usage:** Best for handling massive amounts of data with flexible schemas or real-time big data (e.g., Social Media feeds, Real-time analytics).
* **Examples:** MongoDB, Cassandra, Redis.
* **Scaling:** Typically scales horizontally (adding more servers).



---

## 2. Database Relationships

How many tables are required to map relationships?

* **One-to-One (1:1):**
* **Requirement:** Usually **2 tables** (with a Foreign Key that is also Unique), though strictly speaking, they can often be merged into **1 table** depending on the design.


* **One-to-Many (1:N):**
* **Requirement:** **2 tables**. The "Many" side table will hold the Foreign Key of the "One" side.


* **Many-to-Many (M:N):**
* **Requirement:** **3 tables**.
* Two main tables (e.g., `Student`, `Courses`) and one **Junction/Join Table** (e.g., `Enrollment`) that links them together.



---

## 3. Schema Design Example: Swiggy (Food Delivery)

To design a schema, identify the core **Entities** first.

* **Entities (Tables):**
1. **Users:** (`user_id`, name, email, address, phone)
2. **Restaurants:** (`restaurant_id`, name, address, rating)
3. **FoodItems:** (`item_id`, `restaurant_id`, name, price)
4. **Orders:** (`order_id`, `user_id`, `restaurant_id`, total_amount, order_status, timestamp)
5. **OrderDetails:** (`order_detail_id`, `order_id`, `item_id`, quantity) -> *This handles the Many-to-Many relationship between Orders and FoodItems.*
6. **DeliveryPartners:** (`partner_id`, name, phone, current_location)



---

## 4. Types of Joins

Joins allow you to combine rows from two or more tables based on a related column.

**Visual Guide:**

### 1. Cross Join (Cartesian Product)

Returns all possible combinations of rows from both tables. If Table A has 10 rows and Table B has 10 rows, the result is 100 rows.

```sql
SELECT * FROM users CROSS JOIN groups;

```

### 2. Inner Join

Returns only the rows where there is a match in **both** tables. (The intersection in the Venn diagram).

```sql
SELECT * FROM users u 
INNER JOIN membership m ON u.userID = m.userID;

```

### 3. Left Join

Returns **all** rows from the Left table, and the matching rows from the Right table. If there is no match, the result is `NULL` on the right side.

```sql
SELECT * FROM users u 
LEFT JOIN membership m ON u.userID = m.userID;

```

### 4. Right Join

Returns **all** rows from the Right table, and the matching rows from the Left table.

```sql
SELECT * FROM users u 
RIGHT JOIN membership m ON u.userID = m.userID;

```

### 5. Full Outer Join

Returns all rows when there is a match in **either** the left or right table. It is essentially a Union of Left and Right joins.

```sql
-- MySQL does not have a direct FULL JOIN keyword, so we simulate it:
SELECT * FROM users u LEFT JOIN membership m ON u.userID = m.userID
UNION
SELECT * FROM users u RIGHT JOIN membership m ON u.userID = m.userID;

```

### 6. Self Join

A table is joined with **itself**. useful for hierarchical data (e.g., finding an Employee's Manager within the same Employee table).

---

## 5. UNION vs. UNION ALL

* **UNION:** Combines result sets of two queries and **removes duplicates**. It is slower because it has to sort and filter duplicates.
* **UNION ALL:** Combines result sets but **keeps all duplicates**. It is faster because it does no filtering.

---

## 6. OLTP vs. OLAP (Important Correction)

* **OLTP (Online Transaction Processing):**
* **Goal:** Day-to-day operations (live systems).
* **Characteristics:** Fast inserts/updates, highly interactive.
* **Data Structure:** Highly **Normalized** (to avoid redundancy).
* **Example:** An ATM transaction, placing an order on Amazon.


* **OLAP (Online Analytical Processing):**
* **Goal:** Analysis and reporting (Data Warehouse).
* **Characteristics:** Complex queries for historical data analysis. Read-heavy.
* **Data Structure:** Often **Denormalized** (e.g., Star Schema) to make reading data faster.
* **Example:** Generating a "Yearly Sales Report" for the CEO.


* **ETL (Extract, Transform, Load):**
* The process of moving data from an **OLTP** source, cleaning/formatting it (**Transformation**), and saving it into an **OLAP** Data Warehouse.



---

## 7. Database Keys

Keys are used to establish relationships and ensure data integrity.

1. **Primary Key:** A column that uniquely identifies every record in a table. It cannot be `NULL`. (e.g., `student_id`).
2. **Foreign Key:** A column that links to the Primary Key of another table. It enforces the link between data.
3. **Unique Key:** Ensures all values in a column are different. Unlike Primary Key, it **can** accept one `NULL` value (in most DBs).
4. **Candidate Key:** Any column (or set of columns) that *could* qualify as a Primary Key.
5. **Composite Key:** A Primary Key made by combining two or more columns.

---

## 8. Types of Normalization

Normalization is the process of organizing data to reduce redundancy (duplicates) and improve integrity.

1. **1NF (First Normal Form):** Each column must contain atomic (indivisible) values. No lists or multiple values in a single cell.
2. **2NF (Second Normal Form):** Must be in 1NF + all non-key columns must depend on the **entire** Primary Key.
3. **3NF (Third Normal Form):** Must be in 2NF + no "transitive dependency" (non-key columns should not depend on other non-key columns).

---

## 9. Indexes in Databases

### What is an Index?

Imagine you are looking for a specific chapter in a **1,000-page Science textbook**.

* **Without an Index:** You have to flip through every single page from start to finish until you find it. In databases, this is called a **Full Table Scan** (very slow).
* **With an Index:** You go to the back of the book, find the topic, look at the page number, and jump straight there. This is an **Index Scan** (very fast).

**Trade-off:** Indexes make **Reading (SELECT)** faster, but **Writing (INSERT/UPDATE)** slower.

* *Why?* Because every time you add new data, you also have to update the index at the back of the book!

---

### Types of Indexes

#### 1. Clustered Index (The "Dictionary" Analogy)

* **Concept:** A Clustered Index stores the **actual data** rows in a specific order. The data *is* the index.
* **Analogy:** Think of a **Dictionary**. The words are already sorted alphabetically (A-Z). You don't need a separate list to tell you where "Apple" is; you just go to the 'A' section, and the definition is right there.
* **Rule:** You can only have **1 Clustered Index** per table (because data can only be sorted physically in one way).
* **Example:**
* **Table:** `Users`
* **Clustered Index:** `User_ID` (Primary Key).
* **Result:** The database stores user 1, then user 2, then user 3 physically next to each other on the hard drive.



#### 2. Non-Clustered Index (The "Textbook" Analogy)

* **Concept:** A Non-Clustered Index is a **separate list** that points to where the data is stored. It does not change the order of the actual data.
* **Analogy:** Think of the **Index at the back of a Textbook**. The textbook is sorted by chapters (Logical order), but the index at the back is sorted alphabetically (e.g., "Photosynthesis: Page 45"). The index tells you *where* to go, but the actual text isn't in the back; it's on page 45.
* **Rule:** You can have **many Non-Clustered Indexes** (usually up to 999).
* **Example:**
* **Table:** `Users` (Sorted by ID).
* **Non-Clustered Index:** `Email_Address`.
* **Result:** If you search `SELECT * FROM Users WHERE email = 'munawar@gmail.com'`, the database looks at the "Email Index", finds 'munawar@gmail.com', gets the pointer to "Row 56", and jumps to Row 56 to get the data.

#### 3. Unique Index

* **Concept:** Ensures that all values in the index are different. No duplicates allowed.
* **Example:**
* **Field:** `Passport_Number` or `Phone_Number`.
* **Why:** You don't want two different people to have the same passport number. If you try to insert a duplicate, the database will throw an error.

#### 4. Composite Index (Multi-Column Index)

* **Concept:** An index created on **two or more columns** combined.
* **Analogy:** A phone book sorted first by **Last Name**, and then by **First Name**.
* **Example:**
* **Query:** `SELECT * FROM Employees WHERE Last_Name = 'Smith' AND First_Name = 'John'`.
* **Index:** Create an index on `(Last_Name, First_Name)`.
* **Note:** Order matters! If you index `(Last_Name, First_Name)`, the index helps find "Smith, John" or just "Smith", but it **won't** help you find just "John" (because the list is sorted by Last Name first).

### Summary Table

| Feature | Clustered Index | Non-Clustered Index |
| --- | --- | --- |
| **Analogy** | Dictionary / Phonebook | Textbook Index |
| **Data Storage** | Stores actual data at leaf nodes | Stores pointers to data |
| **Physical Order** | Sorts data physically on disk | Does not sort data physically |
| **Quantity** | Max **1** per table | Many per table |
| **Speed** | Faster (direct access) | Slightly slower (requires extra step) |

---

## 10. Transactions & ACID Properties

### What is a Transaction?

A transaction is a group of SQL commands that are treated as a **single unit of work**.

**Real World Example (Bank Transfer):**
Imagine you are transferring $100 from **Account A** to **Account B**. This involves two steps:

1. Subtract $100 from Account A.
2. Add $100 to Account B.

If step 1 happens but step 2 fails (e.g., power failure), the money would disappear! A **Transaction** ensures that either **both** steps happen successfully, or **neither** happens (the first step is undone).

### ACID Properties

To guarantee that your data is safe and accurate, every transaction must follow these 4 rules:

#### 1. A - Atomicity ("All or Nothing")

* **Concept:** The entire transaction must complete successfully. If any part fails, the whole thing is cancelled (Rolled Back).
* **Example:** If the system crashes after taking money from Account A but *before* adding it to Account B, the database cancels the first step. Account A gets its money back.

#### 2. C - Consistency ("Follow the Rules")

* **Concept:** The database must remain in a valid state before and after the transaction. It must follow all rules (constraints) like unique keys or data types.
* **Example:** If the database has a rule that "Balance cannot be negative," and you try to transfer $200 from an account that only has $100, the transaction will fail to maintain consistency.

#### 3. I - Isolation ("One at a Time")

* **Concept:** Multiple transactions happening at the same time should not mess each other up. They should happen as if they were waiting in line (isolated from each other).
* **Example:** If you withdraw $100 from an ATM while your friend sends you $50 online at the exact same second, the database processes them separately so your balance doesn't get calculated wrong.

#### 4. D - Durability ("Saved Forever")

* **Concept:** Once a transaction is "Committed" (saved), the change is permanent. It will survive system crashes or power failures.
* **Example:** Once the ATM screen says "Transaction Successful," your money is withdrawn. Even if the bank's server explodes 1 second later, your account balance will still show the withdrawal when the system comes back up.
