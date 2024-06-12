### Client not return expected data from a given table or row even though the data exists in the Supabase web interface.

- Check the RLS policies on the table or view (or one of the joined tables).

### Schema Dump

**Install Docker Desktop on Windows first.**

Then, in WSL:

```
apt update -y
apt install -y docker.io
docker version
docker info
```

https://supabase.com/docs/reference/cli/supabase-db-dump
