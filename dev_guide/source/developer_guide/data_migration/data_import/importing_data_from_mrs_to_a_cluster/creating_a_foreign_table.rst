:original_name: dws_04_0214.html

.. _dws_04_0214:

Creating a Foreign Table
========================

This section describes how to create a Hadoop foreign table in the GaussDB(DWS) database to access the Hadoop structured data stored on MRS HDFS. A Hadoop foreign table is read-only. It can only be queried using **SELECT**.

Prerequisites
-------------

-  You have created an MRS cluster and imported data to the ORC table in the Hive/Spark database.

   For details, see :ref:`Preparing Data in an MRS Cluster <dws_04_0212>`.

-  You have created an MRS data source connection for the GaussDB(DWS) cluster.

   For details, see "Managing MRS Data Sources > Creating an MRS Data Source Connection" in the *Data Warehouse Service User Guide*.

.. _en-us_topic_0000001146240997__en-us_topic_0000001082927067_en-us_topic_0109259517_en-us_topic_0101477886_section129581728111417:

Obtaining the HDFS Path of the MRS Data Source
----------------------------------------------

There are two methods for you to obtain the HDFS path.

-  **Method 1**

   For Hive data, log in to the Hive client of MRS (see :ref:`2 <en-us_topic_0000001146360931__en-us_topic_0000001082830951_en-us_topic_0109259515_en-us_topic_0101477888_li14725131112614>`), run the following command to view the detailed information about the table, and record the data storage path in the **location** parameter:

   .. code-block::

      use <database_name>;
      desc formatted <table_name>;

   For example, if the value of the **location** parameter in the returned result is **hdfs://hacluster/user/hive/warehouse/demo.db/product_info_orc/**, the HDFS path is **/user/hive/warehouse/demo.db/product_info_orc/**.

-  **Method 2**

   Perform the following steps to obtain the HDFS path:

   #. Log in to the MRS management console.

   #. Choose **Cluster** > **Active Cluster** and click the name of the cluster to be queried to enter the page displaying the cluster's basic information.

   #. Click **File Management** and select **HDFS File List**.

   #. Go to the storage directory of the data to be imported to the GaussDB(DWS) cluster and record the path.


      .. figure:: /_static/images/en-us_image_0000001099282726.png
         :alt: **Figure 1** Checking the data storage path on MRS

         **Figure 1** Checking the data storage path on MRS

.. _en-us_topic_0000001146240997__en-us_topic_0000001082927067_en-us_topic_0109259517_en-us_topic_0101477886_section1760214326239:

Obtaining Information About the Foreign Server Connected to the MRS Data Source
-------------------------------------------------------------------------------

#. Use the user who creates the foreign server to connect to the corresponding database.

   Determine whether to use a common user to create a foreign table in the customized database based on requirements.

   -  **Yes**

      a. Ensure that you have created the common user **dbuser** and its database **mydatabase**, and manually created a foreign server in **mydatabase** by following steps in :ref:`Manually Creating a Foreign Server <dws_04_0213>`.

      b. Connect to the database **mydatabase** as user **dbuser** through the database client tool provided by GaussDB(DWS).

         If you have connected to the database using the gsql client, run the following command to switch the user and database:

         .. code-block::

            \c mydatabase dbuser;

         Enter your password as prompted.

   -  **No**

      When you create an MRS data source connection on the GaussDB(DWS) management console, the database administrator **dbadmin** automatically creates a foreign server in the default database **postgres**. If you create a foreign table in the default database **postgres** as the database administrator **dbadmin**, you need to connect to the database using the database client tool provided by GaussDB(DWS). For example, use the **gsql** client to connect to the database by running the following command:

      .. code-block::

         gsql -d postgres -h 192.168.2.30 -U dbadmin -p 8000 -W password -r

      Enter your password as prompted.

