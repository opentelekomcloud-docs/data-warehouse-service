:original_name: dws_06_0177.html

.. _dws_06_0177:

CREATE TABLE
============

Function
--------

**CREATE TABLE** creates a table in the current database. The table will be owned by the user who created it.

Precautions
-----------

-  For details about the data types supported by column-store tables, see :ref:`Data Types Supported by Column-Store Tables <dws_06_0024>`.
-  It is recommended that the number of column-store and HDFS partitioned tables do not exceed 1000.
-  The primary key constraint and unique constraint in the table must contain a distribution column.
-  If an error occurs during table creation, after it is fixed, the system may fail to delete the empty disk files created before the last automatic clearance. This problem seldom occurs.
-  Column-store tables support the **PARTIAL CLUSTER KEY** and table-level primary key and unique constraints, but do not support table-level foreign key constraints.
-  Only the NULL, NOT NULL, and DEFAULT constant values can be used as column-store table column constraints.
-  Whether column-store tables support a delta table is specified by the **enable_delta** parameter. The threshold for storing data into a delta table is specified by the **deltarow_threshold** parameter.
-  Hot and cold tables support only partitioned column-store tables and depend on available OBS tablespaces.
-  Only the table-level and partition-level tablespaces of a hot or cold table can be set to general tablespaces.

Syntax
------

::

   CREATE [ [ GLOBAL | LOCAL ] { TEMPORARY | TEMP } | UNLOGGED ] TABLE [ IF NOT EXISTS ] table_name
       ({ column_name data_type [ compress_mode ] [ COLLATE collation ] [ column_constraint [ ... ] ]
           | table_constraint
           | LIKE source_table [ like_option [...] ] }
           [, ... ])
       [ WITH ( {storage_parameter = value} [, ... ] ) ]
       [ ON COMMIT { PRESERVE ROWS | DELETE ROWS | DROP } ]
       [ COMPRESS | NOCOMPRESS ]

       [ DISTRIBUTE BY { REPLICATION | { HASH ( column_name [,...] ) } } ]
       [ TO { GROUP groupname | NODE ( nodename [, ... ] ) } ];

-  **column_constraint** is as follows:

   ::

      [ CONSTRAINT constraint_name ]
      { NOT NULL |
        NULL |
        CHECK ( expression ) |
        DEFAULT default_expr |
        UNIQUE index_parameters |
        PRIMARY KEY index_parameters }
      [ DEFERRABLE | NOT DEFERRABLE | INITIALLY DEFERRED | INITIALLY IMMEDIATE ]

-  **compress_mode** of a column is as follows:

   ::

      { DELTA | PREFIX | DICTIONARY | NUMSTR | NOCOMPRESS }

-  **table_constraint** is as follows:

   ::

      [ CONSTRAINT constraint_name ]
      { CHECK ( expression ) |
        UNIQUE ( column_name [, ... ] ) index_parameters |
        PRIMARY KEY ( column_name [, ... ] ) index_parameters |
        PARTIAL CLUSTER KEY ( column_name [, ... ] ) }
      [ DEFERRABLE | NOT DEFERRABLE | INITIALLY DEFERRED | INITIALLY IMMEDIATE ]

-  **like_option** is as follows:

   ::

      { INCLUDING | EXCLUDING } { DEFAULTS | CONSTRAINTS | INDEXES | STORAGE | COMMENTS | PARTITION | RELOPTIONS | DISTRIBUTION | DROPCOLUMNS | ALL }

-  **index_parameters** is as follows:

   ::

      [ WITH ( {storage_parameter = value} [, ... ] ) ]

Parameter Description
---------------------

-  **UNLOGGED**

   If this key word is specified, the created table is not a log table. Data written to unlogged tables is not written to the write-ahead log, which makes them considerably faster than ordinary tables. However, an unlogged table is automatically truncated after a crash or unclean shutdown, incurring data loss risks. The contents of an unlogged table are also not replicated to standby servers. Any indexes created on an unlogged table are not automatically logged as well.

   Usage scenario: Unlogged tables do not ensure safe data. Users can back up data before using unlogged tables; for example, users should back up the data before a system upgrade.

   Troubleshooting: If data is missing in the indexes of unlogged tables due to some unexpected operations such as an unclean shutdown, users should re-create the indexes with errors.

-  **GLOBAL \| LOCAL**

   When creating a temporary table, you can specify the **GLOBAL** or **LOCAL** keyword before **TEMP** or **TEMPORARY**. Currently, the two keywords are used to be compatible with the SQL standard. GaussDB(DWS) will create a local temporary table regardless of whether **GLOBAL** or **LOCAL** is specified.

