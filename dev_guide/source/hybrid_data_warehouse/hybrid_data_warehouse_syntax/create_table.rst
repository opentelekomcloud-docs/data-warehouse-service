:original_name: dws_04_1029.html

.. _dws_04_1029:

CREATE TABLE
============

Function
--------

Create an HStore table in the current database. The table will be owned by the user who created it.

In a hybrid data warehouse, you can use DDL statements to create HStore tables. To create an HStore table, set **enable_hstore** to **true** and set **orientation** to **column**.

Precautions
-----------

-  To create an HStore table, you must have the **USAGE** permission on schema cstore.
-  The table-level parameters **enable_delta** and **enable_hstore** cannot be enabled at the same time. The parameter **enable_delta** is used to enable delta for common column-store tables and conflicts with **enable_hstore**.
-  Each HStore table is bound to a delta table. The OID of the delta table is recorded in the **reldeltaidx** field in **pg_class**. (The **reldelta** field is used by the delta table of the column-store table).

Syntax
------

::

   CREATE TABLE [ IF NOT EXISTS ] table_name
   ({ column_name data_type
       | LIKE source_table [like_option [...] ] }
   }
       [, ... ])
   [ WITH ( {storage_parameter = value}  [, ... ] ) ]
   [ TABLESPACE tablespace_name ]
   [ DISTRIBUTE BY  HASH ( column_name [,...])]
   [ TO { GROUP groupname | NODE ( nodename [, ... ] ) } ]
   [ PARTITION BY {
           {RANGE (partition_key) ( partition_less_than_item [, ... ] )}
    } [ { ENABLE | DISABLE } ROW MOVEMENT ] ];
   The options for LIKE are as follows:
   { INCLUDING | EXCLUDING } { DEFAULTS | CONSTRAINTS | INDEXES | STORAGE | COMMENTS | PARTITION | RELOPTIONS | DISTRIBUTION | ALL }

Differences Between Delta Tables
--------------------------------

.. table:: **Table 1** Differences between the delta tables of HStore and column-store tables

   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Type            | Column-Store Delta Table                                                                                                                                                                                                                                         | HStore Delta Table                                                                                                                                                                               |
   +=================+==================================================================================================================================================================================================================================================================+==================================================================================================================================================================================================+
   | Table structure | Same as that defined for the column-store primary table.                                                                                                                                                                                                         | Different from that defined for the primary table.                                                                                                                                               |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Function        | Used to temporarily store a small batch of inserted data. After the data size reaches the threshold, the data will be merged to the primary table. In this way, data will not be directly inserted to the primary table or generate a large number of small CUs. | Persistently stores UPDATE, DELETE, and INSERT information. It is used to restore the memory structure that manages concurrent updates, such as the memory update chain, in the case of a fault. |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Weakness        | If data is not merged in a timely manner, the delta table will grow large and affect query performance. In addition, the table cannot solve lock conflicts during concurrent updates.                                                                            | The merge operation depends on the background AUTOVACUUM.                                                                                                                                        |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Parameters
----------

-  **IF NOT EXISTS**

   If **IF NOT EXISTS** is specified, a table will be created if there is no table using the specified name. If there is already a table using the specified name, no error will be reported. A message will be displayed indicating that the table already exists, and the database will skip table creation.

