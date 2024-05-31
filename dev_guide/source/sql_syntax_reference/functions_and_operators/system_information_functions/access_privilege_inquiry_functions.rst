:original_name: dws_06_0339.html

.. _dws_06_0339:

Access Privilege Inquiry Functions
==================================

has_any_column_privilege(user, table, privilege)
------------------------------------------------

Description: Queries whether a specified user has permission for any column of table.

Parameters: **user** can be declared by name (text type) or OID. **table** can be declared by name (text type) or OID. **privilege** is declared using a text string (text type). The text string can be **SELECT**, **INSERT**, **UPDATE**, **REFERENCES**, or multiple permission types separated by commas (,).

Return type: boolean

has_any_column_privilege(table, privilege)
------------------------------------------

Description: Queries whether the current user has permission for any column of table.

Parameters: **table** can be declared by name (text type) or OID. **privilege** is declared using a text string (text type). The text string can be **SELECT**, **INSERT**, **UPDATE**, **REFERENCES**, or multiple permission types separated by commas (,).

Return type: boolean

**has_any_column_privilege** checks whether a user can access any column of a table in a particular way. Its parameter possibilities are analogous to **has_table_privilege**, except that the desired access permission type must be some combination of SELECT, INSERT, UPDATE, or REFERENCES.

.. note::

   Note that having any of these permissions at the table level implicitly grants it for each column of the table, so **has_any_column_privilege** will always return **true** if **has_table_privilege** does for the same parameters. But **has_any_column_privilege** also succeeds if there is a column-level grant of the permission for at least one column.

has_column_privilege(user, table, column, privilege)
----------------------------------------------------

Description: Queries whether a specified user has permission for column.

Parameters: **user** can be declared by name (text type) or OID. **table** can be declared by name (text type) or OID. **column** can be declared by a column name (text type) or an attribute number (smallint type). **privilege** is declared using a text string. The text string can be **SELECT**, **INSERT**, **UPDATE**, **REFERENCES**, or multiple permission types separated by commas (,).

Return type: boolean

has_column_privilege(table, column, privilege)
----------------------------------------------

Description: Queries whether the current user has permission for column.

Parameters: **table** can be declared by name (text type) or OID. **column** can be declared by a column name (text type) or an attribute number (smallint type). **privilege** is declared using a text string. The text string can be **SELECT**, **INSERT**, **UPDATE**, **REFERENCES**, or multiple permission types separated by commas (,).

Return type: boolean

**has_column_privilege** checks whether a user can access a column in a particular way. Its argument possibilities are analogous to **has_table_privilege**, with the addition that the column can be specified either by name or attribute number. The desired access permission type must evaluate to some combination of **SELECT**, **INSERT**, **UPDATE**, or **REFERENCES**.

.. note::

   Note that having any of these permissions at the table level implicitly grants it for each column of the table.

has_database_privilege(user, database, privilege)
-------------------------------------------------

Description: Queries whether a specified user has permission for database.

Parameters: **user** can be declared by name (text type) or OID. **database** can be declared by name (text type) or OID. **privilege** is declared using a text string. The text string can be **SELECT**, **INSERT**, **UPDATE**, **REFERENCES**, or multiple permission types separated by commas (,).

Return type: boolean

has_database_privilege(database, privilege)
-------------------------------------------

Description: Queries whether the current user has permission for database.

Parameters: **database** can be declared by name (text type) or OID. **privilege** is declared using a text string. The text string can be **CREATE**, **CONNECT**, **TEMPORARY**, or **TEMP**, or multiple permission types separated by commas (,).

Return type: boolean

Note: **has_database_privilege** checks whether a user can access a database in a particular way. Its argument possibilities are analogous to **has_table_privilege**. The desired access permission type must evaluate to some combination of **CREATE**, **CONNECT**, **TEMPORARY**, or **TEMP** (which is equivalent to **TEMPORARY**).

has_foreign_data_wrapper_privilege(user, fdw, privilege)
--------------------------------------------------------

Description: Queries whether a specified user has permission for foreign-data wrapper.

Parameters: **user** can be declared by name (text type) or OID. **fdw** indicates a foreign-data wrapper, which can be declared by name (text type) or OID. **privilege** is declared using a text string, which must be USAGE.

The **fdw** parameter indicates the name or ID of the foreign data wrapper.

Return type: boolean

has_foreign_data_wrapper_privilege(fdw, privilege)
--------------------------------------------------

Description: Queries whether the current user has permission for foreign-data wrapper.

Parameters: **fdw** indicates a foreign-data wrapper, which can be declared by name (text type) or OID. **privilege** is declared using a text string, which must be **USAGE**.

Return type: boolean

Note: **has_foreign_data_wrapper_privilege** checks whether a user can access a foreign-data wrapper in a particular way. Its argument possibilities are analogous to **has_table_privilege**. The desired access permission type must evaluate to **USAGE**.

has_function_privilege(user, function, privilege)
-------------------------------------------------

Description: Queries whether a specified user has permission for function.

Parameters: **user** can be declared by name (text type) or OID. **function** can be declared by name (text type) or OID. **privilege** is declared using a text string, which must be **EXECUTE**.

Return type: boolean

has_function_privilege(function, privilege)
-------------------------------------------

Description: Queries whether the current user has permission for function.

Parameters: **function** can be declared by name (text type) or OID. **privilege** is declared using a text string, which must be **EXECUTE**.

Return type: boolean

