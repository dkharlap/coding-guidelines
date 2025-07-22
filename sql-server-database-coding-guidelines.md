# Database Coding Guidelines

## 1. General Coding Guidelines

- Use **stored procedures** instead of inline SQL in application code to leverage performance and security benefits.

- Always specify the **schema/owner** of an object. In most cases, the schema is `dbo`, e.g., `dbo.Contact`.

- Use **triggers** only for enforcing data integrity and business rules—not for returning data or user interaction.

- Use **single quotes** for all character, string, binary, and Unicode constants. Use **double quotes** only for quoted identifiers (e.g., `"TableName"`).

- Avoid storing **binary files or images (BLOBs)** inside the database. Store the file path instead, and manage files outside the database for performance and scalability.

- Avoid using **system tables** directly. They may change in future releases. Use `sp_help*` procedures or `INFORMATION_SCHEMA` views instead.

- Avoid **dynamic SQL** whenever possible. It is generally slower and requires broader access rights for users. Use parameterized queries or stored procedures instead.

- Represent **dates** in the `yyyy/mm/dd` format in all SQL statements to ensure consistency across different server settings.

## 2. Stored Procedures

- **Procedure Format**:

    ```sql
    -- Returns list of authors for the specified book
    CREATE PROCEDURE dbo.BookGetAuthors
        @BookID INT -- Book ID
    AS
        -- SQL body here, tabbed by one level
    ```

- **Indentation**: Nest SQL blocks using one tab level for each nested block:

    ```sql
    IF @@VERSION LIKE 'Microsoft SQL Server 2022%'
    BEGIN
        PRINT 'Microsoft SQL Server 2022 (16.x)'
        RETURN
    END
    ```

- Use `--` for single-line comments. Use `/* ... */` only for multi-line comments (6 lines or more). Avoid nested block comments.

- Use `SET NOCOUNT ON` at the start of stored procedures and triggers to reduce unnecessary network traffic. You can still use `@@ROWCOUNT` to check affected rows.

- After **data modification**, check for errors using `@@ERROR`. For example:

    ```sql
    IF @@ERROR <> 0
        RETURN
    ```

- Use `SCOPE_IDENTITY()` to retrieve the last inserted `IDENTITY` value. Only use `@@IDENTITY` or other methods if inserting across scopes (e.g., triggers).

- Use **table variables** (`DECLARE @MyTable TABLE (...)`) instead of temporary tables where possible for better performance and reduced locking/logging overhead.

- Avoid using `GOTO`. It leads to hard-to-read and error-prone code.

- Use **output parameters** to return single-row results instead of relying on `SELECT`.

- **Do not manage transactions** inside stored procedures. Let the data access layer (DAL) handle transactions (e.g., via ADO.NET).

- Keep stored procedures **simple and atomic**. Implement business logic only when performance necessitates it.

## 3. Queries

- Use **UPPERCASE** for T-SQL keywords and built-in functions. Use **lowercase** for data types (e.g., `int`, `varchar`).

- Always use **table aliases** in `SELECT` statements. Short, intuitive abbreviations are recommended.

- Put each **T-SQL clause** (e.g., `SELECT`, `FROM`, `WHERE`) on a **separate line** for readability.

    ```sql
    SELECT FirstName, LastName
    FROM Authors
    WHERE State = 'CA'
    ```

- If a clause wraps to another line, indent the continuation line by one tab:

    ```sql
    SELECT e.FirstName, e.LastName, l.Name, s.Name, d.Name
    FROM Department d
    INNER JOIN Sector s ON d.DepartmentID = s.DepartmentID
    INNER JOIN Laboratory l ON s.SectorID = l.SectorID
    INNER JOIN Employee e ON l.LaboratoryID = e.LaboratoryID
    ```

- **Never use `SELECT *`**. Always specify only the columns needed.

- Always include a **column list in `INSERT`** statements to ensure compatibility when schema changes.

- **Refer to fields by name**, not by ordinal position in recordsets.

- When updating a single row, include the **primary key** in the `WHERE` clause if possible.

- Avoid using **functions in `WHERE` clauses**, as they may prevent index usage.

- Prefer **ANSI-style JOINs** (e.g., `INNER JOIN`, `LEFT JOIN`) instead of comma-separated joins with conditions in the `WHERE` clause.

- Avoid starting `LIKE` patterns with wildcards (e.g., `%word`). SQL Server cannot use indexes efficiently in such cases. Also avoid using `<>` or `NOT`, which force table scans.

- Favor **uncorrelated subqueries** over correlated subqueries when possible, as they are more efficient.
