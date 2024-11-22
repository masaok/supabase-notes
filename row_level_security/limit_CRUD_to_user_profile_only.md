- [Limiting User CRUD to Their Profile Only](#limiting-user-crud-to-their-profile-only)
  - [Step 1: Enable RLS on the `upload` Table](#step-1-enable-rls-on-the-upload-table)
  - [Step 2: Create Policies](#step-2-create-policies)
    - [1. Select Policy](#1-select-policy)
    - [2. Insert Policy](#2-insert-policy)
    - [3. Update Policy](#3-update-policy)
    - [4. Delete Policy](#4-delete-policy)
  - [Final Steps](#final-steps)
- [USING vs WITH CHECK](#using-vs-with-check)
  - [`USING`](#using)
  - [`WITH CHECK`](#with-check)
  - [Summary](#summary)

## Limiting User CRUD to Their Profile Only

To implement Row-Level Security (RLS) in Supabase for the `upload` table so that each user can only CRUD rows they've inserted, based on their `profile_id`, you'll need to:

1. Enable RLS on the `upload` table.
2. Create a policy that restricts access to rows based on the `profile_id` of the currently logged-in user.

Here's how you can set it up:

### Step 1: Enable RLS on the `upload` Table

```sql
-- Enable row-level security on the upload table
ALTER TABLE public.upload ENABLE ROW LEVEL SECURITY;
```

### Step 2: Create Policies

Next, add policies for each CRUD operation: `SELECT`, `INSERT`, `UPDATE`, and `DELETE`. Each policy will check if the `profile_id` on the row matches the user's `profile_id`.

Assuming you are using Supabase's built-in `auth.uid()` function, which returns the current user's UID (matching the `user_uid` column in `profile`), here’s how to define the policies.

#### 1. Select Policy

This policy allows users to read only their own uploads.

```sql
CREATE POLICY "Allow select own uploads"
ON public.upload
FOR SELECT
USING (
    profile_id = (SELECT profile_id FROM public.profile WHERE user_uid = auth.uid())
);
```

#### 2. Insert Policy

This policy allows users to insert records in the `upload` table but ensures the `profile_id` matches their own `profile_id`.

```sql
CREATE POLICY "Allow insert own uploads"
ON public.upload
FOR INSERT
WITH CHECK (
    profile_id = (SELECT profile_id FROM public.profile WHERE user_uid = auth.uid())
);
```

#### 3. Update Policy

This policy allows users to update only their own uploads.

```sql
CREATE POLICY "Allow update own uploads"
ON public.upload
FOR UPDATE
USING (
    profile_id = (SELECT profile_id FROM public.profile WHERE user_uid = auth.uid())
);
```

#### 4. Delete Policy

This policy allows users to delete only their own uploads.

```sql
CREATE POLICY "Allow delete own uploads"
ON public.upload
FOR DELETE
USING (
    profile_id = (SELECT profile_id FROM public.profile WHERE user_uid = auth.uid())
);
```

### Final Steps

Make sure RLS is enabled and policies are in place:

1. **Enable Policies**: Verify that each policy has been created successfully.
2. **Test the Policies**: Check that users can only access their uploads based on their `profile_id`.

These policies will ensure that users only have CRUD access to rows in the `upload` table that match their own `profile_id`.

## USING vs WITH CHECK

In PostgreSQL Row-Level Security (RLS), `USING` and `WITH CHECK` serve different purposes in controlling access to table rows. Here’s a breakdown of their functions:

### `USING`

The `USING` clause in an RLS policy defines the **condition that must be met for a row to be visible (i.e., accessible)** in a `SELECT`, `UPDATE`, or `DELETE` operation. In other words, `USING` controls **which rows a user can see or interact with** based on the policy.

- If the condition in `USING` is true for a row, then the user can read (SELECT), modify (UPDATE), or delete (DELETE) that row.
- If the condition in `USING` is false for a row, the user cannot see or interact with that row in those operations.

**Example:**

```sql
CREATE POLICY "Allow select own uploads"
ON public.upload
FOR SELECT
USING (
    profile_id = (SELECT profile_id FROM public.profile WHERE user_uid = auth.uid())
);
```

In this example, only rows where `profile_id` matches the user's own `profile_id` are visible to the user.

### `WITH CHECK`

The `WITH CHECK` clause in an RLS policy defines the **condition that must be met for a row to be allowed in `INSERT` or `UPDATE` operations**. In other words, `WITH CHECK` controls **which rows a user can insert or update** based on the policy.

- For `INSERT`: If the condition in `WITH CHECK` is false, the row will not be inserted.
- For `UPDATE`: If the condition in `WITH CHECK` is false, the update operation will fail.

**Example:**

```sql
CREATE POLICY "Allow insert own uploads"
ON public.upload
FOR INSERT
WITH CHECK (
    profile_id = (SELECT profile_id FROM public.profile WHERE user_uid = auth.uid())
);
```

In this example, the user can only insert rows with a `profile_id` that matches their own `profile_id`. If they try to insert a row with a different `profile_id`, the insert will fail.

### Summary

- **`USING`**: Controls which rows a user can **see or interact with** (e.g., `SELECT`, `UPDATE`, `DELETE`).
- **`WITH CHECK`**: Controls which rows a user can **insert or update** based on specific conditions.

In practice, you typically use both clauses together in RLS policies to ensure users can only access and modify rows they are authorized to handle.
