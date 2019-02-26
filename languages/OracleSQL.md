# Oracle SQL

# Limiting results

```sql
SELECT * FROM (
  SELECT * FROM my_table
  ORDER BY my_column
) WHERE rownum < 10;
```

# Types

## Date and time

`DATE` type contains info for date and time.

To display all the information, do:
```sql
TO_CHAR(<date>, 'YYYY-MM-DD HH24:MI:SS')
```

To truncate to just the date:
```sql
trunc(date)
```

```sql
SELECT * FROM my_table WHERE trunc(my_date_column) = TO_DATE('01/01/2018', 'DD/MM/YYYY');
```

To filter records between two dates (inclusive):
```sql
SELECT *
FROM my_table
WHERE my_date BETWEEN TO_DATE('01/01/2018', 'DD/MM/YYYY') AND TO_DATE('31/12/2018', 'DD/MM/YYYY');
```

# Joins

```sql
SELECT * from my_table mt
LEFT JOIN other_table ot on mt.id = ot.mid;
```

