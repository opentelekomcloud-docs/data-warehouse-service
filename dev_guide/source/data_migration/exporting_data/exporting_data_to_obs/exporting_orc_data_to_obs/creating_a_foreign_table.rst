:original_name: dws_04_0260.html

.. _dws_04_0260:

Creating a Foreign Table
========================

After operations in :ref:`Creating a Foreign Server <en-us_topic_0000001717097364>` are complete, create an OBS/HDFS write-only foreign table in the GaussDB(DWS) database to access data stored in OBS/HDFS. The foreign table is write-only and can be used only for data export.

The syntax for creating a foreign table is as follows:

::

   CREATE FOREIGN TABLE [ IF NOT EXISTS ] table_name
   ( [ { column_name type_name
       [ { [CONSTRAINT constraint_name] NULL |
       [CONSTRAINT constraint_name] NOT NULL |
         column_constraint [...]} ] |
         table_constraint [, ...]} [, ...] ] )
       SERVER dfs_server
       OPTIONS ( { option_name ' value ' } [, ...] )
       [ {WRITE ONLY }]
       DISTRIBUTE BY {ROUNDROBIN | REPLICATION}
       [ PARTITION BY ( column_name ) [ AUTOMAPPED ] ] ;

For example, when creating a foreign table **product_info_ext_obs**, configure the parameters in the syntax as follows.

-  **table_name**

   Specifies the name of the foreign table to be created.

-  **Table column definitions**

   -  **column_name**: specifies the name of a column in the foreign table.
   -  **type_name**: specifies the data type of the column.

   Multiple columns are separate by commas (,).

-  **SERVER dfs_server**

   Specifies the foreign server name of the foreign table. This server must exist. The foreign table connects to OBS/HDFS to read data through the foreign server.

   Enter the name of the foreign server created by following steps in :ref:`Creating a Foreign Server <en-us_topic_0000001764817361>`.

-  **OPTIONS parameters**

   These are parameters associated with the foreign table. The key parameters are as follows:

   -  **format**: specifies the format of the exported data file. The ORC format is supported.

   -  **foldername**: (mandatory) specifies the data source file directory in the foreign table. OBS: specifies the OBS path of the data source file. You only need to enter **/**\ *Bucket name*\ **/**\ *Folder directory level*\ **/**. HDFS: specifies the path in the HDFS file system. This parameter is mandatory for the write-only foreign table.

   -  **encoding**: specifies the encoding of the data source file in the foreign table. The default value is **utf8**.

   -  **filesize**

      Specifies the file size of a write-only foreign table, in MB. If this parameter is not specified, the file size in the distributed file system configuration is used by default. This syntax is available only for the write-only foreign table.

      Value range: an integer ranging from 1 to 1024

      .. note::

         The **filesize** parameter is valid only for the ORC-formatted write-only HDFS foreign table.

   -  **compression**

      (Optional) Specifies the compression mode of ORC files. This syntax is available only for the write-only foreign table.

      Value range: **zlib**, **snappy**, and **lz4** The default value is **snappy**.

   -  **version**

      (Optional) Specifies the ORC version number. This syntax is available only for the write-only foreign table.

      Value range: Only **0.12** is supported. The default value is **0.12**.

   -  **dataencoding**

      (Optional) Specifies the data code of the data table to be exported when the database code is different from the data code of the data table. For example, the database code is Latin-1, but the data in the exported data table is in UTF-8 format. If this parameter is not specified, the database encoding format is used by default. This syntax is valid only for the write-only HDFS foreign table.

      Value range: data code types supported by the database encoding

      .. note::

         The **dataencoding** parameter is valid only for the ORC-formatted write-only HDFS foreign table.

-  **Other parameters in the syntax**

   Other parameters are optional and can be configured as required. In this example, they do not need to be configured.

   Based on the preceding settings, the command for creating the foreign table is as follows:

   ::

      DROP FOREIGN TABLE IF EXISTS product_info_ext_obs;

      -- Create an OBS foreign table that does not contain partition columns. The foreign server associated with the table is obs_server, the file format on OBS corresponding to the table is ORC, and the data storage path on OBS is/mybucket/data/.

      CREATE FOREIGN TABLE product_info_ext_obs
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
      ) SERVER obs_server
      OPTIONS (
      format 'orc',
      foldername '/mybucket/demo.db/product_info_orc/',
         compression 'snappy',
          version '0.12'
      ) Write Only;
