:original_name: dws_04_0245.html

.. _dws_04_0245:

.. _en-us_topic_0000001764896585:

Creating a Foreign Table
========================

After performing steps in :ref:`Creating a Foreign Server <en-us_topic_0000001764817361>`, create an OBS foreign table in the GaussDB(DWS) database to access the data stored in OBS. An OBS foreign table is read-only. It can only be queried using **SELECT**.


Creating a Foreign Table
------------------------

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
       DISTRIBUTE BY {ROUNDROBIN | REPLICATION}
       [ PARTITION BY ( column_name ) [ AUTOMAPPED ] ] ;

For example, when creating a foreign table **product_info_ext_obs**, configure the parameters in the syntax as follows.

-  **table_name**

   Specifies the name of the foreign table to be created.

-  **Table column definitions**

   -  **column_name**: specifies the name of a column in the foreign table.
   -  **type_name**: specifies the data type of the column.

   Multiple columns are separate by commas (,).

   The number of fields and field types in the foreign table must be the same as those in the data stored on OBS.

-  **SERVER dfs_server**

   This parameter specifies the foreign server name of the foreign table. This server must exist. The foreign server connects to OBS to read data by setting its foreign server.

   Enter the name of the foreign server created by following steps in :ref:`Creating a Foreign Server <en-us_topic_0000001764817361>`.

-  **OPTIONS parameters**

   These are parameters associated with the foreign table. The key parameters are as follows:

   -  **format**: indicates the file format on OBS. The ORC and CARBONDATA formats are supported.

   -  **foldername**: This parameter is mandatory. It indicates the OBS path of the data source file. You only need to enter **/**\ *Bucket name*\ **/**\ *Folder directory level*\ **/**.

      You can perform :ref:`2 <en-us_topic_0000001717097300__en-us_topic_0000001188482188_en-us_topic_0000001145410931_en-us_topic_0102810712_li12771154711>` in :ref:`Preparing Data on OBS <en-us_topic_0000001717097300>` to obtain the complete OBS path of the data source file. The path is the endpoint of the OBS service.

   -  **totalrows**: This parameter is optional. It does not indicate the total rows of the imported data. Because OBS may store many files, it is slow to analyze data. This parameter allows you to set an estimated value so that the optimizer can estimate the table size according to the value. Generally, query efficiency is relatively high when the estimated value is almost the same as the actual value.

   -  **encoding**: encoding of data source files in foreign tables. The default value is **utf8**. This parameter is mandatory for OBS foreign tables.

-  **DISTRIBUTE BY**:

   This clause is mandatory. Currently, OBS foreign tables support only the **ROUNDROBIN** distribution mode.

   It indicates that when a foreign table reads data from the data source, each node in the GaussDB(DWS) cluster randomly reads some data and integrates the random data to a complete data set.

-  **Other parameters in the syntax**

   Other parameters are optional and can be configured as required. In this example, they do not need to be configured.

Based on the preceding settings, the command for creating the foreign table is as follows:

Create an OBS foreign table that does not contain partition columns. The foreign server associated with the table is **obs_server**, the file format on OBS corresponding to the table is ORC, and the data storage path on OBS is **/mybucket/data/**.

::

   DROP FOREIGN TABLE IF EXISTS product_info_ext_obs;
   CREATE FOREIGN TABLE product_info_ext_obs
   (
       product_price                integer        not null,
       product_id                   char(30)       not null,
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
   encoding 'utf8',
   totalrows '10'
   )
   DISTRIBUTE BY ROUNDROBIN;

Create an OBS foreign table that contains partition columns. The **product_info_ext_obs** foreign table uses the **product_manufacturer** column as the partition key. The following partition directories exist in **obs/mybucket/demo.db/product_info_orc/**:

Partition directory 1: product_manufacturer=10001

Partition directory 2: product_manufacturer=10010

Partition directory 3: product_manufacturer=10086

...

::

   DROP FOREIGN TABLE IF EXISTS product_info_ext_obs;
   CREATE FOREIGN TABLE product_info_ext_obs
   (
       product_price                integer        not null,
       product_id                   char(30)       not null,
       product_time                 date           ,
       product_level                char(10)       ,
       product_name                 varchar(200)   ,
       product_type1                varchar(20)    ,
       product_type2                char(10)       ,
       product_monthly_sales_cnt    integer        ,
       product_comment_time         date           ,
       product_comment_num          integer        ,
       product_comment_content      varchar(200)   ,
       product_manufacturer   integer
   ) SERVER obs_server
   OPTIONS (
   format 'orc',
   foldername '/mybucket/demo.db/product_info_orc/',
   encoding 'utf8',
   totalrows '10'
   )
   DISTRIBUTE BY ROUNDROBIN
   PARTITION BY (product_manufacturer) AUTOMAPPED;
