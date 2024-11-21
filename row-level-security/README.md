For full security notes, see https://github.com/masaok/security-notes

## Supabase RLS

Supabase's Row-Level Security (RLS) is not a replacement for an API but rather a security feature that works alongside your API to control data access at the database level.

**Row-Level Security (RLS)** is a feature in PostgreSQL (and thus in Supabase, which is built on PostgreSQL) that allows you to define policies to control which rows can be accessed or modified by different users. RLS can be very powerful in ensuring that users can only see or modify data they are authorized to, directly in the database.

**APIs (Application Programming Interfaces)** are designed to provide a controlled way for clients (such as web or mobile applications) to interact with your backend services and databases. They offer a layer of abstraction and control, handling things like authentication, authorization, input validation, business logic, and integrating with other services.

Here's how they complement each other:

- **Security and Access Control**: RLS ensures that users can only access the data they are permitted to, based on the policies you set at the database level. This adds an extra layer of security, even if your API has vulnerabilities.
- **Business Logic and Data Transformation**: APIs handle business logic, data transformation, and integration with other services. They are the main interface through which clients interact with your system.
- **User Authentication and Authorization**: APIs typically handle authentication (verifying the identity of users) and authorization (determining what an authenticated user is allowed to do). While RLS can enforce row-level access control, APIs usually enforce broader access control policies.

In summary, while RLS enhances security at the database level, it does not replace the need for an API, which provides essential functionality for interacting with your backend services and handling various aspects of your application logic.
