:original_name: dws_06_0120.html

.. _dws_06_0120:

ALTER DATABASE
==============

Function
--------

**ALTER DATABASE** modifies the attributes of a database, including the database name, owner, maximum number of connections, and object isolation attribute.

Precautions
-----------

-  Only the owner of a database or a system administrator has the permission to run the **ALTER DATABASE** statement. Users other than system administrators may have the following permission constraints depending on the attributes to be modified:

   -  To modify the database name, you must have the CREATEDB permission.
   -  To modify a database owner, you must be a database owner and a member of the new owner, and have the CREATEDB permission.
   -  To change the default tablespace, you must be a database owner or a system administrator, and must have the CREATE permission on the new tablespace. This statement physically migrates tables and indexes in a default tablespace to a new tablespace. Note that tables and indexes outside the default tablespace are not affected.
   -  Only a database owner or a system administrator can modify GUC parameters for the database.
   -  Only database owners and system administrators can modify the object isolation attribute of a database.

-  You are not allowed to rename a database in use. To rename it, connect to another database.
-  The compatibility mode of an existing database cannot be changed. You can only specify the compatibility mode when creating a database. For details, see :ref:`CREATE DATABASE <dws_06_0156>`.

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

      If the database contains OBS multi-temperature tables, the database name cannot be changed.

-  Change the database owner.

   ::

      ALTER DATABASE database_name
          OWNER TO new_owner;

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

.. table:: **Table 1** ALTER DATABASE parameters

   +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | Parameter                     | Description                                                                                                                                                                                                                                                              | Value Range                                                                                                      |
   +===============================+==========================================================================================================================================================================================================================================================================+==================================================================================================================+
   | database_name                 | Specifies the name of the database whose attributes are to be modified.                                                                                                                                                                                                  | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`. |
   +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | connlimit                     | Specifies the maximum number of concurrent connections that can be made to this database (excluding administrators' connections). In the production environment, you are advised to set a proper upper limit to prevent excessive connections from exhausting resources. | An integer ranging from 1 to 50 is recommended.                                                                  |
   |                               |                                                                                                                                                                                                                                                                          |                                                                                                                  |
   |                               |                                                                                                                                                                                                                                                                          | The default value **-1** indicates no restrictions.                                                              |
   +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | new_name                      | Specifies the new name of a database.                                                                                                                                                                                                                                    | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`. |
   +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | new_owner                     | Specifies the new owner of a database.                                                                                                                                                                                                                                   | A string that indicates a valid username.                                                                        |
   +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | configuration_parameter       | Sets a specified database session parameter to a specified value.                                                                                                                                                                                                        | The value should be a string.                                                                                    |
   |                               |                                                                                                                                                                                                                                                                          |                                                                                                                  |
   | value                         |                                                                                                                                                                                                                                                                          | -  **DEFAULT** indicates that the default settings of the system are used in a new session.                      |
   |                               |                                                                                                                                                                                                                                                                          | -  **RESET** indicates that the default settings of the system are used in a new session.                        |
   |                               |                                                                                                                                                                                                                                                                          | -  **OFF** indicates that the setting is disabled.                                                               |
   +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | FROM CURRENT                  | Sets the value based on the database connected to the current session.                                                                                                                                                                                                   | ``-``                                                                                                            |
   +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | RESET configuration_parameter | Resets the specified database session parameter.                                                                                                                                                                                                                         | ``-``                                                                                                            |
   +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | RESET ALL                     | Resets all database session parameters.                                                                                                                                                                                                                                  | ``-``                                                                                                            |
   |                               |                                                                                                                                                                                                                                                                          |                                                                                                                  |
   |                               | The modified database session parameter values will take effect in the next session.                                                                                                                                                                                     |                                                                                                                  |
   +-------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+

Examples
--------

Create the database **testdb** and user **user10**.

::

   CREATE DATABASE testdb ENCODING 'UTF8' template = template0;

::

   DROP USER IF EXISTS user10;
   CREATE USER user10 PASSWORD '{Password}';

Set the number of connections of the **testdb** database to **10**.

::

   ALTER DATABASE testdb CONNECTION LIMIT= 10;

Change the database name from **testdb**\ to **testdb_1**.

::

   ALTER DATABASE testdb RENAME TO testdb_1;

Change the owner of the **testdb_1** database to **user10**.

::

   ALTER DATABASE testdb_1 OWNER TO user10;

Disable the default index scan on the **testdb_1** database.

::

   ALTER DATABASE testdb_1 SET enable_indexscan TO off;

Reset parameter **enable_indexscan**.

::

   ALTER DATABASE testdb_1 RESET enable_indexscan;

Links
-----

:ref:`CREATE DATABASE <dws_06_0156>` and :ref:`DROP DATABASE <dws_06_0189>`
