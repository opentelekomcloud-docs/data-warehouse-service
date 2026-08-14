:original_name: dws_06_0367.html

.. _dws_06_0367:

CREATE EXTERNAL SCHEMA
======================

Function
--------

**CREATE EXTERNAL SCHEMA** creates an external schema. This syntax is supported only by clusters of version 8.3.0 or later.

You can use **CREATE EXTERNAL SCHEMA** to establish a metadata mapping channel between a DWS cluster and an external data source (such as LakeFormation, DLI, or Hive Metastore) to efficiently access external data sources in real time. You can directly query data using "external schema name.table name" without manually creating a foreign table or migrating data, significantly improving cross-source data access and analysis.

Precautions
-----------

-  A user who has the CREATE permission on the current database can create an external schema.
-  When creating a named object, do not use EXTERNAL SCHEMA as the prefix. Objects cannot be created in EXTERNAL SCHEMA.
-  **CREATE EXTERNAL SCHEMA** does not support subcommands for creating objects in the new schema.

Syntax
------

Create an external schema.

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

.. table:: **Table 1** CREATE EXTERNAL SCHEMA parameters

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                                                               | Value Range                                                                                                                                                                                                                                                                       |
   +=======================+===========================================================================================================================================+===================================================================================================================================================================================================================================================================================+
   | schema_name           | Name of an external schema.                                                                                                               | A string, which must comply with :ref:`Identifier Naming Conventions <en-us_topic_0000001811634529__section1475018612353>`.                                                                                                                                                       |
   |                       |                                                                                                                                           |                                                                                                                                                                                                                                                                                   |
   |                       | -  The name must be unique.                                                                                                               |                                                                                                                                                                                                                                                                                   |
   |                       | -  The schema name cannot start with **pg\_**.                                                                                            |                                                                                                                                                                                                                                                                                   |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SOURCE                | Type of the external metadata storage engine.                                                                                             | Currently, only **dli**,\ **lakeformation**, and **hive** are supported.                                                                                                                                                                                                          |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DATABASE              | Database corresponding to the external schema.                                                                                            | ``-``                                                                                                                                                                                                                                                                             |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SERVER                | Name of the external server associated with the external schema. External data can be accessed after the association.                     | Servers whose type is **lf**, **obs**,\ **dli**, or **hdfs** can be associated. For details, see :ref:`CREATE SERVER <dws_06_0175>`. For details about how to create a server of the lf type, see "Managing LakeFormation Data Sources" in the Data Warehouse Service User Guide. |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CATALOG               | Catalog to be accessed in LakeFormation. This parameter is mandatory only when server type is set to **lf**.                              | ``-``                                                                                                                                                                                                                                                                             |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | OPTIONS               | Specifies the parameters for a foreign table are listed in the right column. This function is supported only by 8.3.0 and later versions. | dli_project_id: Specifies the project ID corresponding to DLI. You can obtain the project ID from the management console. This parameter is available only when the server type is DLI.                                                                                           |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | METAADDRESS           | Hive Metastore communication interface. This parameter is supported only by 9.1.0 and later versions.                                     | ``-``                                                                                                                                                                                                                                                                             |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CONFIGURATION         | Path for storing Hive Metastore configuration files. This parameter is supported only by 9.1.0 and later versions.                        | If objects in the schema on the current search path are with the same name, specify the schemas different objects are in. You can run the **SHOW SEARCH_PATH** command to check the schemas on the current search path.                                                           |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example: Read the LakeFormation Table Using the External Schema
---------------------------------------------------------------

Read the LakeFormation table using the external schema.

#. Create **lf_server**. The corresponding foreign data wrapper is **DFS_FDW**.

   .. note::

      For details about how to create **lf_server**, see section "Managing LakeFormation Data Sources" in *Data Warehouse Service (DWS) User Guide*.

#. Create an external schema.

   ::

      CREATE EXTERNAL SCHEMA ex_lf
          WITH SOURCE lakeformation
               DATABASE 'demo'
               SERVER lf_server
               CATALOG 'hive';

   -  Set **SOURCE** to **lakeformation**.
   -  Set **SERVER** to **lf_server**. The server is the DLI server associated with the table.
   -  Set **DATABASE** to the corresponding LakeFormation database.
   -  Set **CATALOG** to the catalog of LakeFormation to be accessed. Replace it as needed.

#. Grant permissions to a role.

   a. Query the current user.

      ::

         SELECT current_user;

   b. Create a role that matches the current role on the LakeFormation management plane and assign it the table access permissions.

#. Query data in the LakeFormation table using the external schema. **test_lf** indicates the LakeFormation table to be accessed.

   ::

      SELECT COUNT(*) FROM ex_dli.test_lf;
       count
      -------
          20
      (1 row)

Reading the Hive Metastore Table Using an External Schema
---------------------------------------------------------

Read the Hive Metastore table using an external schema. The cluster version must be 9.1.0 or later.

#. Create the OBS/HDFS server, with **DFS_FDW** as the foreign data wrapper.

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

#. Before manually creating an external schema to interconnect with Hive Metastore, you need to:

   a. Obtain the **hive-site.xml** and **hivemetastore-site.xml** configuration files from the cluster management page or the Hive installation directory, and upload them to the **CONFIGURATION** directory.

   b. Add the **elk-site.xml** configuration file and configuration items (or add configuration items to the original **elk-site.xml** configuration file). Replace the username, domain name, and keytab file name as required.

      ::

                 <property>
                         <name>hive.kerberos.principal</name>
                         <value>user/hadoop.hadoop.com@HADOOP.COM</value> //User for connecting to Hadoop@Domain name
                 </property>
                 <property>
                         <name>hive.kerberos.keytab</name>
                         <value>{Configuration directory}/user.keytab</value> // Absolute path of the keytab file of the user for connecting to Hadoop
                 </property>

#. Create an external schema.

   ::

      CREATE EXTERNAL SCHEMA ex_hms
          WITH SOURCE source_type
          DATABASE 'db_name'
          SERVER srv_name
          METAADDRESS 'address'
          CONFIGURATION 'confpath';

   -  Set **SOURCE** to **hive**.
   -  Set **SERVER** to the OBS/HDFS server associated with the table.
   -  Set **DATABASE** to the corresponding HMS database.
   -  Set **METAADDRESS** to the Hive Metastore communication interface.
   -  Set **CONFIGURATION** to an absolute path for storing Hive Metastore configuration files. Replace it with the actual path.

#. Query data in the HMS table using the external schema. **test_hms** indicates the HMS table to be accessed. Replace it as needed.

   ::

      SELECT COUNT(*) FROM ex_hms.test_hms;
       count
      -------
          20
      (1 row)

Helpful Links
-------------

:ref:`ALTER EXTERNAL SCHEMA <dws_06_0366>`