#. Run the following command to view the information about the created foreign server connected to the MRS data source:

   .. code-block::

      SELECT * FROM pg_foreign_server;

   .. note::

      You can also run the **\\desc+** command to view the information about the foreign server.

   The returned result is as follows:

   .. code-block::

                           srvname                      | srvowner | srvfdw | srvtype | srvversion | srvacl |                                                     srvoptions
      --------------------------------------------------+----------+--------+---------+------------+--------+---------------------------------------------------------------------------------------------------------------------
       gsmpp_server                                     |       10 |  13673 |         |            |        |
       gsmpp_errorinfo_server                           |       10 |  13678 |         |            |        |
       hdfs_server_8f79ada0_d998_4026_9020_80d6de2692ca |    16476 |  13685 |         |            |        | {"address=192.168.1.245:25000,192.168.1.218:25000",hdfscfgpath=/MRS/8f79ada0-d998-4026-9020-80d6de2692ca,type=hdfs}
      (3 rows)

   In the query result, each row contains the information about a foreign server. The foreign server associated with the MRS data source connection contains the following information:

   -  The value of **srvname** contains **hdfs_server** and the ID of the MRS cluster, which is the same as the MRS ID in the cluster list on the MRS management console.
   -  The **address** parameter in the **srvoptions** field contains the IP addresses and ports of the active and standby nodes in the MRS cluster.

   You can find the foreign server you want based on the above information and record the values of its **srvname** and **srvoptions**.


Creating a Foreign Table
------------------------

After :ref:`Obtaining Information About the Foreign Server Connected to the MRS Data Source <en-us_topic_0000001146240997__en-us_topic_0000001082927067_en-us_topic_0109259517_en-us_topic_0101477886_section1760214326239>` and :ref:`Obtaining the HDFS Path of the MRS Data Source <en-us_topic_0000001146240997__en-us_topic_0000001082927067_en-us_topic_0109259517_en-us_topic_0101477886_section129581728111417>` are completed, you can create a foreign table to read data from the MRS data source.

The syntax for creating a foreign table is as follows. For details, see the syntax **CREATE FOREIGN TABLE (SQL on Hadoop or OBS)**.

.. code-block::

   CREATE FOREIGN TABLE [ IF NOT EXISTS ] table_name
   ( [ { column_name type_name
       [ { [CONSTRAINT constraint_name] NULL |
       [CONSTRAINT constraint_name] NOT NULL |
         column_constraint [...]} ] |
         table_constraint [, ...]} [, ...] ] )
       SERVER dfs_server
       OPTIONS ( { option_name ' value ' } [, ...] )
       DISTRIBUTE BY {ROUNDROBIN | REPLICATION}
       [ PARTITION BY ( column_name ) [ AUTOMAPPED ] ] ;

For example, when creating a foreign table named *foreign\_product_info*, set parameters in the syntax as follows:

-  **table_name**

   Mandatory. This parameter specifies the name of the foreign table to be created.

-  Table column definitions

   -  **column_name**: specifies the name of a column in the foreign table.
   -  **type_name**: specifies the data type of the column.

   Multiple columns are separate by commas (,).

   The number of columns and column types in the foreign table must be the same as those in the data stored on MRS. Learn :ref:`Data Type Conversion <en-us_topic_0000001146240997__en-us_topic_0000001082927067_en-us_topic_0109259517_en-us_topic_0101477886_section185347544812>` before defining column data types.

-  **SERVER dfs_server**

   This parameter specifies the foreign server name of the foreign table. This server must exist. The foreign table can read data from an MRS cluster by configuring the foreign server and connecting to the MRS data source.

   Enter the value of the **srvname** field queried in :ref:`Obtaining Information About the Foreign Server Connected to the MRS Data Source <en-us_topic_0000001146240997__en-us_topic_0000001082927067_en-us_topic_0109259517_en-us_topic_0101477886_section1760214326239>`.

-  **OPTIONS** parameters

   These are parameters associated with the foreign table. The key parameters are as follows:

   -  **format**: This parameter is mandatory. The value can only be **orc**. It specifies the format of the source data file. Only Hive ORC files are supported.

   -  **foldername**: This parameter is mandatory. It specifies the HDFS directory for storing data or data file path.

      If the MRS analysis cluster has enabled Kerberos authentication, ensure that the MRS user having the MRS data source connection has the read and write permissions for the directory.

      Follow the steps in :ref:`Obtaining the HDFS Path of the MRS Data Source <en-us_topic_0000001146240997__en-us_topic_0000001082927067_en-us_topic_0109259517_en-us_topic_0101477886_section129581728111417>` to obtain the HDFS path, which is the value of parameter **foldername**.

   -  **encoding**: This parameter is optional. It specifies the encoding format of a source data file in the foreign table. Its default value is **utf8**.

   -  **DISTRIBUTE BY**

      This parameter specifies the data read mode for the foreign table. There are two read modes supported. In this example, **ROUNDROBIN** is selected.

      -  **ROUNDROBIN**: When a foreign table reads data from the data source, each node in a GaussDB(DWS) cluster randomly reads some data and integrates the random data to a complete data set.
      -  **REPLICATION**: When a foreign table reads data from the data source, each node in the GaussDB(DWS) cluster reads a complete data set.

   -  Other parameters in the syntax

      Other parameters are optional. You can set them as required. In this example, you do not need to set these parameters.

