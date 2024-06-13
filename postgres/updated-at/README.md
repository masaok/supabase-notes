- [Update `updated_at` only when row data actually changes](#update-updated_at-only-when-row-data-actually-changes)

### Update `updated_at` only when row data actually changes

To modify the SQL to update the `updated_at` column only when something in the row actually changes, you need to add a condition to check if the new row values differ from the old row values. Here's how you can do it:

1. First, create a function that updates the `updated_at` column only if there is a change.
2. Then, create a trigger that calls this function before an update.

Here’s the SQL code to achieve this:

```sql
-- Create the function
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
  -- Check if any column value is different from its old value
  IF (NEW.* IS DISTINCT FROM OLD.*) THEN
    NEW.updated_at = NOW();
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Create the trigger
CREATE TRIGGER handle_updated_at
BEFORE UPDATE ON YOUR_TABLE_NAME
FOR EACH ROW
EXECUTE FUNCTION update_updated_at_column();
```

This code ensures that the `updated_at` column is updated only when there are actual changes in the row.
