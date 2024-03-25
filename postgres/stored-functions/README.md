- [Certain operations are too complex to perform directly using the client libraries.](#certain-operations-are-too-complex-to-perform-directly-using-the-client-libraries)

### Certain operations are too complex to perform directly using the client libraries.

1. Go to the SQL query editor on your database dashboard.
2. Run the following SQL script to create a stored function tailored to your specific complex query:

```sql
DROP FUNCTION IF EXISTS get_my_complex_query;
CREATE FUNCTION get_my_complex_query(parameter INT)
RETURNS TABLE (column1 INTEGER, column2 VARCHAR, column3 DATE) AS
$$
BEGIN
    RETURN QUERY
    SELECT t1.column1, t1.column2, t2.column3
    FROM "TableName1" AS t1
    INNER JOIN "TableName2" AS t2 ON t1.column = t2.column
    INNER JOIN "TableName3" AS t3 ON t2.another_column = t3.another_column
    LEFT JOIN "TableName4" AS t4 ON t3.some_column = t4.some_column
    WHERE t2.column = parameter
    AND t3.column_name = 'some_value';
END;
$$
LANGUAGE plpgsql VOLATILE;
```

https://github.com/orgs/supabase/discussions/21294
