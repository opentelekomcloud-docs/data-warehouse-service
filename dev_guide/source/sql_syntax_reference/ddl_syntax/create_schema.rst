:original_name: dws_06_0173.html

.. _dws_06_0173:

CREATE SCHEMA
=============

Function
--------

Creates a schema.

Named objects are accessed either by "qualifying" their names with the schema name as a prefix, or by setting a search path that includes the desired schema(s). When creating named objects, you can also use the schema name as a prefix.

Optionally, **CREATE SCHEMA** can include sub-commands to create objects within the new schema. The sub-commands are treated essentially the same as separate commands issued after creating the schema, If the **AUTHORIZATION** clause is used, all the created objects are owned by this user.

Precautions
-----------

-  As long as the current user has the CREATE permission, the user can create a schema.
-  The owner of an object created by a system administrator in a schema with the same name as a common user is the common user, not the system administrator.

Syntax
------

-  Create a schema based on a specified name:

   ::

      CREATE SCHEMA schema_name
          [ AUTHORIZATION user_name ] [ WITH PERM SPACE 'space_limit'] [ schema_element [ ... ] ];

-  Create a schema based on a username:

   ::

      CREATE SCHEMA AUTHORIZATION user_name [ WITH PERM SPACE 'space_limit'] [ schema_element [ ... ] ];

Parameter Description
---------------------

-  **schema_name**

   Indicates the name of the schema to be created.

   .. important::

      -  The name must be unique,
      -  and cannot start with **pg\_**.

   Value range: a string. It must comply with the naming convention rule.

-  **AUTHORIZATION user_name**

   Indicates the name of the user who will own this schema. If **schema_name** is not specified, **user_name** will be used as the schema name. In this case, **user_name** can only be a role name.

   Value range: An existing user name/role.

-  **WITH PERM SPACE 'space_limit'**

   Indicates the storage upper limit of the permanent table in the specified schema. If **space_limit** is not specified, the space is not limited.

   Value range: A string consists of an integer and unit. The unit can be K/M/G/T/P currently. The unit of parsed value is K and cannot exceed the range that can be expressed in 64 bits, which is 1 KB to 9007199254740991 KB.

-  **schema_element**

   Indicates an SQL statement defining an object to be created within the schema. Currently, only **CREATE TABLE**, **CREATE VIEW**, **CREATE INDEX**, **CREATE PARTITION**, and **GRANT** are accepted as clauses within **CREATE SCHEMA**.

   Objects created by sub-commands are owned by the user specified by **AUTHORIZATION**.

.. note::

   If objects in the schema on the current search path are with the same name, specify the schemas different objects are in. You can run the **SHOW SEARCH_PATH** command to check the schemas on the current search path.

Examples
--------

-- Create the **role1** role.

::

   CREATE ROLE role1 IDENTIFIED BY '{password}';

Create a schema named **role1** for the **role1** role. The owner of the **films** and **winners** tables created by the clause is **role1**.

::

   CREATE SCHEMA AUTHORIZATION role1
        CREATE TABLE films (title text, release date, awards text[])
        CREATE VIEW winners AS
        SELECT title, release FROM films WHERE awards IS NOT NULL;

Helpful Links
-------------

:ref:`ALTER SCHEMA <dws_06_0136>`, :ref:`DROP SCHEMA <dws_06_0204>`
