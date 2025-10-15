:original_name: dws_06_0366.html

.. _dws_06_0366:

ALTER EXTERNAL SCHEMA
=====================

Function
--------

Modifies EXTERNAL SCHEMA. This syntax is supported only in 8.3.0 and later versions.

Syntax
------

-  Modifies an external schema based on the specified name.

   ::

      ALTER EXTERNAL SCHEMA schema_name
          WITH [ SOURCE source_name ]
               [ DATABASE 'database_name' ]
               [ SERVER server_name ]
               [ CATALOG 'catalog_name' ]
               [ OPTIONS ( { option_name ' value ' } [, ...] ) ]
               [ METAADDRESS 'address']
               [ CONFIGURATION 'confpath'];

Parameter Description
---------------------

-  **schema_name**

   Name of the external schema to be modified.

-  **SOURCE**

   Type of the external metadata storage engine. Currently, **source_type** can only be **dli**, **lakeformation**, or **hive**.

-  **DATABASE**

   Database corresponding to the external schema.

-  **SERVER**

   Value range: an existing foreign server whose type is **lf**, **dli**, **obs**, or **hdfs**.

   You can associate an external schema with a foreign server to access external data.

-  **CATALOG**

   Catalog to be accessed in LakeFormation.

-  **OPTIONS**

   Foreign table parameters. This function is supported only by 8.3.0 and later versions.

   **dli_project_id**

   Specifies the project ID corresponding to DLI. You can obtain the project ID from the management console. This parameter is available only when the server type is DLI.

-  **METAADDRESS**

   Hivemetastore communication interface. This parameter is supported only by 9.1.0 and later versions.

-  **CONFIGURATION**

   Path for storing **hivemetastore** configuration files. This parameter is supported only by 9.1.0 and later versions.

.. note::

   If objects in the schema on the current search path are with the same name, specify the schemas different objects are in. You can run the **SHOW SEARCH_PATH** command to check the schemas on the current search path.

Examples
--------

Modify the database and FOREIGN SERVER corresponding to ex1.

::

   ALTER EXTERNAL SCHEMA ex1
       WITH DATABASE 'demo'
            SERVER my_server;
