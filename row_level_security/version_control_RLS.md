- [1. Use a SQL Migration Tool](#1-use-a-sql-migration-tool)
- [2. Create SQL Migration Scripts](#2-create-sql-migration-scripts)
- [3. Example Migration Script for RLS Policies](#3-example-migration-script-for-rls-policies)
- [4. Track Changes in Git](#4-track-changes-in-git)
- [5. Apply Migrations in Each Environment](#5-apply-migrations-in-each-environment)
- [6. Testing RLS Migrations](#6-testing-rls-migrations)
- [7. Updating Policies](#7-updating-policies)

To version control RLS (Row-Level Security) policies, you can use SQL migration scripts as part of your database schema management process. By including RLS policies in your migration scripts, you keep track of changes over time and ensure they’re applied consistently across different environments (e.g., development, staging, production). Here’s a step-by-step guide to version-controlling RLS:

### 1. Use a SQL Migration Tool

If you're working with Supabase or a PostgreSQL database, tools like **sqitch**, **Flyway**, **Liquibase**, or even a migration framework within an ORM (e.g., Prisma for Node.js or Alembic for Python) can be helpful. You can also manually create versioned SQL scripts and track them with Git.

### 2. Create SQL Migration Scripts

Write SQL scripts for each change you want to make to your RLS policies. These should include the creation, modification, or deletion of RLS policies, as well as enabling/disabling RLS on tables.

For example:

- **`001_create_profile_table.sql`** – Creates the `profile` table.
- **`002_create_upload_table.sql`** – Creates the `upload` table.
- **`003_enable_rls_upload_table.sql`** – Enables RLS and adds policies for the `upload` table.

### 3. Example Migration Script for RLS Policies

Here’s how your migration script could look for setting up RLS policies on the `upload` table:

**`003_enable_rls_upload_table.sql`**

```sql
-- Enable RLS on the upload table
ALTER TABLE public.upload ENABLE ROW LEVEL SECURITY;

-- Create a SELECT policy to allow users to see only their own uploads
CREATE POLICY "Allow select own uploads"
ON public.upload
FOR SELECT
USING (
    profile_id = (SELECT profile_id FROM public.profile WHERE user_uid = auth.uid())
);

-- Create an INSERT policy to allow users to insert only their own uploads
CREATE POLICY "Allow insert own uploads"
ON public.upload
FOR INSERT
WITH CHECK (
    profile_id = (SELECT profile_id FROM public.profile WHERE user_uid = auth.uid())
);

-- Create an UPDATE policy to allow users to update only their own uploads
CREATE POLICY "Allow update own uploads"
ON public.upload
FOR UPDATE
USING (
    profile_id = (SELECT profile_id FROM public.profile WHERE user_uid = auth.uid())
);

-- Create a DELETE policy to allow users to delete only their own uploads
CREATE POLICY "Allow delete own uploads"
ON public.upload
FOR DELETE
USING (
    profile_id = (SELECT profile_id FROM public.profile WHERE user_uid = auth.uid())
);
```

Each time you make changes to your RLS policies, you would create a new versioned migration file.

### 4. Track Changes in Git

Add these migration scripts to your version control system (like Git), so each change has a history. You can revert to previous versions if needed.

```bash
git add 003_enable_rls_upload_table.sql
git commit -m "Enable RLS and create policies for upload table"
```

### 5. Apply Migrations in Each Environment

Whenever you deploy changes, apply the migration scripts in order. Supabase handles migration application automatically if you use its CLI tools or SQL migrations feature. Otherwise, you can apply the migrations manually in other PostgreSQL setups.

### 6. Testing RLS Migrations

Testing RLS migrations in a development or staging environment before deploying them to production is essential. This can help ensure that the policies behave as expected.

### 7. Updating Policies

When modifying an RLS policy, create a new migration script. For example, if you need to change the `SELECT` policy, you might have a script called:

**`004_modify_select_policy_upload.sql`**

```sql
-- Drop the existing SELECT policy
DROP POLICY IF EXISTS "Allow select own uploads" ON public.upload;

-- Create the new SELECT policy with modified conditions
CREATE POLICY "Allow select own uploads"
ON public.upload
FOR SELECT
USING (
    profile_id = (SELECT profile_id FROM public.profile WHERE user_uid = auth.uid() AND some_additional_condition)
);
```

By following these steps, you’ll have a versioned, consistent, and controlled approach to managing RLS policies across different environments.
