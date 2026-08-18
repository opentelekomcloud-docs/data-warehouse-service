:original_name: dws_04_0245.html

.. _dws_04_0245:

Creating a Foreign Table
========================

After performing steps in :ref:`Creating a Foreign Server <dws_04_0244>`, create an OBS foreign table in the DWS database to access the data stored in OBS. An OBS foreign table is read-only. It can only be queried using **SELECT**. The operations vary according to the cluster version.

-  For 8.2.0 and later versions, you can refer to the operations described in "Managing OBS Data Sources" in *Data Warehouse Service (DWS) User Guide*.
-  For versions earlier than 8.2.0, refer to the operations outlined in the following section.

.. caution::

   If error message "permission denied for foreign server xxx" is displayed during foreign table creation, the current user does not have permissions on the foreign server. To grant the user the permissions on the foreign server, run the following command (replace *obs_server* with the foreign server name and **u1** with the current user name):

   .. code-block::

      GRANT usage ON foreign server obs_server TO u1;


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

For example, when creating a foreign table named **product_info_ext_obs**, configure the parameters in the syntax as described in :ref:`Table 1 <en-us_topic_0000001811490541__table118511415619>`.

.. _en-us_topic_0000001811490541__table118511415619:

.. table:: **Table 1** Parameters for creating a foreign table

   +------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | Parameter                          | Description                                                                                                                                                                                                                                                                                                                                                                                        | Example                |
   +====================================+====================================================================================================================================================================================================================================================================================================================================================================================================+========================+
   | **table_name**                     | Specifies the name of the foreign table.                                                                                                                                                                                                                                                                                                                                                           | *product_info_ext_obs* |
   +------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | **column_nam**                     | Specifies the column name in the foreign table. Multiple columns are separate by commas (,). The number of columns and column types in the foreign table must be the same as those in the data stored on OBS.                                                                                                                                                                                      | product_price          |
   +------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | **type_name**                      | Specifies the data type of the column.                                                                                                                                                                                                                                                                                                                                                             | integer                |
   +------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | **SERVER dfs_server**              | Specifies the foreign server name of the foreign table. This server must exist. The foreign server connects to OBS to read data by setting its foreign server. Enter the name of the foreign server created by following steps in :ref:`Creating a Foreign Server <dws_04_0244>`.                                                                                                                  | *obs_server*           |
   +------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | **OPTIONS**                        | Specifies various parameters of the foreign table data. The key parameters are as follows:                                                                                                                                                                                                                                                                                                         | format 'orc'           |
   |                                    |                                                                                                                                                                                                                                                                                                                                                                                                    |                        |
   |                                    | -  **format**: indicates the file format on OBS. The ORC, CarbonData, and Parquet formats are supported.                                                                                                                                                                                                                                                                                           |                        |
   |                                    |                                                                                                                                                                                                                                                                                                                                                                                                    |                        |
   |                                    | -  **foldername** (mandatory): indicates the OBS path of the data source file. You only need to enter **/**\ *Bucket name*\ **/**\ *Folder directory level*\ **/**.                                                                                                                                                                                                                                |                        |
   |                                    |                                                                                                                                                                                                                                                                                                                                                                                                    |                        |
   |                                    |    To obtain the complete OBS path of the data source file, see :ref:`2 <en-us_topic_0000001811609589__en-us_topic_0000001188482188_en-us_topic_0000001145410931_en-us_topic_0102810712_li123314509351>` in :ref:`Preparing Data on OBS <dws_04_0243>`. The path is the endpoint of OBS.                                                                                                           |                        |
   |                                    |                                                                                                                                                                                                                                                                                                                                                                                                    |                        |
   |                                    | -  **totalrows** (optional): It does not indicate the total rows of the imported data. Because OBS may store many files, it is slow to analyze data. This parameter allows you to set an estimated value so that the optimizer can estimate the table size according to the value. Generally, query efficiency is relatively high when the estimated value is almost the same as the actual value. |                        |
   |                                    |                                                                                                                                                                                                                                                                                                                                                                                                    |                        |
   |                                    | -  **encoding**: indicates the encoding format of the data source file in the foreign table. The default value is **utf8**. This parameter is mandatory for OBS foreign tables.                                                                                                                                                                                                                    |                        |
   +------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | **DISTRIBUTE BY**                  | (Mandatory) Specifies the distribution mode. The value can be **ROUNDROBIN** or **REPLICATION**. The default value is **ROUNDROBIN**.                                                                                                                                                                                                                                                              | ROUNDROBIN             |
   |                                    |                                                                                                                                                                                                                                                                                                                                                                                                    |                        |
   |                                    | **ROUNDROBIN** indicates that when a foreign table reads data from the data source, each node in the DWS cluster randomly reads some data and integrates the random data to a complete data set.                                                                                                                                                                                                   |                        |
   |                                    |                                                                                                                                                                                                                                                                                                                                                                                                    |                        |
   |                                    | **REPLICATION** indicates that when a foreign table reads data from the data source, the DWS cluster selects a node to read all data. This is because each data node has complete table data.                                                                                                                                                                                                      |                        |
   +------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | **Other parameters in the syntax** | Other parameters are optional and can be configured as required. In this example, they do not need to be configured.                                                                                                                                                                                                                                                                               | ``-``                  |
   +------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+

Based on the preceding settings, the command for creating the foreign table is as follows:

Create an OBS foreign table that does not contain partition columns. The foreign server associated with the table is **obs_server**, the file format on OBS corresponding to the table is ORC, and the data storage path on OBS is **/mybucket/demo.db/product_info_orc/**.

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