-  **TEMPORARY \| TEMP**

   If **TEMP** or **TEMPORARY** is specified, the created table is a temporary table. Temporary tables are automatically dropped at the end of a session, or optionally at the end of the current transaction. Therefore, apart from CN and other CN errors connected by the current session, you can still create and use temporary table in the current session. Temporary tables are created only in the current session. If a DDL statement involves operations on temporary tables, a DDL error will be generated. Therefore, you are not advised to perform operations on temporary tables in DDL statements. **TEMP** is equivalent to **TEMPORARY**.

   .. important::

      -  Temporary tables are visible to the current session through schema of the **pg_temp** start. Users should not delete schema started with **pg_temp**, **pg_toast_temp**.
      -  If **TEMPORARY** or **TEMP** is not specified when you create a table and the schema of the specified table starts with **pg_temp\_**, the table is created as a temporary table.

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

-  **compress_mode**

   Specifies the compress option of the table, only available for row-store table. The option specifies the algorithm preferentially used by table columns.

   Value range: DELTA, PREFIX, DICTIONARY, NUMSTR, NOCOMPRESS

-  **COLLATE collation**

   Assigns a collation to the column (which must be of a collatable data type). If no collation is specified, the default collation is used.

-  **LIKE source_table [ like_option ... ]**

   Specifies a table from which the new table automatically copies all column names, their data types, and their not-null constraints.

   The new table and the source table are decoupled after creation is complete. Changes to the source table will not be applied to the new table, and it is not possible to include data of the new table in scans of the source table.

   Columns and constraints copied by **LIKE** are not merged with the same name. If the same name is specified explicitly or in another **LIKE** clause, an error is reported.

   -  The default expressions are copied from the source table to the new table only if **INCLUDING DEFAULTS** is specified. The default behavior is to exclude default expressions, resulting in the copied columns in the new table having default values **NULL**.
   -  The **CHECK** constraints are copied from the source table to the new table only when **INCLUDING CONSTRAINTS** is specified. Other types of constraints are never copied to the new table. **NOT NULL** constraints are always copied to the new table. These rules also apply to column constraints and table constraints.
   -  Any indexes on the source table will not be created on the new table, unless the **INCLUDING INDEXES** clause is specified.
   -  STORAGE settings for the copied column definitions are copied only if **INCLUDING STORAGE** is specified. The default behavior is to exclude **STORAGE** settings.
   -  If **INCLUDING COMMENTS** is specified, comments for the copied columns, constraints, and indexes are copied. The default behavior is to exclude comments.
   -  If **INCLUDING PARTITION** is specified, the partition definitions of the source table are copied to the new table, and the new table no longer uses the **PARTITION BY** clause. The default behavior is to exclude partition definition of the source table.
   -  If **INCLUDING RELOPTIONS** is specified, the storage parameter (**WITH** clause of the source table) of the source table is copied to the new table. The default behavior is to exclude partition definition of the storage parameter of the source table.
   -  If **INCLUDING DISTRIBUTION** is specified, the distribution information of the source table is copied to the new table, including distribution type and column, and the new table no longer use the **DISTRIBUTE BY** clause. The default behavior is to exclude distribution information of the source table.
   -  If **INCLUDING DROPCOLUMNS** is specified, the deleted column information in the source table is copied to the new table. By default, the deleted column information of the source table is not copied.
   -  **INCLUDING ALL** contains the meaning of **INCLUDING DEFAULTS**, **INCLUDING CONSTRAINTS**, **INCLUDING INDEXES**, **INCLUDING STORAGE**, **INCLUDING COMMENTS**, **INCLUDING PARTITION**, **INCLUDING RELOPTIONS**, **INCLUDING DISTRIBUTION**, and **INCLUDING DROPCOLUMNS**.
   -  If EXCLUDING is specified, the specified parameters are not included.
   -  For an OBS hot or cold table, all partitions of the new table are local hot partitions after **INCLUDING PARTITION** is specified.

   .. important::

      -  If the source table contains a sequence with the SERIAL, BIGSERIAL, or SMALLSERIAL data type, or a column in the source table is a sequence by default and the sequence is created for this table by using **CREATE SEQUENCE...** **OWNED BY**, these sequences will not be copied to the new table, and another sequence specific to the new table will be created. This is different from earlier versions. To share a sequence between the source table and new table, create a shared sequence (do not use **OWNED BY**) and set a column in the source table to this sequence.
      -  You are not advised to set a column in the source table to the sequence specific to another table especially when the table is distributed in specific Node Groups, because doing so may result in **CREATE TABLE ... LIKE** execution failures. In addition, doing so may cause the sequence to become invalid in the source sequence because the sequence will also be deleted from the source table when it is deleted from the table that the sequence is specific to. To share a sequence among multiple tables, you are advised to create a shared sequence for them.

