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

To display all the information, do
```sql
TO_CHAR(<date>, 'YYYY-MM-DD HH24:MI:SS')
```
