# SQL Technical Interview Questions & Answers

## 1. How to insert multiple rows in a table?

To add multiple records in a single query, list the value sets separated by commas after the `VALUES` clause.

**Query:**

```sql
INSERT INTO users (name, age) 
VALUES ('Ali', 25), ('Sara', 22), ('Ahmed', 30);

```

## 2. How to add or delete multiple columns in a table?

You can modify the table structure using the `ALTER TABLE` command.

**To Add Multiple Columns:**

```sql
ALTER TABLE users 
ADD email VARCHAR(255) NOT NULL, 
ADD password VARCHAR(255) NOT NULL;

```

**To Delete (Drop) Multiple Columns:**

```sql
ALTER TABLE users 
DROP COLUMN email, 
DROP COLUMN password;

```

## 3. How to find the 2nd highest value in a table?

You can use the `LIMIT` clause with an offset. The syntax is `LIMIT offset, count`.

**Query:**

```sql
SELECT * FROM movies 
ORDER BY score DESC 
LIMIT 1, 1;

```

**Explanation:**

* `LIMIT 0, 5`: Starts from the 1st row and picks 5 records.
* `LIMIT 1, 1`: Skips the 1st row (highest) and picks the next 1 record (2nd highest).

## 4. How to find the maximum value without using ORDER BY?

Using `ORDER BY` on very large datasets can be computationally expensive because it involves sorting. Instead, use the aggregate function `MAX()`.

**Query:**

```sql
SELECT name, budget 
FROM movies 
WHERE budget = (SELECT MAX(budget) FROM movies);

```

*Note: If the column is stored as `VARCHAR`, you must cast/convert it to an integer first for accurate numerical comparison.*

## 5. How to find the manager of every employee (Self Join)?

To link an employee to their manager within the same table, use a **Self Join**.

**Query:**

```sql
SELECT e1.fname AS Employee, e2.fname AS Manager 
FROM employees e1 
JOIN employees e2 ON e1.managerID = e2.id;

```

## 6. What are Wildcards and how to use them?

Wildcards are special characters used with the `LIKE` operator to search for specific patterns in text.

* `%` (Percent): Represents zero, one, or multiple characters.
* `_` (Underscore): Represents exactly one single character.

**Examples:**

* **Find exact length match:** Finds names starting with 'M' followed by exactly 6 characters:
```sql
SELECT * FROM movies WHERE name LIKE 'M______';

```


* **Find text containing a word:** Finds names containing "man" anywhere:
```sql
SELECT * FROM movies WHERE name LIKE '%man%';

```


* **Find text ending with a word:** Finds names ending with "man":
```sql
SELECT * FROM movies WHERE name LIKE '%man';

```



## 7. How to use IF conditions (CASE Statements) in SQL?

SQL uses the `CASE` statement to handle if-else logic.

**Query (Categorizing Titanic passengers by family size):**

```sql
SELECT name, (sibsp + parch) AS family_size,
CASE
    WHEN (sibsp + parch) <= 2 THEN 'Small'
    WHEN (sibsp + parch) > 2 AND (sibsp + parch) <= 5 THEN 'Medium'
    ELSE 'Large'
END AS family_type
FROM titanic;

```

## 8. How to find the top value per category (Correlated Subquery)?

To find the highest-rated movie in *each* genre, we use a correlated subquery. The inner query depends on the outer query's current row (`m1.genre`).

**Query:**

```sql
SELECT genre, name, score 
FROM movies m1 
WHERE score = (
    SELECT MAX(score) 
    FROM movies m2 
    WHERE m2.genre = m1.genre
);

```

**Explanation:**
This is a **Correlated Subquery** because the inner query refers to the outer table (`m1`). It executes once for every row in the outer query, unlike a standard subquery which runs only once.

## 9. How to find and delete duplicate rows?

To handle duplicates, we identify unique rows using `GROUP BY` and `MIN(id)` (keeping the first occurrence) and target the rest.

**To Find Duplicates (Rows to be removed):**

```sql
SELECT * FROM contacts 
WHERE id NOT IN (
    SELECT MIN(id) 
    FROM contacts 
    GROUP BY first_name, last_name, email
);

```

**To Delete Duplicates:**

```sql
DELETE FROM contacts 
WHERE id NOT IN (
    SELECT MIN(id) 
    FROM contacts 
    GROUP BY first_name, last_name, email
);

```