-  **WITH ( { storage_parameter = value } [, ... ] )**

   Specifies an optional storage parameter for a table or an index.

   .. note::

      Using Numeric of any precision to define column, specifies precision p and scale s. When precision and scale are not specified, the input will be displayed.

   The description of parameters is as follows:

   -  FILLFACTOR

      The fillfactor of a table is a percentage between 10 and 100. 100 (complete packing) is the default value. When a smaller fillfactor is specified, **INSERT** operations pack table pages only to the indicated percentage. The remaining space on each page is reserved for updating rows on that page. This gives **UPDATE** a chance to place the updated copy of a row on the same page, which is more efficient than placing it on a different page. For a table whose records are never updated, setting the fillfactor to 100 (complete packing) is the appropriate choice, but in heavily updated tables smaller fillfactors are appropriate. The parameter has no meaning for column-based tables.

      Value range: 10-100

   -  ORIENTATION

      Specifies the storage mode (row-store, column-store) for table data. This parameter cannot be modified once it is set.

      Valid value:

      -  **ROW** indicates that table data is stored in rows.

         **ROW** applies to OLTP service, which has many interactive transactions. An interaction involves many columns in the table. Using ROW can improve the efficiency.

      -  **COLUMN** indicates that the data is stored in columns.

         **COLUMN** applies to the data warehouse service, which has a large amount of aggregation computing, and involves a few column operations.

      Default value:

      If an ordinary tablespace is specified, the default is **ROW**.

   -  COMPRESSION

      Specifies the compression level of the table data. It determines the compression ratio and time. Generally, the higher the level of compression, the higher the ratio, the longer the time, and the lower the level of compression, the lower the ratio, the shorter the time. The actual compression ratio depends on the distribution characteristics of loading table data.

      Valid value:

      -  The valid values for column-store tables are **YES**/**NO** and **LOW**/**MIDDLE**/**HIGH**, and the default is **LOW**.
      -  The valid values for row-store tables are **YES** and **NO**, and the default is **NO**.

         .. note::

            -  The row-store table compression function is not put into commercial use. To use this function, contact technical support.

      GaussDB(DWS) provides the following compression algorithms:

      .. table:: **Table 1** Compression algorithms for column-based storage

         +-------------+--------------------------------------------------------+--------------------------------------+---------------------------------------------------------+
         | COMPRESSION | NUMERIC                                                | STRING                               | INT                                                     |
         +=============+========================================================+======================================+=========================================================+
         | LOW         | Delta compression + RLE compression                    | LZ4 compression                      | Delta compression (RLE is optional.)                    |
         +-------------+--------------------------------------------------------+--------------------------------------+---------------------------------------------------------+
         | MIDDLE      | Delta compression + RLE compression + LZ4 compression  | dict compression or LZ4 compression  | Delta compression or LZ4 compression (RLE is optional)  |
         +-------------+--------------------------------------------------------+--------------------------------------+---------------------------------------------------------+
         | HIGH        | Delta compression + RLE compression + zlib compression | dict compression or zlib compression | Delta compression or zlib compression (RLE is optional) |
         +-------------+--------------------------------------------------------+--------------------------------------+---------------------------------------------------------+

   -  COMPRESSLEVEL

      Specifies the compression level of the table data. It determines the compression ratio and time. This divides a compression level into sublevels, providing you with more choices for compression rate and duration. As the value becomes greater, the compression rate becomes higher and duration longer at the same compression level. The parameter is only valid for column-store table.

      Value range: 0 to 3. The default value is **0**.

   -  MAX_BATCHROW

      Specifies the maximum of a storage unit during data loading process. The parameter is only valid for column-store table.

      Value range: 10000 to 60000

      Default value: **60000**

      .. note::

         When a column-store table is imported, the following error is reported: **cu.cpp: 249: The parameter destMax is equal to zero or larger than the macro: SECUREC_STRING_MAX_LEN.**

         If the error persists after the statement or sorting is adjusted, change the maximum number of records in a storage unit from 60,000 to 30,000 by setting **MAX_BATCHROW**.

   -  PARTIAL_CLUSTER_ROWS

      Specifies the number of records to be partial cluster stored during data loading process. The parameter is only valid for column-store table.

      Value range: 600000 to 2147483647

   -  enable_delta

      Specifies whether to enable delta tables in column-store tables. The parameter is only valid for column-store tables.

      Default value: **off**

   -  DELTAROW_THRESHOLD

      Specifies the upper limit of to-be-imported rows for triggering the data import to a delta table when data is to be imported to a column-store table. This parameter takes effect only if the **enable_delta** table parameter is set to **on**. The parameter is only valid for column-store table.

      The value ranges from **0** to **60000**. The default value is **6000**.

   -  COLVERSION

      Specifies the version of the column-store format. You can switch between different storage formats.

      Valid value:

      **1.0**: Each column in a column-store table is stored in a separate file. The file name is **relfilenode.C1.0**, **relfilenode.C2.0**, **relfilenode.C3.0**, or similar.

      **2.0**: All columns of a column-store table are combined and stored in a file. The file is named **relfilenode.C1.0**.

      Default value: **2.0**

      The value of **COLVERSION** can only be set to **2.0** for OBS hot and cold tables.

      .. note::

         -  For clusters of version 8.1.0, the default value of this parameter is **1.0**. For clusters of version 8.1.1 or later, the default value of this parameter is **2.0**. If the cluster version is upgraded from 8.1.0 to 8.1.1 or later, the default value of this parameter changes from **1.0** to **2.0**.
         -  When creating a column-store table, set **COLVERSION** to **2.0**. Compared with the **1.0** storage format, the performance is significantly improved:

            #. The time required for creating a column-store wide table is significantly reduced.
            #. In the Roach data backup scenario, the backup time is significantly reduced.
            #. The build and catch up time is greatly reduced.
            #. The occupied disk space decreases significantly.

   -  COLD_TABLESPACE

      Specifies the OBS tablespace for the cold partitions in a hot or cold table. This parameter is available only to partitioned column-store tables and cannot be modified. It must be used together with **storage_policy**.

      Valid value: a valid OBS tablespace name

   -  STORAGE_POLICY

      Specifies the hot and cold partition switching policy. This parameter is supported only by hot and cold tables. This parameter must be used together with **cold_tablespace**.

      Value range: *Cold and hot switchover policy name*:*Cold and hot switchover threshold*. Currently, only LMT and HPN policies are supported. LMT indicates that the switchover is performed based on the last update time of partitions. HPN indicates the switchover is performed based on a fixed number of reserved hot partitions.

      -  **LMT:[**\ *day*\ **]**: Switch the hot partition data that is not updated in the last *[day]* days to the OBS tablespace as cold partition data. *[day]* is an integer ranging from 0 to 36500, in days.
      -  **HPN:[**\ *hot_partition_num*\ **]**: [*hot_partition_num*] indicates the number of hot partitions (with data) to be retained. The rule is to find the maximum sequence ID of the partitions with data. The partitions without data whose sequence ID is greater than the maximum sequence ID are hot partitions, and [*hot_partition_num*] partitions are retained as hot partitions in descending order according to the sequence ID. A partition whose sequence ID is smaller than the minimum sequence ID of the retained hot partition is a cold partition. During hot and cold partition switchover, data needs to be migrated to the OBS tablespace. *[hot_partition_num]* is an integer ranging from 0 to 1600.

      .. note::

         The hybrid data warehouse (standalone) does not support cold and hot partition switchover.

   -  SKIP_FPI_HINT

      Indicates whether to skip the hint bits operation when the full-page writes (FPW) log needs to be written during sequential scanning.

      Default value: **false**

      .. note::

         If **SKIP_FPI_HINT** is set to **true** and the checkpoint operation is performed on a table, no Xlog will be generated when the table is sequentially scanned. This applies to intermediate tables that are queried less frequently, reducing the size of Xlogs and improving query performance.

-  **ON COMMIT { PRESERVE ROWS \| DELETE ROWS \| DROP }**

   **ON COMMIT** determines what to do when you commit a temporary table creation operation. The three options are as follows. Currently, only **PRESERVE ROWS** and **DELETE ROWS** can be used.

   -  **PRESERVE ROWS** (Default): No special action is taken at the ends of transactions. The temporary table and its table data are unchanged.
   -  **DELETE ROWS**: All rows in the temporary table will be deleted at the end of each transaction block.
   -  **DROP**: The temporary table will be dropped at the end of the current transaction block.

-  **COMPRESS \| NOCOMPRESS**

   If you specify **COMPRESS** in the **CREATE TABLE** statement, the compression feature is triggered in the case of a bulk **INSERT** operation. If this feature is enabled, a scan is performed for all tuple data within the page to generate a dictionary and then the tuple data is compressed and stored. If **NOCOMPRESS** is specified, the table is not compressed.

   Default value: **NOCOMPRESS**, tuple data is not compressed before storage.

-  **DISTRIBUTE BY**

   Specifies how the table is distributed or replicated between DNs.

   Valid value:

   -  **REPLICATION**: Each row in the table exists on all DNs, that is, each DN has complete table data.
   -  **HASH (column_name)**: Each row of the table will be placed into all the DNs based on the hash value of the specified column.

      .. note::

         -  When **DISTRIBUTE BY HASH (column_name)** is specified, the primary key and its unique index must contain the **column_name** column.
         -  When **DISTRIBUTE BY HASH (column_name)** in a referenced table is specified, the foreign key of the reference table must contain the **column_name** column.
         -  The hybrid data warehouse (standalone) has only one DN. Therefore, the distribution rule is ignored and cannot be modified.

   Default value: **HASH(column_name)**, the key column of **column_name** (if any) or the column of distribution column supported by first data type.

   **column_name** supports the following data types:

   -  Integer types: TINYINT, SMALLINT, INT, BIGINT, and NUMERIC/DECIMAL
   -  Character types: CHAR, BPCHAR, VARCHAR, VARCHAR2, NVARCHAR2, and TEXT
   -  Date/time types: DATE, TIME, TIMETZ, TIMESTAMP, TIMESTAMPTZ, INTERVAL, and SMALLDATETIME

   .. note::

      When you create a table, the choices of distribution keys and partition keys have major impact on SQL query performance. Therefore, choosing proper distribution column and partition key with strategies.

      -  Selecting an Appropriate Distribution Column

         In the data distributed table using Hash, an appropriate distributed array should be used to distribute and store data on multiple DNs evenly, preventing data skew (uneven data distribution across several DNs). Determine the proper distribution column based on the following principles:

         #. Determine whether data is skewed.

            Connect to the database and run the following statements to check the number of tuples on each DN: Replace *tablename* with the actual name of the table to be analyzed.

            .. code-block::

               SELECT a.count,b.node_name FROM (SELECT count(*) AS count,xc_node_id FROM tablename GROUP BY xc_node_id) a, pgxc_node b WHERE a.xc_node_id=b.node_id ORDER BY a.count DESC;

            If tuple numbers vary greatly (several times or tenfold) in each DN, a data skew occurs. Change the data distribution key based on the following principles:

         #. Run the ALTER TABLE statement to adjust the distribution column. The rules for selecting a distribution column are as follows:

            The column value of the distribution column should be discrete so that data can be evenly distributed on each DN. For example, you are advised to select the primary key of a table as the distribution column, and the ID card number as the distribution column in a personnel information table.

            With the above principles met, you can select join conditions as distribution keys so that join tasks can be pushed down to DNs, reducing the amount of data transferred between the DNs.

      -  Selecting appropriate partition keys

         In range partitioning, the table is partitioned into ranges defined by a key column or set of columns, with no overlap between the ranges of values assigned to different partitions. Each range has a dedicated partition for data storage.

         Modify partition keys to make the query result stored in the same or least partitions (partition pruning). Obtaining consecutive I/O to improve the query performance.

         In actual services, time is used to filter query objects. Therefore, you can use time as a partition key, and change the key value based on the total data volume and single data query volume.

-  **TO { GROUP groupname \| NODE ( nodename [, ... ] ) }**

   **TO GROUP** specifies the Node Group in which the table is created. Currently, it cannot be used for HDFS tables. **TO NODE** is used for internal scale-out tools.

-  **CONSTRAINT constraint_name**

   Specifies a name for a column or table constraint. The optional constraint clauses specify constraints that new or updated rows must satisfy for an insert or update operation to succeed.

   There are two ways to define constraints:

   -  A column constraint is defined as part of a column definition, and it is bound to a particular column.
   -  A table constraint is not bound to any particular columns but can apply to more than one column.

-  **NOT NULL**

   Indicates that the column is not allowed to contain **NULL** values.

-  **NULL**

   The column is allowed to contain **NULL** values. This is the default setting.

   This clause is only provided for compatibility with non-standard SQL databases. You are advised not to use this clause.

-  **CHECK ( expression )**

   Specifies an expression producing a Boolean result which new or updated rows must satisfy for an insert or update operation to succeed. Expressions evaluating to **TRUE** or **UNKNOWN** succeed. If any row of an insert or update operation produces a FALSE result, an error exception is raised and the insert or update does not alter the database.

   A check constraint specified as a column constraint should reference only the column's values, while an expression appearing in a table constraint can reference multiple columns.

   .. note::

      **<>NULL** and **!=NULL** are invalid in an expression. Change them to **IS NOT NULL**.

-  **DEFAULT default_expr**

   Assigns a default data value for a column. The value can be any variable-free expressions (Subqueries and cross-references to other columns in the current table are not allowed). The data type of the default expression must match the data type of the column.

   The default expression will be used in any insert operation that does not specify a value for the column. If there is no default value for a column, then the default value is **NULL**.

-  **UNIQUE index_parameters**

   **UNIQUE ( column_name [, ... ] ) index_parameters**

   Specifies that a group of one or more columns of a table can contain only unique values.

   For the purpose of a unique constraint, NULL is not considered equal.

   .. note::

      If **DISTRIBUTE BY REPLICATION** is not specified, the column table that contains only unique values must contain distribution columns.

-  **PRIMARY KEY index_parameters**

   **PRIMARY KEY ( column_name [, ... ] ) index_parameters**

   Specifies the primary key constraint specifies that a column or columns of a table can contain only unique (non-duplicate) and non-null values.

   Only one primary key can be specified for a table.

   .. note::

      If **DISTRIBUTE BY REPLICATION** is not specified, the column set with a primary key constraint must contain distributed columns.

-  **DEFERRABLE \| NOT DEFERRABLE**

   Controls whether the constraint can be deferred. A constraint that is not deferrable will be checked immediately after every command. Checking of constraints that are deferrable can be postponed until the end of the transaction using the **SET CONSTRAINTS** command. **NOT DEFERRABLE** is the default value. Currently, only **UNIQUE** and **PRIMARY KEY** constraints of row-store tables accept this clause. All the other constraints are not deferrable.

-  **PARTIAL CLUSTER KEY**

   Specifies a partial cluster key for storage. When importing data to a column-store table, you can perform local data sorting by specified columns (single or multiple).

-  **INITIALLY IMMEDIATE \| INITIALLY DEFERRED**

   If a constraint is deferrable, this clause specifies the default time to check the constraint.

   -  If the constraint is **INITIALLY IMMEDIATE** (default value), it is checked after each statement.
   -  If the constraint is **INITIALLY DEFERRED**, it is checked only at the end of the transaction.

   The constraint check time can be altered using the **SET CONSTRAINTS** command.

Using the LIKE Clause to Declare a Table
----------------------------------------

The new table **films_bk** automatically inherits all column names, data types, and non-null constraints from the source table **films**.

::

   CREATE TABLE films (
   code        char(5) PRIMARY KEY,
   title       varchar(40) NOT NULL,
   did         integer NOT NULL,
   date_prod   date,
   kind        varchar(10),
   len         interval hour to minute
   );
   CREATE TABLE films_bk LIKE films;

Creating a Table with Default Columns
-------------------------------------

Specify that the default value of the **W_STATE** column to **GA**. At the end of the transaction, check for duplicate values in the **W_WAREHOUSE_NAME** column.

::

   CREATE TABLE tpcds.warehouse_t2
   (
       W_WAREHOUSE_SK            INTEGER                NOT NULL,
       W_WAREHOUSE_ID            CHAR(16)               NOT NULL,
       W_WAREHOUSE_NAME          VARCHAR(20)   UNIQUE DEFERRABLE,
       W_WAREHOUSE_SQ_FT         INTEGER                        ,
       W_STREET_NUMBER           CHAR(10)                       ,
       W_STREET_NAME             VARCHAR(60)                    ,
       W_STREET_TYPE             CHAR(15)                       ,
       W_SUITE_NUMBER            CHAR(10)                       ,
       W_CITY                    VARCHAR(60)                    ,
       W_COUNTY                  VARCHAR(30)                    ,
       W_STATE                   CHAR(2)            DEFAULT 'GA',
       W_ZIP                     CHAR(10)                       ,
       W_COUNTRY                 VARCHAR(20)                    ,
       W_GMT_OFFSET              DECIMAL(5,2)
   );

Creating a Table with a Filler Factor
-------------------------------------

Set the fill factor to 70%.

::

   CREATE TABLE tpcds.warehouse_t3
   (
       W_WAREHOUSE_SK            INTEGER                NOT NULL,
       W_WAREHOUSE_ID            CHAR(16)               NOT NULL,
       W_WAREHOUSE_NAME          VARCHAR(20)                    ,
       W_WAREHOUSE_SQ_FT         INTEGER                        ,
       W_STREET_NUMBER           CHAR(10)                       ,
       W_STREET_NAME             VARCHAR(60)                    ,
       W_STREET_TYPE             CHAR(15)                       ,
       W_SUITE_NUMBER            CHAR(10)                       ,
       W_CITY                    VARCHAR(60)                    ,
       W_COUNTY                  VARCHAR(30)                    ,
       W_STATE                   CHAR(2)                        ,
       W_ZIP                     CHAR(10)                       ,
       W_COUNTRY                 VARCHAR(20)                    ,
       W_GMT_OFFSET              DECIMAL(5,2),
       UNIQUE(W_WAREHOUSE_NAME) WITH(fillfactor=70)
   );

Alternatively, use the following syntax to create a table with its fillfactor set to 70%:

::

   CREATE TABLE tpcds.warehouse_t4
   (
       W_WAREHOUSE_SK            INTEGER                NOT NULL,
       W_WAREHOUSE_ID            CHAR(16)               NOT NULL,
       W_WAREHOUSE_NAME          VARCHAR(20)              UNIQUE,
       W_WAREHOUSE_SQ_FT         INTEGER                        ,
       W_STREET_NUMBER           CHAR(10)                       ,
       W_STREET_NAME             VARCHAR(60)                    ,
       W_STREET_TYPE             CHAR(15)                       ,
       W_SUITE_NUMBER            CHAR(10)                       ,
       W_CITY                    VARCHAR(60)                    ,
       W_COUNTY                  VARCHAR(30)                    ,
       W_STATE                   CHAR(2)                        ,
       W_ZIP                     CHAR(10)                       ,
       W_COUNTRY                 VARCHAR(20)                    ,
       W_GMT_OFFSET              DECIMAL(5,2)
   ) WITH(fillfactor=70);

Creating a Table Whose Data Is Not Written to WALs
--------------------------------------------------

Use **UNLOGGED** to specify that table data is not written to write-ahead logs (WALs).

::

   CREATE UNLOGGED TABLE tpcds.warehouse_t5
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
   );

Creating a Table Without Reporting Errors for Duplicate Tables (If Any)
-----------------------------------------------------------------------

If **IF NOT EXISTS** is specified, a table will be created if there is no table using the specified name. If there is already a table using the specified name, no error will be reported. A message will be displayed indicating that the table already exists, and the database will skip table creation.

::

   CREATE TABLE IF NOT EXISTS tpcds.warehouse_t6
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
   );

Creating a Table with a Primary Key Constraint
----------------------------------------------

Use **PRIMARY KEY** to declare the primary key.

::

   CREATE TABLE tpcds.warehouse_t7
   (
       W_WAREHOUSE_SK            INTEGER            PRIMARY KEY,
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
   );

Alternatively, use the following syntax to create a table with a primary key constraint:

::

   CREATE TABLE tpcds.warehouse_t8
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
       W_GMT_OFFSET              DECIMAL(5,2),
       PRIMARY KEY(W_WAREHOUSE_SK)
   );

Or use the following statement to specify the name of the constraint:

::

   CREATE TABLE tpcds.warehouse_t9
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
       W_GMT_OFFSET              DECIMAL(5,2),
       CONSTRAINT W_CSTR_KEY1 PRIMARY KEY(W_WAREHOUSE_SK)
   );

Creating a Table with a Compound Primary Key Constraint
-------------------------------------------------------

Use **PRIMARY KEY** to declare two primary keys at the same time.

::

   CREATE TABLE tpcds.warehouse_t10
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
       W_GMT_OFFSET              DECIMAL(5,2),
       CONSTRAINT W_CSTR_KEY2 PRIMARY KEY(W_WAREHOUSE_SK, W_WAREHOUSE_ID)
   );

Creating a Column-store Table
-----------------------------

Use **ORIENTATION** to specify the storage mode of table data.

::

   CREATE TABLE tpcds.warehouse_t11
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
   ) WITH (ORIENTATION = COLUMN);

Creating a Column-store Table Using Partial Clustered Storage
-------------------------------------------------------------

When data is imported to a column-store table, perform partial sorting based on the one or more columns specified by **PARTIAL CLUSTER KEY**.

::

   CREATE TABLE tpcds.warehouse_t12
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
       W_GMT_OFFSET              DECIMAL(5,2),
       PARTIAL CLUSTER KEY(W_WAREHOUSE_SK, W_WAREHOUSE_ID)
   ) WITH (ORIENTATION = COLUMN)

Defining a Column-store Table with Compression Enabled
------------------------------------------------------

Use the **with** clause to declare the compression level.

::

   CREATE TABLE tpcds.warehouse_t17
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
   ) WITH (ORIENTATION = COLUMN, COMPRESSION=HIGH);

Defining a Table with Compression Enabled
-----------------------------------------

When creating a table, specify the keyword **COMPRESS**.

::

   CREATE TABLE tpcds.warehouse_t13
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
   ) COMPRESS;

Creating a Table that Checks Column Constraints
-----------------------------------------------

Use **CONSTRAINT** to declare a constraint.

::

   CREATE TABLE tpcds.warehouse_t19
   (
       W_WAREHOUSE_SK            INTEGER               PRIMARY KEY CHECK (W_WAREHOUSE_SK > 0),
       W_WAREHOUSE_ID            CHAR(16)              NOT NULL,
       W_WAREHOUSE_NAME          VARCHAR(20)           CHECK (W_WAREHOUSE_NAME IS NOT NULL),
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
   );

::

   CREATE TABLE tpcds.warehouse_t20
   (
       W_WAREHOUSE_SK            INTEGER               PRIMARY KEY,
       W_WAREHOUSE_ID            CHAR(16)              NOT NULL,
       W_WAREHOUSE_NAME          VARCHAR(20)           CHECK (W_WAREHOUSE_NAME IS NOT NULL),
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
       W_GMT_OFFSET              DECIMAL(5,2),
       CONSTRAINT W_CONSTR_KEY2 CHECK(W_WAREHOUSE_SK > 0 AND W_WAREHOUSE_NAME IS NOT NULL)
   );

Creating a Temporary Table
--------------------------

Specify the **TEMP** or **TEMPORARY** keyword to create a temporary table.

::

   CREATE TEMPORARY TABLE warehouse_t14
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
   );

Create a temporary table in a transaction and specify that data of this table is deleted when the transaction is committed.

::

   CREATE TEMPORARY TABLE warehouse_t15
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
   ) ON COMMIT DELETE ROWS;

Creating a Row-store Table
--------------------------

Set **ORIENTATION** to **ROW**.

::

   CREATE TABLE tpcds.warehouse_t16
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
   ) WITH (ORIENTATION = ROW);

Creating a Column-store Table in a Specified Version
----------------------------------------------------

Set **COLVERSION** to specify the version of the column storage format.

::

   CREATE TABLE tpcds.warehouse_t18
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
   ) WITH (ORIENTATION = COLUMN, COLVERSION=2.0);

Creating a Column-store Table with the Delta Table Enabled
----------------------------------------------------------

Set **enable_delta=on** to enable the delta table in column-store tables.

::

   CREATE TABLE tpcds.warehouse_t21
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
   ) WITH (ORIENTATION = COLUMN, ENABLE_DELTA = ON);

Defining a Table with **SKIP_FPI_HINT** Enabled
-----------------------------------------------

Use the **with** clause to set **SKIP_FPI_HINT**.

::

   CREATE TABLE tpcds.warehouse_t22
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
   ) WITH (SKIP_FPI_HINT = TRUE);

Creating Hot and Cold Tables
----------------------------

Create an OBS tablespace that hot and cold tables depend on.

::

   CREATE TABLESPACE obs_location WITH(
       filesystem = obs,
       address = 'obs URL',
       access_key = 'xxxxxxxx',
       secret_access_key = 'xxxxxxxx',
       encrypt = 'on',
       storepath = '/obs_bucket/obs_tablespace'
   );

Create a hot or cold table. Only column-store partitioned tables are supported.

::

   CREATE TABLE tpcds.warehouse_t23
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
   )
   WITH (ORIENTATION = COLUMN, cold_tablespace = "obs_location", storage_policy = 'LMT:30')
   DISTRIBUTE BY HASH (W_WAREHOUSE_SK)
   PARTITION BY RANGE(W_WAREHOUSE_SQ_FT)
   (
       PARTITION P1 VALUES LESS THAN(100000),
       PARTITION P2 VALUES LESS THAN(200000),
       PARTITION P3 VALUES LESS THAN(300000),
       PARTITION P4 VALUES LESS THAN(400000),
       PARTITION P5 VALUES LESS THAN(500000),
       PARTITION P6 VALUES LESS THAN(600000),
       PARTITION P7 VALUES LESS THAN(700000),
       PARTITION P8 VALUES LESS THAN(MAXVALUE)
   )ENABLE ROW MOVEMENT;

Creating an Auto-increment Table That Uses UUID as the Primary Key
------------------------------------------------------------------

Set **W_UUID** to **SMALLSERIAL**.

::

   CREATE TABLE tpcds.warehouse_t24
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
       W_UUID                    SMALLSERIAL                   ,
       W_ZIP                     CHAR(10)                      ,
       W_COUNTRY                 VARCHAR(20)                   ,
       W_GMT_OFFSET              DECIMAL(5,2)
   ) WITH (ORIENTATION = ROW);

Creating a Table that Uses Hash Distribution
--------------------------------------------

Use **DISTRIBUTE BY** to specify table distribution across nodes.

::

   CREATE TABLE tpcds.warehouse_t25
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
       W_GMT_OFFSET              DECIMAL(5,2),
       CONSTRAINT W_CONSTR_KEY3 UNIQUE(W_WAREHOUSE_SK)
   )DISTRIBUTE BY HASH(W_WAREHOUSE_SK);

Defining a Table with Each Row Stored in All DNs
------------------------------------------------

::

   CREATE TABLE tpcds.warehouse_t26
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
   )DISTRIBUTE BY REPLICATION;

Links
-----

:ref:`ALTER TABLE <dws_06_0142>`, :ref:`DROP TABLE <dws_06_0208>`