Note: **has_function_privilege** checks whether a user can access a function in a particular way. Its argument possibilities are analogous to **has_table_privilege**. When a function is specified by a text string rather than by OID, the allowed input is the same as that for the **regprocedure** data type (see :ref:`Object Identifier Types <dws_06_0022>`). The desired access permission type must evaluate to **EXECUTE**.

has_language_privilege(user, language, privilege)
-------------------------------------------------

Description: Queries whether a specified user has permission for language.

Parameters: **user** can be declared by name (text type) or OID. **language** can be declared by name (text type) or OID. **privilege** is declared using a text string, which must be **USAGE**.

Return type: boolean

has_language_privilege(language, privilege)
-------------------------------------------

Description: Queries whether the current user has permission for language.

Parameters: **language** can be declared by name (text type) or OID. **privilege** is declared using a text string, which must be **USAGE**.

Return type: boolean

Note: **has_language_privilege** checks whether a user can access a procedural language in a particular way. Its argument possibilities are analogous to **has_table_privilege**. The desired access permission type must evaluate to **USAGE**.

has_schema_privilege(user, schema, privilege)
---------------------------------------------

Description: Queries whether a specified user has permission for schema.

Parameters: **user** can be declared by name (text type) or OID. **schema** can be declared by name (text type) or OID. **privilege** is declared using a text string, which can be **CREATE**, **USAGE**, or multiple permission types separated by commas (,).

Return type: boolean

has_schema_privilege(schema, privilege)
---------------------------------------

Description: Queries whether the current user has permission for schema.

Parameters: **schema** can be declared by name (text type) or OID. **privilege** is declared using a text string, which can be **CREATE**, **USAGE**, or multiple permission types separated by commas (,).

Return type: boolean

Note: **has_schema_privilege** checks whether a user can access a schema in a particular way. Its argument possibilities are analogous to **has_table_privilege**. The desired access permission type must evaluate to some combination of **CREATE** or **USAGE**.

has_server_privilege(user, server, privilege)
---------------------------------------------

Description: Queries whether a specified user has permission for foreign server.

Parameters: **user** can be declared by name (text type) or OID. **server** can be declared by name (text type) or OID. **privilege** is declared using a text string, which must be **USAGE**.

Return type: boolean

has_server_privilege(server, privilege)
---------------------------------------

Description: Queries whether the current user has permission for foreign server.

Parameters: **server** can be declared by name (text type) or OID. **privilege** is declared using a text string, which must be **USAGE**.

Return type: boolean

Note: **has_server_privilege** checks whether a user can access a foreign server in a particular way. Its argument possibilities are analogous to **has_table_privilege**. The desired access permission type must evaluate to **USAGE**.

has_table_privilege(user, table, privilege)
-------------------------------------------

Description: Queries whether a specified user has permission for table.

Parameters: **user** can be declared by name (text type) or OID. **table** can be declared by name (text type) or OID. **privilege** is declared using a text string, which can be **SELECT**, **INSERT**, **UPDATE**, **DELETE**, **TRUNCATE**, **REFERENCES**, or **TRIGGER**, or multiple permission types separated by commas (,).

Return type: boolean

has_table_privilege(table, privilege)
-------------------------------------

Description: Queries whether the current user has permission for table.

Parameters: **table** can be declared by name (text type) or OID. **privilege** is declared using a text string, which can be **SELECT**, **INSERT**, **UPDATE**, **DELETE**, **TRUNCATE**, **REFERENCES**, or **TRIGGER**, or multiple permission types separated by commas (,).

Return type: boolean

**has_table_privilege** checks whether a user can access a table in a particular way. The user can be specified by name, by OID (**pg_authid.oid**), **public** to indicate the PUBLIC pseudo-role, or if the argument is omitted **current_user** is assumed. The table can be specified by name or by OID. When specifying by name, the name can be schema-qualified if necessary. If a text string is used to declare **privilege**, you can add **WITH GRANT OPTION** to the permission type to test whether the permission has the grant option. When there are multiple permission types, owning one of them will lead to a **true** result.

Example:

::

   SELECT has_table_privilege('tpcds.web_site', 'select');
    has_table_privilege
   ---------------------
    t
   (1 row)

   SELECT has_table_privilege('dbadmin', 'tpcds.web_site', 'select,INSERT WITH GRANT OPTION ');
    has_table_privilege
   ---------------------
    t
   (1 row)

pg_has_role(user, role, privilege)
----------------------------------

Description: Queries whether a specified user has permission for role.

Parameters: **user** can be declared by name (text type) or OID. **role** can be declared by name (text type) or OID. **privilege** is declared using a text string, which can be **MEMBER**, **USAGE**, or multiple permission types separated by commas (,).

Return type: boolean

pg_has_role(role, privilege)
----------------------------

Description: Specifies whether the current user has permission for role.

Parameters: **role** can be declared by name (text type) or OID. **privilege** is declared using a text string, which can be **MEMBER**, **USAGE**, or multiple permission types separated by commas (,).

Return type: boolean

Note: **pg_has_role** checks whether a user can access a role in a particular way. Its argument possibilities are analogous to **has_table_privilege**, except that **public** is not allowed as a user name. The desired access permission type must evaluate to some combination of **MEMBER** or **USAGE**. **MEMBER** denotes direct or indirect membership in the role (that is, the right to do **SET ROLE**), while **USAGE** denotes the permissions of the role are available without doing **SET ROLE**.
