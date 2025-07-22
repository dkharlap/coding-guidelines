# Database naming conventions

## 1. General naming rules

- All identifiers, table, view and column names are in American English. Be aware of differences that exist, such as color/colour and check/cheque.
- Only letters, numbers, and the underscore are allowed in names. No spaces, punctuation, or extended characters are allowed. Spaces confuse front-end data access tools and applications. This avoids the need to use square brackets as delimiters around identifier names in SQL statements. This provides considerable simplification when writing code and SQL statements with little or no loss in readability. A column name of FirstName is just as easy to understand as First Name. But if you must use spaces within the name of a database object, make sure you surround the name with square brackets as shown here: [First Name].
- The first character in a name must be a letter.
- The first letter in the identifier and the first letter of each subsequent concatenated word are capitalized (Pascal case). Use Pascal case names instead of using underscores to separate two worlds of a name (not CUSTOMER_ORDER but CustomerOrder).
- Use underscores only between the actual object name and the suffix.
- Use the name without using pointless or obscure abbreviations. Abbreviations should be avoided entirely if possible, and only used if it is a commonly known meaning.
- Make sure you are not using any reserved words for naming your database objects, as that can lead to some unpredictable situations.

## 2. Tables

- Regular Tables are named using the singular form of the object they represent. For example, if the table contains customer data, the name should be Customer, not Customers.
- Junction Tables are used to break many-to-many relationships down. Junction tables are also known as Composite Tables or Intersections. Such tables are named using the names of both tables. For example, if table is a junction between Order table and Item table, the name should be OrderItem.
- Extension Tables used as extensions to a base table in one-to-one relationship are named using the base name and then a word describing the nature of the extension. For example, you may have a generic Person table with basic name and address columns that has a one-to-one relationship with an extension for only a few individuals. If, for example, you have an extension table storing information specifically about people who are doctors, info for the doctors would be in the PersonDoctor table.
- If your database deals with different logical functions and you want to group your tables according to the logical group they belong to, prefix your table name with a two or three small character prefix that can identify the group. This kind of naming convention makes sure all the related tables are grouped together when you list all your tables in alphabetical order. However, if your database deals with only one logical group of tables, you need not use this naming convention.
- Test and Temporary Tables must have postfix _Test, _Temp, if they belong to a logical group. This rule will help a DBA to keep track of unnecessary tables.
- Every table should have a primary key, preferably consisting of one field called TableNameID. Group prefix of the table that was discussed in 2.4 does not need to be included in the field name. For example, if you have an account namespace named acc and an address table accAddress, the key field of the table should be AddressID, not accAddressID.
- If referring to another table you should use the following conventions. A table Account referring to Person contains the field PersonID to implement the link to Person.

## 3. Columns

- Columns are attributes of an entity, that is, columns describe the properties of an entity. Use meaningful and natural names for columns.
- Do not prefix the column names with the entity that they are representing (not CustomerFirstName but FirstName).
- According to the item 2.6, name for the primary key field should be TableNameID.
- According to the item 2.7, if you have to name the columns in a junction/mapping table, add the postfix ID to the table codes of mapped tables. For example, CustomerID.

## 4. Views

- A view is nothing but a table for any application that is accessing it. The same naming convention defined above for tables (2), applies to views as well with one exception. All views must have prefix vw. This will help to visually separate tables from views in queries.
- If view represents a combination of two tables based on a join condition, thus, effectively representing two entities, use the same naming convention as Junction Tables.
- Views can summarize data from existing base tables in the form of reports. In this case the name of the view can be, for example, vwSummaryOfSalesByYear.

## 5. Stored procedures

- Stored procedures always do some work, they are action oriented. Their name should describe the work they do. Use a verb to describe the work. For example, GetCustomerDetails, InsertCustomerInfo, etc.
- As explained above in the case of tables, you can use a prefix, to group stored procedures also, depending upon the logical group they belong to.
- Never prefix your stored procedures with sp_, unless you are storing the procedure in the master database. If you call a stored procedure prefixed with sp_, SQL Server always looks for this procedure in the master database. Only after checking in the master database (if not found) it searches the current database.
- Do not prefix stored procedures with prefixes like sproc just to make it obvious that the object is a stored procedure. Any database developer/DBA can identify stored procedures in the code as the procedures are always preceded by EXEC or EXECUTE keyword.

## 6. User defined functions

- UDFs are almost similar to stored procedures, except for the fact that UDFs can be used in SELECT statements. The naming conventions discussed above for stored procedures, apply to UDFs as well with one exception. All UDFs must have prefix udf.
- Inline UDF functions are a subset of UDFs that return a table. Inline functions can be used to achieve the functionality of parameterized views. The naming conventions discussed above for views (2.4) are applied to Inline UDFs with one exception. You should not include vw prefix in UDF’s name.

## 7. Triggers

- As triggers always depend on a base table and can't exist on their own, link the base table's name with the trigger name.
- As triggers are associated with one or more of the following operations: Insert, Update, Delete, the name of the trigger should reflect its nature. For example, Customer_Insert, Customer_Update, Customer_Delete.
- If you have more than one trigger per action per table, use action name to distinguish triggers. For example, Customer_ValidateData_Insert, Customer_MakeAuditEntries_Insert.
- If you have a single trigger for more than one action, use the postfixes Insert, Update, Delete together in the name of the trigger. Here's an example: Customer_InsertUpdate.

## 8. Indexes/Keys

- Primary key is the column(s) that can uniquely identify each row in a table. Use the prefix PK_TableName for naming primary keys. SQL Server uses this rule by default.
- Foreign key are used to represent the relationships between tables that are related. Use the following naming convention for foreign keys: FK_ReferencingTable_ReferencedTable. For example, FK_Order_Customer. SQL Server uses this rule by default.
- You usually don't need more than one relation between the same tables. But if you do, use postfix, which describes meaning of the relation.
- Use the following naming convention for indexes: IX_TableName_Column1(Desc)…_ColumnN(Desc).
