:original_name: dws_04_0163.html

.. _dws_04_0163:

Creating a Foreign Table
========================

After operations in :ref:`Creating a Foreign Server <dws_04_0162>` are complete, create an HDFS write-only foreign table in the GaussDB(DWS) database to access data stored in HDFS. The foreign table is write-only and can be used only for data export.

The syntax for creating a foreign table is as follows. For details, see **CREATE FOREIGN TABLE (SQL on Hadoop or OBS)**.

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

For example, when creating a foreign table *product_info_ext_obs*, configure the parameters in the syntax as follows.

-  **table_name**

   Specifies the name of the foreign table.

-  **Table column definitions**

   -  **column_name**: specifies the name of a column in the foreign table.
   -  **type_name**: specifies the data type of the column.

   Multiple columns are separate by commas (,).

-  **SERVER dfs_server**

   Specifies the foreign server name of the foreign table. This server must exist. The foreign table connects to OBS/HDFS to read data through the foreign server.

   Enter the name of the foreign server created in :ref:`Creating a Foreign Server <dws_04_0162>`.

-  **OPTIONS parameters**

   These parameters are associated with the foreign table. The key parameters are as follows:

   -  **format**: specifies the format of the exported data file. The ORC format is supported.

   -  **foldername**: specifies the directory of the data source file in the foreign table, that is, the corresponding file directory in HDFS. This parameter is mandatory for write-only foreign tables and optional for read-only foreign tables.

   -  **encoding**: specifies the encoding format of the data source file in the foreign table. The default value is **utf8**.

   -  filesize

      (Optional) Specifies the file size of a write-only foreign table. If this parameter is not specified, the file size in the distributed file system is used by default. This syntax is available only for the write-only foreign table.

      Value range: an integer ranging from 1 to 1024

      .. note::

         The **filesize** parameter is valid only for the write-only HDFS foreign table in ORC format.

   -  compression

      (Optional) Specifies the compression mode of ORC files. This syntax is available only for the write-only foreign table.

      Value range: **zlib**, **snappy**, and **lz4**. The default value is **snappy**.

   -  version

      (Optional) Specifies the ORC version number. This syntax is available only for the write-only foreign table.

      Value range: Only **0.12** is supported. The default value is **0.12**.

   -  dataencoding

      (Optional) Specifies the data encoding of the data table to be exported when the database encoding is different from the data encoding of the data table. For example, the database encoding is Latin-1, but the data encoding of the exported data table is in UTF-8 format. If this parameter is not specified, the database encoding is used by default. This syntax is valid only for the write-only HDFS foreign table.

      Value range: data encoding types supported by the database encoding

      .. note::

         The **dataencoding** parameter is valid only for the write-only HDFS foreign table in ORC format.

-  **Other parameters in the syntax**

   Other parameters are optional. You can configure them as required. In this example, you do not need to configure these parameters. For details, see **CREATE FOREIGN TABLE (SQL on Hadoop or OBS)**.

   Based on the preceding settings, the command for creating the foreign table is as follows:

   ::

      DROP FOREIGN TABLE IF EXISTS product_info_ext_obs;

      -- Create an OBS foreign table that does not contain partition columns. The foreign server associated with the table is hdfs_server, the format of the file on HDFS corresponding to the table is ORC, and the data storage path on OBS is /user/hive/warehouse/product_info_orc/.

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
      ) SERVER hdfs_server
      OPTIONS (
      format 'orc',
      foldername '/user/hive/warehouse/product_info_orc/',
         compression 'snappy',
          version '0.12'
      ) Write Only;
