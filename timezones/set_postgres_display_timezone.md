- [1. **Modify the `postgresql.conf` File**](#1-modify-the-postgresqlconf-file)
  - [Steps:](#steps)
- [2. **Set it in the Database Directly**](#2-set-it-in-the-database-directly)
  - [Steps:](#steps-1)
- [3. **Set Default for a Specific Role**](#3-set-default-for-a-specific-role)
- [4. **Verify the Change**](#4-verify-the-change)
- [5. **For Supabase**](#5-for-supabase)

To set the `timezone` globally and persistently in PostgreSQL, you need to modify the database server configuration. Here's how you can do it:

---

### 1. **Modify the `postgresql.conf` File**

This file contains PostgreSQL's configuration settings, including the default timezone.

#### Steps:

1. Locate the `postgresql.conf` file. Its location depends on your system and installation:

   - Typical locations:
     - `/etc/postgresql/<version>/main/postgresql.conf` (Linux)
     - `/usr/local/var/postgres/postgresql.conf` (macOS)
     - In Docker, it could be in `/var/lib/postgresql/data/`.

2. Open the file with a text editor:

   ```bash
   sudo nano /path/to/postgresql.conf
   ```

3. Find the `timezone` setting and set it to your desired value:

   ```conf
   timezone = 'America/Los_Angeles'
   ```

4. Save the file and exit.

5. Restart the PostgreSQL server to apply the changes:
   ```bash
   sudo service postgresql restart
   ```
   Or use the appropriate command for your system.

---

### 2. **Set it in the Database Directly**

If you don’t have access to the configuration file, you can set the timezone globally for a specific database using the SQL `ALTER DATABASE` command.

#### Steps:

1. Connect to your PostgreSQL database:

   ```bash
   psql -U username -d your_database
   ```

2. Run this command to set the default timezone for the database:
   ```sql
   ALTER DATABASE your_database SET timezone = 'America/Los_Angeles';
   ```

This setting will apply to all new connections to the database.

---

### 3. **Set Default for a Specific Role**

If you want to set the timezone for a specific user/role:

```sql
ALTER ROLE your_role SET timezone = 'America/Los_Angeles';
```

---

### 4. **Verify the Change**

To ensure the change was applied, you can reconnect to the database and run:

```sql
SHOW timezone;
```

---

### 5. **For Supabase**

Since Supabase does not give direct access to the `postgresql.conf` file, use the `ALTER DATABASE` or `ALTER ROLE` commands to set the timezone persistently for your specific database or user.
