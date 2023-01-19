:original_name: dws_06_0120.html

.. _dws_06_0120:

ALTER DATABASE
==============

Function
--------

This command is used to modify the attributes of a database, including the database name, owner, maximum number of connections, and object isolation attribute.

Important Notes
---------------

-  Only the owner of a database or a system administrator has the permission to run the **ALTER DATABASE** statement. Users other than system administrators may have the following permission constraints depending on the attributes to be modified:

   -  To modify the database name, you must have the CREATEDB permission.
   -  To modify a database owner, you must be a database owner and a member of the new owner, and have the CREATEDB permission.
   -  To change the default tablespace, you must be a database owner or a system administrator, and must have the CREATE permission on the new tablespace. This statement physically migrates tables and indexes in a default tablespace to a new tablespace. Note that tables and indexes outside the default tablespace are not affected.
   -  Only a database owner or a system administrator can modify GUC parameters for the database.
   -  Only database owners and system administrators can modify the object isolation attribute of a database.

-  You are not allowed to rename a database in use. To rename it, connect to another database.
-  Database compatibility cannot be modified, it can only be specified during database creation. For details, see the section "CREATE DATABASE" in *SQL Syntax Reference*

Syntax
------

-  Modify the maximum number of connections of the database.

   ::

      ALTER DATABASE database_name
          [ [ WITH ] CONNECTION LIMIT connlimit ];

-  Rename the database.

   ::

      ALTER DATABASE database_name
          RENAME TO new_name;

   .. note::

      If the database contains OBS hot or cold tables, the database name cannot be changed.

-  Change the database owner.

   ::

      ALTER DATABASE database_name
          OWNER TO new_owner;

-  Change the default tablespace of the database.

   ::

      ALTER DATABASE database_name
          SET TABLESPACE new_tablespace;

   .. note::

      The current tablespaces cannot be changed to OBS tablespaces.

-  Modify the session parameter value of the database.

   ::

      ALTER DATABASE database_name
          SET configuration_parameter { { TO | = } { value | DEFAULT } | FROM CURRENT };

-  Reset the database configuration parameter.

   ::

      ALTER DATABASE database_name RESET
          { configuration_parameter | ALL };

-  Modify the object isolation attribute of a database.

   ::

      ALTER DATABASE database_name [ WITH ] { ENABLE | DISABLE } PRIVATE OBJECT;

   .. note::

      -  To modify the object isolation attribute of a database, the database must be connected. Otherwise, the modification will fail.
      -  For a new database, the object isolation attribute is disabled by default. After this attribute is enabled, common users can view only the objects (such as tables, functions, views, and columns) that they have the permission to access. This attribute does not take effect for administrators. After this attribute is enabled, administrators can still view all database objects.

Parameter Description
---------------------

-  **database_name**

   Specifies the name of the database whose attributes are to be modified.

   Value range: a string. It must comply with the naming convention.

-  **connlimit**

   Specifies the maximum number of concurrent connections that can be made to this database (excluding administrators' connections).

   Value range: The value must be an integer, preferably between **1** and **50**. The default value **-1** indicates no restrictions.

-  **new_name**

   Specifies the new name of a database.

   Value range: a string. It must comply with the naming convention.

-  **new_owner**

   Specifies the new owner of a database.

   Value range: a string indicating a valid user name

-  **configuration_parameter**

   **value**

   Sets a specified database session parameter. If the value is **DEFAULT** or **RESET**, the default setting is used in the new session. **OFF** closes the setting.

   Value range: a string. It can be set to:

   -  DEFAULT
   -  OFF
   -  RESET

-  **FROM CURRENT**

   Sets the value based on the database connected to the current session.

-  **RESET configuration_parameter**

   Resets the specified database session parameter.

-  **RESET ALL**

   Resets all database session parameters.

.. note::

   -  Modifies the default tablespace of a database by moving all the tables or indexes from the old tablespace to the new one. This operation does not affect the tables or indexes in other non-default tablespaces.
   -  The modified database session parameter values will take effect in the next session.

Examples
--------

Modify the number of connections of the **music** database.

::

   ALTER DATABASE music CONNECTION LIMIT= 10;

Change the name of the **music** database to **music1**.

::

   ALTER DATABASE music RENAME TO music1;

Change the owner of the **music1** database.

::

   ALTER DATABASE music1 OWNER TO tom;

Modify the tablespace of the **music1** database.

::

   ALTER DATABASE music1 SET TABLESPACE PG_DEFAULT;

Disable the default index scan on the **music1** database.

::

   ALTER DATABASE music1 SET enable_indexscan TO off;

Reset the **enable_indexscan** parameter of the **music1** database.

::

   ALTER DATABASE music1 RESET enable_indexscan;

Links
-----

:ref:`CREATE DATABASE <dws_06_0156>`, :ref:`DROP DATABASE <dws_06_0189>`
