- [Run this query](#run-this-query)
- [pgsodium deprecated](#pgsodium-deprecated)

### Run this query

```
SELECT conrelid::regclass AS table_name, conname AS constraint_name
FROM pg_constraint
WHERE contype = 'f' AND conrelid = confrelid;
```

https://www.reddit.com/r/Supabase/comments/1bold5d/pg_dump_warning_there_are_circular_foreignkey/

### pgsodium deprecated

https://supabase.com/docs/guides/database/extensions/pgsodium
