:original_name: dws_06_0177.html

.. _dws_06_0177:

CREATE TABLE
============

Function
--------

Creates a new empty table in the current database.

This table is owned by the user who executes the command. However, if the system administrator creates a table in the schema with the same name as a common user, the owner of the table is the user (not the system administrator).

Precautions
-----------

-  For details about the data types supported by column-store tables, see :ref:`Data Types Supported by Column-Store Tables <dws_06_0024>`.
-  It is recommended that the number of column-store and HDFS partitioned tables do not exceed 1000.
-  The primary key constraint and unique constraint in the table must contain a distribution column.
-  If an error occurs during table creation, after it is fixed, the system may fail to delete the empty disk files created before the last automatic clearance. This problem seldom occurs.
-  Column-store tables support the **PARTIAL CLUSTER KEY** and table-level primary key and unique constraints, but do not support table-level foreign key constraints.
-  Only the NULL, NOT NULL, and DEFAULT constant values can be used as column-store table column constraints.
-  Whether column-store tables support a delta table is specified by the **enable_delta** parameter. The threshold for storing data into a delta table is specified by the **deltarow_threshold** parameter.
-  Multi-temperature tables support only partitioned column-store tables and depend on available OBS tablespaces.
-  Multi-temperature tables support only the default tablespace **default_obs_tbs**. If you need to add an OBS tablespace, contact technical support.

Syntax
------

::

   CREATE [ [ GLOBAL | LOCAL ] { TEMPORARY | TEMP } | UNLOGGED ] TABLE [ IF NOT EXISTS ] table_name
       { ({ column_name data_type [ compress_mode ] [ COLLATE collation ] [ column_constraint [ ... ] ]
           | table_constraint
           | LIKE source_table [ like_option [...] ] }
           [, ... ])|
           LIKE source_table [ like_option [...] ] }
       [ WITH ( {storage_parameter = value} [, ... ] ) ]
       [ ON COMMIT { PRESERVE ROWS | DELETE ROWS } ]
       [ COMPRESS | NOCOMPRESS ]
       [ DISTRIBUTE BY { REPLICATION | ROUNDROBIN | { HASH ( column_name [,...] ) } } ]
       [ TO { GROUP groupname | NODE ( nodename [, ... ] ) } ]
       [ COMMENT [=] 'text' ];

