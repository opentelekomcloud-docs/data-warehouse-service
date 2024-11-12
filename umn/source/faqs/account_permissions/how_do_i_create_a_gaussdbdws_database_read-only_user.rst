:original_name: dws_03_0198.html

.. _dws_03_0198:

How Do I Create a GaussDB(DWS) Database Read-Only User?
=======================================================

Scenario
--------

In service development, database administrators use schemas to classify data. For example, in the financial industry, liability data belong to schema **s1**, and asset data belong to schema **s2**.

Now you have to create a read-only user **user1** in the database. The user can access all tables (including new tables to be created in the future) in schema **s1** for daily reading, but cannot insert, modify, or delete data.

Principles
----------

GaussDB(DWS) provides role-based user management. You need to create a read-only role **role1** and grant the role to **user1**.

Procedure
---------

#. Connect to the GaussDB(DWS) database as user **dbadmin**.

#. Run the following SQL statement to create role **role1**:

   ::

      CREATE ROLE role1 PASSWORD disable;

#. Run the following SQL statement to grant permissions to **role1**:

   ::

      The GRANT usage ON SCHEMA s1 TO role1; -- grants the access permission to schema s1.
      GRANT select ON ALL TABLES IN SCHEMA s1 TO role1;    --grants the query permission on all tables in schema s1.
      ALTER DEFAULT PRIVILEGES FOR USER tom IN SCHEMA s1 GRANT select ON TABLES TO role1; -- grants schema s1 the permission to create tables. tom is the owner of schema s1.

#. Run the following SQL statement to grant the role **role1** to the actual user **user1**:

   ::

      GRANT role1 TO user1;

5. Grant read-only access to user1 for a foreign table in schema **s1**.

   ::

      ALTER USER user1 USEFT;

   Without proper permissions, querying the foreign table as a read-only user will result in the error message "ERROR: permission denied to select from foreign table in security mode."

6. Read all table data in schema **s1** as read-only user **user1**.