Based on the above settings, the foreign table is created using the following statements:

.. code-block::

   DROP FOREIGN TABLE IF EXISTS foreign_product_info;

   CREATE FOREIGN TABLE foreign_product_info
   (
       product_price                integer        ,
       product_id                   char(30)       ,
       product_time                 date           ,
       product_level                char(10)       ,
       product_name                 varchar(200)   ,
       product_type1                varchar(20)    ,
       product_type2                char(10)       ,
       product_monthly_sales_cnt    integer        ,
       product_comment_time         date           ,
       product_comment_num          integer        ,
       product_comment_content      varchar(200)
   ) SERVER hdfs_server_8f79ada0_d998_4026_9020_80d6de2692ca
   OPTIONS (
   format 'orc',
   encoding 'utf8',
   foldername '/user/hive/warehouse/demo.db/product_info_orc/'
   )
   DISTRIBUTE BY ROUNDROBIN;

.. _en-us_topic_0000001146240997__en-us_topic_0000001082927067_en-us_topic_0109259517_en-us_topic_0101477886_section185347544812:

Data Type Conversion
--------------------

Data is imported to Hive/Spark and then stored on HDFS in ORC format. Actually, GaussDB(DWS) reads ORC files on HDFS, and queries and analyzes data in these files.

Data types supported by Hive/Spark are different from those supported by GaussDB(DWS). Therefore, you need to learn the mapping between them. :ref:`Table 1 <en-us_topic_0000001146240997__en-us_topic_0000001082927067_en-us_topic_0109259517_en-us_topic_0101477886_table1410311611489>` describes the mapping in detail.

.. _en-us_topic_0000001146240997__en-us_topic_0000001082927067_en-us_topic_0109259517_en-us_topic_0101477886_table1410311611489:

.. table:: **Table 1** Data type mapping

   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | Type                                   | Column Type Supported by an HDFS/OBS Foreign Table of GaussDB(DWS) | Column Type Supported by a Hive Table                 | Column Type Supported by a Spark Table |
   +========================================+====================================================================+=======================================================+========================================+
   | Integer in two bytes                   | SMALLINT                                                           | SMALLINT                                              | SMALLINT                               |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | Integer in four bytes                  | INTEGER                                                            | INT                                                   | INT                                    |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | Integer in eight bytes                 | BIGINT                                                             | BIGINT                                                | BIGINT                                 |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | Single-precision floating point number | FLOAT4 (REAL)                                                      | FLOAT                                                 | FLOAT                                  |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | Double-precision floating point number | FLOAT8(DOUBLE PRECISION)                                           | DOUBLE                                                | FLOAT                                  |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | Scientific data type                   | DECIMAL[p (,s)]                                                    | DECIMAL                                               | DECIMAL                                |
   |                                        |                                                                    |                                                       |                                        |
   |                                        | The maximum precision can reach up to 38.                          | The maximum precision can reach up to 38 (Hive 0.11). |                                        |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | Date type                              | DATE                                                               | DATE                                                  | DATE                                   |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | Time type                              | TIMESTAMP                                                          | TIMESTAMP                                             | TIMESTAMP                              |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | BOOLEAN type                           | BOOLEAN                                                            | BOOLEAN                                               | BOOLEAN                                |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | CHAR type                              | CHAR(n)                                                            | CHAR (n)                                              | STRING                                 |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | VARCHAR type                           | VARCHAR(n)                                                         | VARCHAR (n)                                           | VARCHAR (n)                            |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
   | String                                 | TEXT(CLOB)                                                         | STRING                                                | STRING                                 |
   +----------------------------------------+--------------------------------------------------------------------+-------------------------------------------------------+----------------------------------------+