-  **column_constraint** is as follows:

   ::

      [ CONSTRAINT constraint_name ]
      { NOT NULL |
        NULL |
        CHECK ( expression ) |
        DEFAULT default_expr |
        COMMENT 'text' |
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

Parameters
----------

-  **UNLOGGED**

   If this key word is specified, the created table is not a log table. Data written to unlogged tables is not written to the write-ahead log, which makes them considerably faster than ordinary tables. However, an unlogged table is automatically truncated after a crash or unclean shutdown, incurring data loss risks. The contents of an unlogged table are also not replicated to standby servers. Any indexes created on an unlogged table are not automatically logged as well.

   Usage scenario: Unlogged tables do not ensure safe data. Users can back up data before using unlogged tables; for example, users should back up the data before a system upgrade.

   Troubleshooting: If data is missing in the indexes of unlogged tables due to some unexpected operations such as an unclean shutdown, users should re-create the indexes with errors.

   .. important::

      The UNLOGGED table uses no primary/standby mechanism. In the case of system faults or abnormal breakpoints, data loss may occur. Therefore, the UNLOGGED table cannot be used to store basic data.

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

   A table name enclosed in double quotation marks can contain spaces and special characters. However, you are not advised to use these characters in a table name because they may make it difficult to reference and use. In addition, they may be processed differently under different database compatibility modes.

-  **column_name**

   Specifies the name of a column to be created in the new table.

   The column name can contain a maximum of 63 characters, including letters, digits, underscores (_), dollar signs ($), and number signs (#). It must start with a letter or underscore (_).

-  **data_type**

   Specifies the data type of the column.

   .. note::

      In a database compatible with Teradata or MySQL syntax, if the data type of a column is set to DATE, the DATE type is returned. Otherwise, the TIMESTAMP type is returned.

-  **compress_mode**

   Specifies the compress option of the table, only available for row-store table. The option specifies the algorithm preferentially used by table columns.

   This compression option is irrelevant to the adaptive compression algorithm of column-store tables. The adaptive compression algorithm is used for internal data storage of column-store tables and does not allow users to specify the compression mode. For details, see the description of the :ref:`COMPRESSION <en-us_topic_0000001188429064__l770ea1d44c9f44bf8d17c8cb0a26bac6>` parameter.

   Value range: DELTA, PREFIX, DICTIONARY, NUMSTR, NOCOMPRESS

   -  DELTA compression supports only data types with a length of 1 to 8 bytes (0 < pg_type.typlen<=8).
   -  PREFIX and NUMSTR compression support only variable-length data types (pg_type.typlen=-1) and NULL-terminated C strings (pg_type.typlen=-2).

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
   -  For an OBS multi-temperature table, all partitions of the new table are local hot partitions after **INCLUDING PARTITION** is specified.

   .. important::

      -  If the source table contains a sequence with the SERIAL, BIGSERIAL, or SMALLSERIAL data type, or a column in the source table is a sequence by default and the sequence is created for this table by using **CREATE SEQUENCE...** **OWNED BY**, these sequences will not be copied to the new table, and another sequence specific to the new table will be created. This is different from earlier versions. To share a sequence between the source table and new table, create a shared sequence (do not use **OWNED BY**) and set a column in the source table to this sequence.
      -  You are not advised to set a column in the source table to the sequence specific to another table especially when the table is distributed in specific Node Groups, because doing so may result in **CREATE TABLE ... LIKE** execution failures. In addition, doing so may cause the sequence to become invalid in the source sequence because the sequence will also be deleted from the source table when it is deleted from the table that the sequence is specific to. To share a sequence among multiple tables, you are advised to create a shared sequence for them.

-  **WITH ( { storage_parameter = value } [, ... ] )**

   Specifies an optional storage parameter for a table or an index.

   .. note::

      Using Numeric of any precision to define column, specifies precision p and scale s. When precision and scale are not specified, the input will be displayed.

   The description of parameters is as follows:

   -  FILLFACTOR

      The fillfactor of a table is a percentage between 10 and 100. 100 (complete packing) is the default value. When a smaller fillfactor is specified, **INSERT** operations pack table pages only to the indicated percentage. The remaining space on each page is reserved for updating rows on that page. This gives **UPDATE** a chance to place the updated copy of a row on the same page, which is more efficient than placing it on a different page. For a table whose records are never updated, setting the fillfactor to 100 (complete packing) is the appropriate choice, but in heavily updated tables smaller fillfactors are appropriate. The parameter has no meaning for column-store tables.

      Value range: 10 to 100

   -  ORIENTATION

      Specifies the storage mode (row-store, column-store) for table data. This parameter cannot be modified once it is set.

      Valid value:

      -  **ROW** indicates that table data is stored in rows.

         **ROW** applies to OLTP service, which has many interactive transactions. An interaction involves many columns in the table. Using ROW can improve the efficiency.

      -  **COLUMN** indicates that the data is stored in columns.

         **COLUMN** applies to the data warehouse service, which has a large amount of aggregation computing, and involves a few column operations.

      Default value: ROW (row-store)

      .. note::

         In cluster 8.1.3 and later versions, the GUC parameter **default_orientation** (default value: **row**) is added. If the storage mode is not specified when a table is created, by default, the table is created based on the value of the parameter (row, column, column enabledelta).

   -  .. _en-us_topic_0000001188429064__l770ea1d44c9f44bf8d17c8cb0a26bac6:

      COMPRESSION

      Specifies the compression level of the table data. It determines the compression ratio and time. Generally, the higher the level of compression, the higher the ratio, the longer the time, and the lower the level of compression, the lower the ratio, the shorter the time. The actual compression ratio depends on the distribution characteristics of loading table data.

      Valid value:

      The valid values for column-store tables are **YES**/**NO** and **LOW**/**MIDDLE**/**HIGH**, and the default is **LOW**. When this parameter is set to **YES**, the compression level is **LOW** by default.

      .. note::

         -  Currently, row-store table compression is not supported.

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

      Specifies the compression level of the table data. It determines the compression ratio and time. This divides a compression level into sublevels, providing you with more choices for compression rate and duration. As the value becomes greater, the compression rate becomes higher and duration longer at the same compression level. The parameter is only valid for column-store tables.

      Value range: 0 to 3. The default value is **0**.

   -  MAX_BATCHROW

      Specifies the maximum of a storage unit during data loading process. The parameter is only valid for column-store tables.

      Value range: 10000 to 60000

      Default value: 60,000

   -  PARTIAL_CLUSTER_ROWS

      Specifies the number of records to be partial cluster stored during data loading process. The parameter is only valid for column-store tables.

      Value range: 600000 to 2147483647

      Default value: 4,200,000

   -  enable_delta

      Specifies whether to enable delta tables in column-store tables. The parameter is only valid for column-store tables.

      Default value: **off**

   -  DELTAROW_THRESHOLD

      Specifies the upper limit of to-be-imported rows for triggering the data import to a delta table when data is to be imported to a column-store table. This parameter takes effect only if the **enable_delta** table parameter is set to **on**. The parameter is only valid for column-store tables.

      The value ranges from **0** to **60000**. The default value is **6000**.

   -  COLVERSION

      Specifies the version of the column-store format. You can switch between different storage formats.

      Valid value:

      **1.0**: Each column in a column-store table is stored in a separate file. The file name is **relfilenode.C1.0**, **relfilenode.C2.0**, **relfilenode.C3.0**, or similar.

      **2.0**: All columns of a column-store table are combined and stored in a file. The file is named **relfilenode.C1.0**.

      Default value: **2.0**

      The value of **COLVERSION** can only be set to **2.0** for OBS multi-temperature tables.

      .. note::

         -  For clusters of version 8.1.0, the default value of this parameter is **1.0**. For clusters of version 8.1.1 or later, the default value of this parameter is **2.0**. If the cluster version is upgraded from 8.1.0 to 8.1.1 or later, the default value of this parameter changes from **1.0** to **2.0**.
         -  When creating a column-store table, set **COLVERSION** to **2.0**. Compared with the **1.0** storage format, the performance is significantly improved:

            #. The time required for creating a column-store wide table is significantly reduced.
            #. In the Roach data backup scenario, the backup time is significantly reduced.
            #. The build and catch up time is greatly reduced.
            #. The occupied disk space decreases significantly.

   -  SKIP_FPI_HINT

      Indicates whether to skip the hint bits operation when the full-page writes (FPW) log needs to be written during sequential scanning.

      Default value: **false**

      .. note::

         If **SKIP_FPI_HINT** is set to **true** and the checkpoint operation is performed on a table, no Xlog will be generated when the table is sequentially scanned. This applies to intermediate tables that are queried less frequently, reducing the size of Xlogs and improving query performance.

-  **ON COMMIT { PRESERVE ROWS \| DELETE ROWS }**

   **ON COMMIT** determines what to do when you commit a temporary table creation operation.

   -  **PRESERVE ROWS** (Default): No special action is taken at the ends of transactions. The temporary table and its table data are unchanged.
   -  **DELETE ROWS**: All rows in the temporary table will be deleted at the end of each transaction block.

-  **COMPRESS \| NOCOMPRESS**

   If you specify **COMPRESS** in the **CREATE TABLE** statement, the compression feature is triggered in the case of a bulk **INSERT** operation. If this feature is enabled, a scan is performed for all tuple data within the page to generate a dictionary and then the tuple data is compressed and stored. If **NOCOMPRESS** is specified, the table is not compressed.

   Default value: **NOCOMPRESS**, tuple data is not compressed before storage.

-  **DISTRIBUTE BY**

   Specifies how the table is distributed or replicated between DNs.

   Valid value:

   -  **REPLICATION**: Each row in the table exists on all DNs, that is, each DN has complete table data.
   -  **ROUNDROBIN**: Each row in the table is sent to each DN in sequence. This distribution policy prevents data skew. However, data distribution nodes are random. As a result, there is a higher probability that table redistribution is triggered during computing. This distribution policy is recommended for large tables with severe column skew. This value is supported only in 8.1.2 or later.
   -  **HASH (column_name)**: Each row of the table will be placed into all the DNs based on the hash value of the specified column.

      .. note::

         -  When **DISTRIBUTE BY HASH (column_name)** is specified, the primary key and its unique index must contain the **column_name** column.
         -  When **DISTRIBUTE BY HASH (column_name)** in a referenced table is specified, the foreign key of the reference table must contain the **column_name** column.
         -  If **TO GROUP** is set to a replication table node group (supported in 8.1.2 or later), **DISTRIBUTE BY** must be set to **REPLICATION**. If **DISTRIBUTE BY** is not specified, the created table is automatically set as a replication table.
         -  The hybrid data warehouse (standalone) has only one DN. Therefore, the distribution rule is ignored and cannot be modified.

   Default value: determined by the GUC parameter **default_distribution_mode**

   -  When **default_distribution_mode** is set to **roundrobin**, the default value of **DISTRIBUTE BY** is selected according to the following rules:

      #. If the primary key or unique constraint is included during table creation, hash distribution is selected. The distribution column is the column corresponding to the primary key or unique constraint.
      #. If the primary key or unique constraint is not included during table creation, round-robin distribution is selected.

   -  When **default_distribution_mode** is set to **hash**, the default value of **DISTRIBUTE BY** is selected according to the following rules:

      #. If the primary key or unique constraint is included during table creation, hash distribution is selected. The distribution column is the column corresponding to the primary key or unique constraint.
      #. If the primary key or unique constraint is not included during table creation but there are columns whose data types can be used as distribution columns, hash distribution is selected. The distribution column is the first column whose data type can be used as a distribution column.
      #. If the primary key or unique constraint is not included during table creation and no column whose data type can be used as a distribution column exists, round-robin distribution is selected.

   The following data types can be used as distribution columns:

   -  Integer types: **TINYINT**, **SMALLINT**, **INT**, **BIGINT**, and **NUMERIC/DECIMAL**
   -  Character types: **CHAR**, **BPCHAR**, **VARCHAR**, **VARCHAR2**, **NVARCHAR2**, and **TEXT**
   -  Date/time types: **DATE**, **TIME**, **TIMETZ**, **TIMESTAMP**, **TIMESTAMPTZ**, **INTERVAL**, and **SMALLDATETIME**

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

         #. If a proper distribution column cannot be found to make data evenly distributed on each DN, you can use the **REPLICATION** or **ROUNDROBIN** data distribution mode. The **REPLICATION** data distribution mode stores complete data on each DN. Therefore, if a table is large and no proper distribution column can be found, the **ROUNDROBIN** data distribution mode is recommended. The **ROUNDROBIN** data distribution mode is supported in 8.1.2 or later.

      -  Selecting appropriate partition keys

         In range partitioning, the table is partitioned into ranges defined by a key column or set of columns, with no overlap between the ranges of values assigned to different partitions. Each range has a dedicated partition for data storage.

         Modify partition keys to make the query result stored in the same or least partitions (partition pruning). Obtaining consecutive I/O to improve the query performance.

         In actual services, time is used to filter query objects. Therefore, you can use time as a partition key, and change the key value based on the total data volume and single data query volume.

-  **TO { GROUP groupname \| NODE ( nodename [, ... ] ) }**

   **TO GROUP** specifies the Node Group in which the table is created. Currently, it cannot be used for HDFS tables. **TO NODE** is used for internal scale-out tools.

   In logical cluster mode, if **TO GROUP** is not specified, the table is created in the node group associated with the logical cluster user by default. If the user, such as the administrator or a common user, does not manage the logical cluster, by default the table is created in the first logical cluster, which is the logical cluster with the smallest **OID** in **pgxc_group**.

   If the node group specified by **TO GROUP** is a replication table node group, the table is created on all CNs and DNs, but the replication table data is distributed only on the DNs in the replication table node group.

-  **COMMENT [=] 'text'**

   The **COMMENT** clause can specify table comments during table creation.

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

-  **COMMENT** **'text'**

   The **COMMENT** clause can specify a comment for a column.

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

Example
-------

Define a unique column constraint for the table.

::

   DROP TABLE IF EXISTS CUSTOMER;
   CREATE TABLE CUSTOMER
   (
       C_CUSTKEY     BIGINT NOT NULL CONSTRAINT C_CUSTKEY_pk PRIMARY KEY  ,
       C_NAME        VARCHAR(25)  ,
       C_ADDRESS     VARCHAR(40)  ,
       C_NATIONKEY   INT          ,
       C_PHONE       CHAR(15)     ,
       C_ACCTBAL     DECIMAL(15,2)
   )
   DISTRIBUTE BY HASH(C_CUSTKEY);

Define a primary key table constraint for a table. You can define a primary key table constraint on one or more columns of a table.

::

   DROP TABLE IF EXISTS CUSTOMER;
   CREATE TABLE CUSTOMER
   (
       C_CUSTKEY     BIGINT       ,
       C_NAME        VARCHAR(25)  ,
       C_ADDRESS     VARCHAR(40)  ,
       C_NATIONKEY   INT          ,
       C_PHONE       CHAR(15)     ,
       C_ACCTBAL     DECIMAL(15,2)   ,
       CONSTRAINT C_CUSTKEY_KEY PRIMARY KEY(C_CUSTKEY,C_NAME)
   )
   DISTRIBUTE BY HASH(C_CUSTKEY,C_NAME);

Define the **CHECK** column constraint:

::

   DROP TABLE IF EXISTS CUSTOMER;
   CREATE TABLE CUSTOMER
   (
       C_CUSTKEY     BIGINT NOT NULL CONSTRAINT C_CUSTKEY_pk PRIMARY KEY  ,
       C_NAME        VARCHAR(25)  ,
       C_ADDRESS     VARCHAR(40)  ,
       C_NATIONKEY   INT NOT NULL  CHECK (C_NATIONKEY > 0)
   )
   DISTRIBUTE BY HASH(C_CUSTKEY);

Define the **CHECK** table constraint:

::

   DROP TABLE IF EXISTS CUSTOMER;
   CREATE TABLE CUSTOMER
   (
       C_CUSTKEY     BIGINT NOT NULL CONSTRAINT C_CUSTKEY_pk PRIMARY KEY  ,
       C_NAME        VARCHAR(25)      ,
       C_ADDRESS     VARCHAR(40)      ,
       C_NATIONKEY   INT              ,
       CONSTRAINT C_CUSTKEY_KEY2 CHECK(C_CUSTKEY > 0 AND C_NAME <> '')
   )
   DISTRIBUTE BY HASH(C_CUSTKEY);

Create a column-store table and specify the storage format and compression mode:

::

   DROP TABLE IF EXISTS customer_address;
   CREATE TABLE customer_address
   (
       ca_address_sk       INTEGER                  NOT NULL   ,
       ca_address_id       CHARACTER(16)            NOT NULL   ,
       ca_street_number    CHARACTER(10)                       ,
       ca_street_name      CHARACTER varying(60)               ,
       ca_street_type      CHARACTER(15)                       ,
       ca_suite_number     CHARACTER(10)
   )
   WITH (ORIENTATION = COLUMN, COMPRESSION=HIGH,COLVERSION=2.0)
   DISTRIBUTE BY HASH (ca_address_sk);

Use **DEFAULT** to declare a default value for column **W_STATE**:

::

   DROP TABLE IF EXISTS warehouse_t;
   CREATE TABLE warehouse_t
   (
       W_WAREHOUSE_SK            INTEGER                NOT NULL,
       W_WAREHOUSE_ID            CHAR(16)               NOT NULL,
       W_WAREHOUSE_NAME          VARCHAR(20)   UNIQUE DEFERRABLE,
       W_WAREHOUSE_SQ_FT         INTEGER                        ,
       W_COUNTY                  VARCHAR(30)                    ,
       W_STATE                   CHAR(2)            DEFAULT 'GA',
       W_ZIP                     CHAR(10)
   );

Create the **CUSTOMER_bk** table in LIKE mode:

::

   DROP TABLE IF EXISTS CUSTOMER_bk;
   CREATE TABLE CUSTOMER_bk (LIKE CUSTOMER INCLUDING ALL);

Helpful Links
-------------

:ref:`ALTER TABLE <dws_06_0142>`, :ref:`RENAME TABLE <dws_06_0276>`, :ref:`DROP TABLE <dws_06_0208>`