-  **table_name**

   Specifies the name of the table to be created.

   The table name can contain a maximum of 63 characters, including letters, digits, underscores (_), dollar signs ($), and number signs (#). It must start with a letter or underscore (_).

-  **column_name**

   Specifies the name of a column to be created in the new table.

   The column name can contain a maximum of 63 characters, including letters, digits, underscores (_), dollar signs ($), and number signs (#). It must start with a letter or underscore (_).

-  **data_type**

   Specifies the data type of the column.

-  **LIKE source_table [ like_option ... ]**

   Specifies a table from which the new table automatically copies all column names and their data types.

   The new table and the original table are decoupled after creation is complete. Changes to the original table will not be applied to the new table, and scans on the original table will not be performed on the data of the new table.

   Columns copied by **LIKE** are not merged with the same name. If the same name is specified explicitly or in another **LIKE** clause, an error will be reported.

   HStore tables can be inherited only from HStore tables.

-  **WITH ( { storage_parameter = value } [, ... ] )**

   Specifies an optional storage parameter for a table.

   -  ORIENTATION

      Specifies the storage mode (time series, row-store, or column-store) of table data. This parameter cannot be modified once it is set. For HStore tables, use the column storage mode and set **enable_hstore** to **on**.

      Options:

      -  **TIMESERIES** indicates that the data is stored in time series.
      -  **COLUMN** indicates that the data is stored in columns.
      -  **ROW** indicates that table data is stored in rows.

      Default value: **ROW**

   -  COMPRESSION

      Specifies the compression level of the table data. It determines the compression ratio and time. Generally, a higher compression level indicates a higher compression ratio and a longer compression time, and vice versa. The actual compression ratio depends on the distribution characteristics of loading table data.

      Options:

      -  The valid values for HStore tables and column-store tables are **YES**/**NO** and **LOW**/**MIDDLE**/**HIGH**, and the default is **LOW**.
      -  The valid values for row-store tables are **YES** and **NO**, and the default is **NO**.

   -  COMPRESSLEVEL

      Specifies table data compression rate and duration at the same compression level. This divides a compression level into sub-levels, providing you with more choices for compression ratio and duration. As the value becomes greater, the compression rate becomes higher and duration longer at the same compression level. The parameter is only valid for time series tables and column-store tables.

      Value range: 0 to 3

      Default value: **0**

   -  MAX_BATCHROW

      Specifies the maximum number of rows in a storage unit during data loading. The parameter is only valid for time series tables and column-store tables.

      Value range: 10000 to 60000

      Default value: **60000**

   -  PARTIAL_CLUSTER_ROWS

      Specifies the number of records to be partially clustered for storage during data loading. The parameter is only valid for time series tables and column-store tables.

      Value range: 600000 to 2147483647

   -  enable_delta

      Specifies whether to enable delta tables in column-store tables. This parameter cannot be enabled for HStore tables.

      Default value: **off**

   -  SUB_PARTITION_COUNT

      Specifies the number of level-2 partitions. This parameter specifies the number of level-2 partitions during data import. This parameter is configured during table creation and cannot be modified after table creation. You are not advised to set the default value, which may affect the import and query performance.

      Value range: 1 to 1024

      Default value: **32**

   -  DELTAROW_THRESHOLD

      Specifies the maximum number of rows (**SUB_PARTITION_COUNT** x **DELTAROW_THRESHOLD**) to be imported to the delta table.

      Value range: 0 to 60000

      Default value: **60000**

   -  COLVERSION

      Specifies the version of the storage format. HStore tables support only version 2.0.

      Options:

      **1.0**: Each column in a column-store table is stored in a separate file. The file name is **relfilenode.C1.0**, **relfilenode.C2.0**, **relfilenode.C3.0**, or similar.

      **2.0**: All columns of a column-store table are combined and stored in a file. The file is named **relfilenode.C1.0**.

      Default value: **2.0**

   -  DISTRIBUTE BY

      Specifies how the table is distributed or replicated between DNs.

      Options:

      **HASH (column_name)**: Each row of the table will be placed into all the DNs based on the hash value of the specified column.

   -  TO { GROUP groupname \| NODE ( nodename [, ... ] ) }

      **TO GROUP** specifies the Node Group in which the table is created. Currently, it cannot be used for HDFS tables. **TO NODE** is used for internal scale-out tools.

   -  PARTITION BY

      Specifies the initial partition of an HStore table.

Example
-------

Create a simple HStore table.

.. code-block::

   CREATE TABLE warehouse_t1
   (
       W_WAREHOUSE_SK            INTEGER               NOT NULL,
       W_WAREHOUSE_ID            CHAR(16)              NOT NULL,
       W_WAREHOUSE_NAME          VARCHAR(20)                   ,
       W_WAREHOUSE_SQ_FT         INTEGER                       ,
       W_STREET_NUMBER           CHAR(10)                      ,
       W_STREET_NAME             VARCHAR(60)                   ,
       W_STREET_TYPE             CHAR(15)                      ,
       W_SUITE_NUMBER            CHAR(10)                      ,
       W_CITY                    VARCHAR(60)                   ,
       W_COUNTY                  VARCHAR(30)                   ,
       W_STATE                   CHAR(2)                       ,
       W_ZIP                     CHAR(10)                      ,
       W_COUNTRY                 VARCHAR(20)                   ,
       W_GMT_OFFSET              DECIMAL(5,2)
   )WITH(ORIENTATION=COLUMN, ENABLE_HSTORE=ON);

   CREATE TABLE warehouse_t2 (LIKE warehouse_t1 INCLUDING ALL);
