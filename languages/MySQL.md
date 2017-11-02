# MySQL

## Correct settings for new tables

- Engine: InnoDB
- Charset utf-8 unicode
- Collation: utf8_general_ci

## Create table
```sql
CREATE TABLE table_name (
     table_id INT,
     name VARCHAR(255) NOT NULL,
     last_name varchar(255),
     address text,
     PRIMARY KEY(table_id)
) ENGINE=InnoDB;
```

## Retrieve data

```sql
SELECT *
FROM table
WHERE id = 1;
```

## Insert data

```sql
INSERT INTO table (name, age) VALUES ('Miguel', 18), ('Jorge', 20);
```

## Update data

```sql
UPDATE table SET age = age + 1 WHERE age = 18;
```

## Delete data

Remember to instead use active/inactive flags when possible

```sql
DELETE FROM table WHERE id = 1;
```

## Select from a table to insert into another

```sql
INSERT INTO table2 (col1, col2) SELECT col1, col2 FROM table1 WHERE id = 1
```

## Insert record or update it if it already exists

Solution:

1. Use `REPLACE INTO` instead of `INSERT INTO` and set your unique field as `UNIQUE`.  
   Problem: it actually deletes the old row before inserting a new one, which doesn't play well with Foreign Keys.

2. Use `INSERT ... ON DUPLICATE KEY UPDATE`
   If we don't decide the keys, we simply have to query the DB for the current key, if any.  
   If key doesn't exist, it returns `NULL` and will `INSERT` using `AutoIncrement`.  
   The double `SELECT` in the inner query is needed to create a temporary table so MySQL allows the use of the same table in the `INSERT`/`UPDATE` and the `FROM` clause.

```sql
INSERT INTO ProductTestOpinion (idProductTestOpinion, idParticipation, rating)
VALUES (
    (SELECT idProductTestOpinion
     FROM (SELECT idProductTestOpinion
            FROM ProductTestOpinion
            WHERE idParticipation = 54) IQ),
     54, 5)
ON DUPLICATE KEY UPDATE
rating = VALUES(rating);
```

## Select X max per group
Without variables:
```sql
SELECT t1.*
FROM Table AS t1
LEFT OUTER JOIN Table AS t2
  ON t1.GroupId = t2.GroupId AND t1.OrderField < t2.OrderField
WHERE t2.GroupId IS NULL

SELECT f.type, f.variety, f.price
FROM (
   SELECT type, min(price) AS minprice
   FROM fruits GROUP BY type
) AS x INNER JOIN fruits AS f ON f.type = x.type AND f.price = x.minprice;
```

With variables (MySQL):
```sql
SET @num := 0, @type := '';

SELECT type, variety, price
FROM (
   SELECT type, variety, price,
      @num := IF(@type = type, @num + 1, 1) AS row_number,
      @type := type AS dummy
  FROM fruits
  ORDER BY type, price
) AS x WHERE x.row_number <= 2;
```
https://stackoverflow.com/questions/8748986/get-records-with-highest-smallest-whatever-per-group
http://www.xaprb.com/blog/2006/12/07/how-to-select-the-firstleastmax-row-per-group-in-sql/

## Read-modify-write

Precisam de `SELECT ... FOR UPDATE`  
Porque o default isolation level não é `SERIALIZABLE`  
http://mysqldump.azundris.com/archives/77-Transactions-An-InnoDB-Tutorial.html
