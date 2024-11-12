:original_name: dws_03_0201.html

.. _dws_03_0201:

How Do I View the Table Permissions of a GaussDB(DWS) User?
===========================================================

**Scenario 1**: Run the **information_schema.table_privileges** command to **view the table permissions of a user**. Example:

::

   SELECT * FROM information_schema.table_privileges WHERE GRANTEE='user_name';

|image1|

.. table:: **Table 1** table_privileges columns

   +----------------+----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column         | Data Type      | Description                                                                                                                                                                                                |
   +================+================+============================================================================================================================================================================================================+
   | grantor        | sql_identifier | Permission grantor                                                                                                                                                                                         |
   +----------------+----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | grantee        | sql_identifier | Permission grantee                                                                                                                                                                                         |
   +----------------+----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_catalog  | sql_identifier | Database where the table is                                                                                                                                                                                |
   +----------------+----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_schema   | sql_identifier | Schema where the table is                                                                                                                                                                                  |
   +----------------+----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name     | sql_identifier | Table name                                                                                                                                                                                                 |
   +----------------+----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | privilege_type | character_data | Type of the granted permission. The value can be **SELECT**, **INSERT**, **UPDATE**, **DELETE**, **TRUNCATE**, **REFERENCES**, **ANALYZE**, **VACUUM**, **ALTER**, **DROP**, or **TRIGGER**.               |
   +----------------+----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | is_grantable   | yes_or_no      | Indicates if the permission can be granted to other users. **YES** indicates that the permission can be granted to other users, and **NO** indicates that the permission cannot be granted to other users. |
   +----------------+----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | with_hierarchy | yes_or_no      | Indicates if specific operations are allowed to be inherited at the table level. If the specific operation is **SELECT**, **YES** is displayed. Otherwise, **NO** is displayed.                            |
   +----------------+----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

In the preceding figure, user **u2** has all permissions of table **t2** in schema **u2** and the **SELECT** permission of table **t1** in schema **u1**.

**information_schema.table_privileges** can query only the permissions directly granted to the user, the **has_table_privilege()** function can query both directly granted permissions and indirect permissions (obtained by GRANT role to user). For example:

::

   CREATE TABLE t1 (c1 int);
   CREATE USER u1 password '********';
   CREATE USER u2 password '********';
   GRANT dbadmin to u2; //Indirectly grant permissions through roles.
   GRANT SELECT on t1 to u1; // Directly grant the permission.

   SET ROLE u1 password '********';
   SELECT * FROM public.t1; // Directly grant the permission to access the table.
    c1
   ----
   (0 rows)

   SET ROLE u2 password '********';
   SELECT * FROM public.t1; // Indirectly grant the permission to access the table.
    c1
   ----
   (0 rows)

   RESET role; //Switch back to dbadmin.
   SELECT * FROM information_schema.table_privileges WHERE table_name = 't1'; // Can only view direct grants.
    grantor |  grantee   | table_catalog | table_schema | table_name | privilege_type | is_grantable | with_hierarchy
   ---------+------------+---------------+--------------+------------+----------------+--------------+----------------
    dbadmin | u1         | gaussdb       | public       | t1         | SELECT         | NO           | YES
   (1 rows)

   SELECT has_table_privilege('u2', 'public.t1', 'select'); // Can view both direct and indirect grants.
    has_table_privilege
   ---------------------
    t
   (1 row)

**Scenario 2**: To **check whether a user has permissions on a table**, perform the following steps:

#. Query the **pg_class** system catalog.

   ::

      SELECT * FROM pg_class WHERE relname = 'tablename';

   Check the **relacl** column. The command output is shown in the following figure. For details about the permission parameters, see :ref:`Table 2 <en-us_topic_0000001437886853__en-us_topic_0000001076582357_table94980395217>`.

   -  *rolename*\ =\ *xxxx/yyyy*: indicates that *rolename* has the *xxxx* permission on the table and the permission is obtained from *yyyy*.
   -  =\ *xxxx/yyyy*: indicates that **public** has the *xxxx* permission on the table and the permission is obtained from *yyyy*.

   Take the following figure as an example:

   **joe=arwdDxtA**: indicates that user **joe** has all permissions (**ALL PRIVILEGES**).

   **leo=arw/joe**: indicates that user **leo** has the read, write, and modify permissions, which are granted by user **joe**.

   |image2|

   .. _en-us_topic_0000001437886853__en-us_topic_0000001076582357_table94980395217:

   .. table:: **Table 2** Permissions parameters

      ========= =================================
      Parameter Description
      ========= =================================
      r         SELECT (read)
      w         UPDATE (write)
      a         INSERT (insert)
      d         DELETE
      D         TRUNCATE
      x         REFERENCES
      t         TRIGGER
      X         EXECUTE
      U         USAGE
      C         CREATE
      c         CONNECT
      T         TEMPORARY
      A         ANALYZE|ANALYSE
      arwdDxtA  ALL PRIVILEGES (for tables)
      \*        Actions for preceding permissions
      ========= =================================

#. You can also use the **has_table_privilege** function to query user permissions on tables.

   ::

      SELECT * FROM has_table_privilege('Username','Table_name', 'select');

   For example, query whether user **joe** has the query permission on table **t1**.

   ::

      SELECT * FROM has_table_privilege('joe','t1','select');

   |image3|

.. |image1| image:: /_static/images/en-us_image_0000001439795569.png
.. |image2| image:: /_static/images/en-us_image_0000001117912942.png
.. |image3| image:: /_static/images/en-us_image_0000001118137728.png
