:original_name: dws_06_0367.html

.. _dws_06_0367:

CREATE EXTERNAL SCHEMA
======================

Function
--------

Creates an EXTERNAL schema. This syntax is supported only in 8.3.0 and later versions.

The external schema is used to access tables of the LakeFormation, DLI, or HMS service. You can use an external schema name as the prefix for access. If there is no schema name prefix, you can access the named objects in the current schema.

Precautions
-----------

-  A user who has the CREATE permission on the current database can create a foreign schema.
-  When creating a named object, do not use EXTERNAL SCHEMA as the prefix. Objects cannot be created in EXTERNAL SCHEMA.
-  **CREATE EXTERNAL SCHEMA** does not support subcommands for creating objects in the new schema.

Syntax
------

-  Create an external schema with a specified name.

   ::

      CREATE EXTERNAL SCHEMA schema_name
          WITH SOURCE source_name
               DATABASE 'database_name'
               SERVER server_name
               [ CATALOG 'catalog_name' ]
               [ OPTIONS ( { option_name ' value ' } [, ...] ) ]
               [METAADDRESS 'address']
               [CONFIGURATION 'confpath'];

Parameter Description
---------------------

-  **schema_name**

   Name of an external schema.

   Value range: a string. It must comply with the naming convention.

   .. important::

      -  The name must be unique.
      -  The name cannot start with **pg\_**.

-  **SOURCE**

   Type of the external metadata storage engine. Currently, **source_type** can only be **dli**, **lakeformation**, or **hive**.

-  **DATABASE**

   Database corresponding to the external schema.

-  **SERVER**

   Value range: an existing foreign server whose type is **lf**, **dli**, **obs**, or **hdfs**.

   You can associate an external schema with a foreign server to access external data.

-  **CATALOG**

   Catalog to be accessed in LakeFormation. This parameter is mandatory only when server type is set to **lf**.

-  **OPTIONS**

   Specifies the following parameters for a foreign table: This function is supported only by 8.3.0 and later versions.

   dli_project_id

   Specifies the project ID corresponding to DLI. You can obtain the project ID from the management console. This parameter is available only when the server type is DLI.

-  **METAADDRESS**

   Hivemetastore communication interface. This parameter is supported only by 9.1.0 and later versions.

-  **CONFIGURATION**

   Path for storing **hivemetastore** configuration files. This parameter is supported only by 9.1.0 and later versions.

   .. note::

      If objects in the schema on the current search path are with the same name, specify the schemas different objects are in. You can run the **SHOW SEARCH_PATH** command to check the schemas on the current search path.

Examples
--------

-  Read the LakeFormation table using the external schema.

   -  Create **lf_server**. The corresponding foreign data wrapper is **DFS_FDW**.

      .. note::

         For details about how to create **lf_server**, see section "Managing LakeFormation Data Sources" in User Guide.

   -  Create an external schema, set **SOURCE** to **lakeformation**, and set the DLI server associated with the table to **lf_server**. In the following command, **DATABASE** is the LakeFormation database, and **CATALOG** is the LakeFormation catalog to be accessed. Replace them with the actual ones you use.

      ::

         CREATE EXTERNAL SCHEMA ex_lf
             WITH SOURCE lakeformation
                  DATABASE 'demo'
                  SERVER lf_server
                  CATALOG 'hive';

   -  Role authorization

      -  Query the current user.
      -  SELECT current_user;
      -  Create a role that matches the current role on the LakeFormation management plane and assign it the table access permissions.

   -  Query data in the LakeFormation table using the external schema. **test_lf** indicates the LakeFormation table to be accessed.

      ::

         SELECT COUNT(*) FROM ex_dli.test_lf;
          count
         -------
             20
         (1 row)

-  Read a DLI multi-version foreign table using external schema. This function is supported only in 8.3.0 or later.

   .. note::

      DLI tables and DLI internal tables in Lakehouse mode can be accessed.

   -  Create **dli_server**, with **DFS_FDW** as the foreign data wrapper.

      ::

         CREATE SERVER dli_server FOREIGN DATA WRAPPER DFS_FDW OPTIONS (
           ADDRESS 'obs.example.com',
           ACCESS_KEY 'xxxxxxxxx',
           SECRET_ACCESS_KEY 'yyyyyyyyyyyyy',
           TYPE 'DLI',
           DLI_ADDRESS 'dli.example.com',
           DLI_ACCESS_KEY 'xxxxxxxxx',
           DLI_SECRET_ACCESS_KEY 'yyyyyyyyyyyyy'
         );

      .. note::

         -  **ADDRESS** is the endpoint of OBS. **DLI_ADDRESS** is the endpoint of DLI. Replace it with the actual endpoint.
         -  **ACCESS_KEY** and **SECRET_ACCESS_KEY** are access keys for the cloud account system to access OBS. Replace the values as needed.
         -  **DLI_ACCESS_KEY** and **DLI_SECRET_ACCESS_KEY** are access keys for the cloud account system to access DLI. Replace the values as needed.
         -  **TYPE** indicates the server type. Retain the value **DLI**.

   -  Create an external schema, set **SOURCE** to **dli**, and set the DLI server associated with the table to **dli_server**. **project_id** is *xxxxxxxxxxxxxxx*, and **database_name** on DLI is **database123**. Replace them as needed.

      ::

         CREATE EXTERNAL SCHEMA ex_dli
             WITH SOURCE dli
                  DATABASE 'database123'
                  SERVER dli_server
                  options (dli_project_id 'xxxxxxxxxxxxxxx');

   -  Query data in the DLI multi-version table using the external schema. **test_dli** indicates the DLI table to be accessed. Replace it as needed.

      ::

         SELECT COUNT(*) FROM ex_dli.test_dli;
          count
         -------
             20
         (1 row)

-  Reads the **hivemetastore** table using the external schema. Only 9.1.0 and later versions support this function.

   -  Create **obs/hdfs server**, with **DFS_FDW** as the foreign data wrapper.

      ::

         CREATE SERVER hdfs_server
         FOREIGN DATA WRAPPER HDFS_FDW
         OPTIONS (
         address '***.***.***.***:9000',
         type'HDFS');

         CREATE SERVER obs_server
         FOREIGN DATA WRAPPER dfs_fdw
         OPTIONS (
             address 'obs.example.com' ,
             ACCESS_KEY 'access_key_value_to_be_replaced',
             SECRET_ACCESS_KEY 'secret_access_key_value_to_be_replaced',
             encrypt 'on',
             type 'obs' );

   -  Create an external schema. Set **SOURCE** to **hive** and the server associated with the table to **obs/hdfs server**. **DATABASE** indicates the HMS database, **METAADDRESS** indicates the HiveMetaStore communication interface, and **CONFIGURATION** indicates the location where HiveMetaStore configuration files are saved. Adjust these parameters to meet your site's requirements.

      ::

         CREATE EXTERNAL SCHEMA ex_hms
             WITH SOURCE source_type
             DATABASE 'db_name'
             SERVER srv_name
             METAADDRESS 'address'
             CONFIGURATION 'confpath';

   -  Queries data in the HMS table using the external schema. **test_hms** indicates the HMS table to be accessed. Replace it as needed.

      ::

         SELECT COUNT(*) FROM ex_hms.test_hms;
          count
         -------
             20
         (1 row)

Helpful Links
-------------

:ref:`ALTER EXTERNAL SCHEMA <dws_06_0366>`
