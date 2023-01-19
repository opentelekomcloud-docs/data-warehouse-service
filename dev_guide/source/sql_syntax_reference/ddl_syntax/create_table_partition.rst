:original_name: dws_06_0179.html

.. _dws_06_0179:

CREATE TABLE PARTITION
======================

Function
--------

**CREATE TABLE PARTITION** creates a partitioned table. Partitioning refers to splitting what is logically one large table into smaller physical pieces based on specific schemes. The table based on the logic is called a partition cable, and a physical piece is called a partition. Data is stored on these smaller physical pieces, namely, partitions, instead of the larger logical partitioned table.

The common forms of partitioning include range partitioning, hash partitioning, list partitioning, and value partitioning. Currently, row-store and column-store tables support only range partitioning.

In range partitioning, the table is partitioned into ranges defined by a key column or set of columns, with no overlap between the ranges of values assigned to different partitions. Each range has a dedicated partition for data storage.

The partitioning policy for Range Partitioning refers to how data is inserted into partitions. Currently, range partitioning only allows the use of the range partitioning policy.

Range partitioning policy: Data is mapped to a created partition based on the partition key value. If the data can be mapped to, it is inserted into the specific partition; if it cannot be mapped to, error messages are returned. This is the most commonly used partitioning policy.

Partitioning can provide several benefits:

-  Query performance can be improved dramatically in certain situations, particularly when most of the heavily accessed rows of the table are in a single partition or a small number of partitions. Partitioning narrows the range of data search and improves data access efficiency.
-  When queries or updates access a large percentage of a single partition, performance can be improved by taking advantage of sequential scan of that partition instead of reads scattered across the whole table.
-  Bulk loads and deletion can be performed by adding or removing partitions, if that requirement is planned into the partitioning design. It also entirely avoids the **VACUUM** overhead caused by a bulk **DELETE** (only for range partitioning).

Precautions
-----------

A partitioned table supports unique and primary key constraints. The constraint keys of these constraints contain all partition keys.

Syntax
------

::

   CREATE TABLE [ IF NOT EXISTS ] partition_table_name
   ( [
       { column_name data_type [ COLLATE collation ] [ column_constraint [ ... ] ]
       | table_constraint
       | LIKE source_table [ like_option [...] ] }[, ... ]
   ] )
       [ WITH ( {storage_parameter = value} [, ... ] ) ]
       [ COMPRESS | NOCOMPRESS ]
       [ TABLESPACE tablespace_name ]
       [ DISTRIBUTE BY { REPLICATION | { [ HASH ] ( column_name ) } } ]
       [ TO { GROUP groupname | NODE ( nodename [, ... ] ) } ]
       PARTITION BY {
           {VALUES (partition_key)} |
           {RANGE (partition_key) ( partition_less_than_item [, ... ] )} |
           {RANGE (partition_key) ( partition_start_end_item [, ... ] )}
       } [ { ENABLE | DISABLE } ROW MOVEMENT ];

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

-  **table_constraint** is as follows:

   .. code-block::

      [ CONSTRAINT constraint_name ]
      { CHECK ( expression ) |
        UNIQUE ( column_name [, ... ] ) index_parameters |
        PRIMARY KEY ( column_name [, ... ] ) index_parameters}
      [ DEFERRABLE | NOT DEFERRABLE | INITIALLY DEFERRED | INITIALLY IMMEDIATE ]

-  **like_option** is as follows:

   ::

      { INCLUDING | EXCLUDING } { DEFAULTS | CONSTRAINTS | INDEXES | STORAGE | COMMENTS | RELOPTIONS | DISTRIBUTION | ALL }

-  **index_parameters** is as follows:

   ::

      [ WITH ( {storage_parameter = value} [, ... ] ) ]
      [ USING INDEX TABLESPACE tablespace_name ]

-  **partition_less_than_item** is as follows:

   ::

      PARTITION partition_name VALUES LESS THAN ( { partition_value | MAXVALUE } ) [TABLESPACE tablespace_name]

-  **partition_start_end_item** is as follows:

   ::

      PARTITION partition_name {
              {START(partition_value) END (partition_value) EVERY (interval_value)} |
              {START(partition_value) END ({partition_value | MAXVALUE})} |
              {START(partition_value)} |
              {END({partition_value | MAXVALUE})}
      } [TABLESPACE tablespace_name]

Parameter Description
---------------------

-  **IF NOT EXISTS**

   Does not throw an error if a table with the same name exists. A notice is issued in this case.

-  **partition_table_name**

   Name of the partitioned table

   Value range: a string. It must comply with the naming convention.

-  **column_name**

   Specifies the name of a column to be created in the new table.

   Value range: a string. It must comply with the naming convention.

-  **data_type**

   Specifies the data type of the column.

-  **COLLATE collation**

   Assigns a collation to the column (which must be of a collatable data type). If no collation is specified, the default collation is used.

   The collatable types are char, varchar, text, nchar, and nvarchar.

-  **CONSTRAINT constraint_name**

   Specifies a name for a column or table constraint. The optional constraint clauses specify constraints that new or updated rows must satisfy for an insert or update operation to succeed.

   There are two ways to define constraints:

   -  A column constraint is defined as part of a column definition, and it is bound to a particular column.
   -  A table constraint is not bound to any particular columns but can apply to more than one column.

-  **LIKE source_table [ like_option ... ]**

   Specifies a table from which the new table automatically copies all column names, their data types, and their not-null constraints.

   Unlike **INHERITS**, the new table and original table are decoupled after creation is complete. Changes to the original table will not be applied to the new table, and it is not possible to include data of the new table in scans of the original table.

   Default expressions for the copied column definitions will only be copied if **INCLUDING DEFAULTS** is specified. The default behavior is to exclude default expressions, resulting in the copied columns in the new table having default values **NULL**.

   **NOT NULL** constraints are always copied to the new table. **CHECK** constraints will only be copied if **INCLUDING CONSTRAINTS** is specified; other types of constraints will never be copied. These rules also apply to column constraints and table constraints.

   Unlike **INHERITS**, columns and constraints copied by **LIKE** are not merged with similarly named columns and constraints. If the same name is specified explicitly or in another **LIKE** clause, an error is reported.

   -  Any indexes on the source table will not be created on the new table, unless the **INCLUDING INDEXES** clause is specified.
   -  STORAGE settings for the copied column definitions will only be copied if **INCLUDING STORAGE** is specified. The default behavior is to exclude **STORAGE** settings.
   -  Comments for the copied columns, constraints, and indexes will only be copied if **INCLUDING COMMENTS** is specified. The default behavior is to exclude comments.
   -  If **INCLUDING RELOPTIONS** is specified, the new table will copy the storage parameter (**WITH** clause of the source table) of the source table. The default behavior is to exclude partition definition of the storage parameter of the source table.
   -  If **INCLUDING DISTRIBUTION** is specified, the new table will copy the distribution information of the source table, including distribution type and column, and the new table cannot use **DISTRIBUTE BY** clause. The default behavior is to exclude distribution information of the source table.
   -  **INCLUDING ALL** is an abbreviated form of **INCLUDING DEFAULTS INCLUDING CONSTRAINTS INCLUDING INDEXES INCLUDING STORAGE INCLUDING COMMENTS INCLUDING RELOPTIONS INCLUDING DISTRIBUTION.**

-  **WITH ( storage_parameter [= value] [, ... ] )**

   Specifies an optional storage parameter for a table or an index. Optional parameters are as follows:

   -  FILLFACTOR

      The fillfactor of a table is a percentage between 10 and 100. 100 (complete packing) is the default value. When a smaller fillfactor is specified, **INSERT** operations pack table pages only to the indicated percentage. The remaining space on each page is reserved for updating rows on that page. This gives **UPDATE** a chance to place the updated copy of a row on the same page, which is more efficient than placing it on a different page. For a table whose records are never updated, setting the fillfactor to 100 (complete packing) is the appropriate choice, but in heavily updated tables smaller fillfactors are appropriate. The parameter has no meaning for column-store tables.

      Value range: 10-100

   -  ORIENTATION

      Determines the storage mode of the data in the table.

      Valid value:

      -  **COLUMN**: The data will be stored in columns.
      -  **ROW** (default value): The data will be stored in rows.
      -  **ORC**: The data of the table will be stored in ORC format (only HDFS table).

         .. important::

            **orientation** cannot be modified.

   -  COMPRESSION

      -  The valid values for column-store tables are **YES**/**NO** and **LOW**/**MIDDLE**/**HIGH**, and the default is **LOW**.
      -  The valid values for row-store tables are **YES** and **NO**, and the default is **NO**.

         .. note::

            The row-store table compression function is not put into commercial use. To use this function, contact technical support engineers.

   -  MAX_BATCHROW

      Specifies the maximum of a storage unit during data loading process. The parameter is only valid for column-store table.

      Value range: 10000 to 60000

      Default value: **60000**

   -  PARTIAL_CLUSTER_ROWS

      Specifies the number of records to be partial cluster stored during data loading process. The parameter is only valid for column-store table.

      Value range: The valid value is no less than 100000. The value is the multiple of **MAX_BATCHROW**.

   -  enable_delta

      Specifies whether to enable delta tables in column-store tables. The parameter is only valid for column-store tables.

      Default value: **off**

   -  DELTAROW_THRESHOLD

      A reserved parameter. The parameter is only valid for column-store table.

      The value ranges from **0** to **60000**. The default value is **6000**.

   -  COLD_TABLECPACE

      Specifies the OBS tablespace for the cold partitions in a hot or cold table. This parameter is available only to partitioned column-store tables and cannot be modified. It must be used together with **storage_policy**.

      Valid value: a valid OBS tablespace name

   -  STORAGE_POLICY

      Specifies the rule for switching between hot and cold partitions. This parameter is used only for multi-temperature tables. It must be used together with **cold_tablespace**.

      Value range: *Cold and hot switchover policy name*:*Cold and hot switchover threshold*. Currently, only LMT and HPN policies are supported. LMT indicates that the switchover is performed based on the last update time of partitions. HPN indicates the switchover is performed based on a fixed number of reserved hot partitions.

      -  **LMT:[**\ *day*\ **]**: Switch the hot partition data that is not updated in the last *[day]* days to the OBS tablespace as cold partition data. *[day]* is an integer ranging from 0 to 36500, in days.
      -  **HPN:[**\ *hot_partition_num*\ **]**: [*hot_partition_num*] indicates the number of hot partitions (with data) to be retained. The rule is to find the maximum sequence ID of the partitions with data. The partitions without data whose sequence ID is greater than the maximum sequence ID are hot partitions, and [*hot_partition_num*] partitions are retained as hot partitions in descending order according to the sequence ID. A partition whose sequence ID is smaller than the minimum sequence ID of the retained hot partition is a cold partition. During hot and cold partition switchover, data needs to be migrated to the OBS tablespace. *[hot_partition_num]* is an integer ranging from 0 to 1600.

   -  COLVERSION

      Specifies the version of the column-store format. Switching between different storage formats is supported. However, the storage format of a partitioned table cannot be switched.

      Valid value:

      **1.0**: Each column in a column-store table is stored in a separate file. The file name is **relfilenode.C1.0**, **relfilenode.C2.0**, **relfilenode.C3.0**, or similar.

      **2.0**: All columns of a column-store table are combined and stored in a file. The file is named **relfilenode.C1.0**.

      Default value: **2.0**

      The value of **COLVERSION** can only be set to **2.0** for OBS hot and cold tables.

      .. note::

         When creating a column-store table, set **COLVERSION** to **2.0**. Compared with the **1.0** storage format, the performance is significantly improved:

         #. The time required for creating a column-store wide table is significantly reduced.
         #. In the Roach data backup scenario, the backup time is significantly reduced.
         #. The build and catch up time is greatly reduced.
         #. The occupied disk space decreases significantly.

   -  SKIP_FPI_HINT

      Indicates whether to skip the hint bits operation when the full-page writes (FPW) log needs to be written during sequential scanning.

      Default value: **false**

      .. note::

         If **SKIP_FPI_HINT** is set to **true** and the checkpoint operation is performed on a table, no Xlog will be generated when the table is sequentially scanned. This applies to intermediate tables that are queried less frequently, reducing the size of Xlogs and improving query performance.

-  **COMPRESS / NOCOMPRESS**

   Specifies the keyword **COMPRESS** during the creation of a table, so that the compression feature is triggered in the case of a bulk **INSERT** operation. If this feature is enabled, a scan is performed for all tuple data within the page to generate a dictionary and then the tuple data is compressed and stored. If **NOCOMPRESS** is specified, the table is not compressed.

   Default value: **NOCOMPRESS**, tuple data is not compressed before storage.

-  **TABLESPACE tablespace_name**

   Specifies the new table will be created in **tablespace_name** tablespace. If not specified, default tablespace is used. The OBS tablespace is not supported.

-  **DISTRIBUTE BY**

   Specifies how the table is distributed or replicated between DNs.

   Valid value:

   -  **REPLICATION**: Each row in the table exists on all DNs, that is, each DN has complete table data.
   -  **HASH (column_name)**: Each row of the table will be placed into all the DNs based on the hash value of the specified column.

   .. important::

      -  When **DISTRIBUTE BY HASH (column_name)** is specified, the primary key and its unique index must contain the **column_name** column.
      -  When **DISTRIBUTE BY HASH (column_name)** in a referenced table is specified, the foreign key of the reference table must contain the **column_name** column.

   Default value: **HASH(column_name)**, the key column of **column_name** (if any) or the column of distribution column supported by first data type.

   **column_name** supports the following data types:

   -  INTEGER TYPES: TINYINT, SMALLINT, INT, BIGINT, NUMERIC/DECIMAL
   -  CHARACTER TYPES: CHAR, BPCHAR, VARCHAR, VARCHAR2, NVARCHAR2
   -  DATA/TIME TYPES: DATE, TIME, TIMETZ, TIMESTAMP, TIMESTAMPTZ, INTERVAL, SMALLDATETIME

-  **TO { GROUP groupname \| NODE ( nodename [, ... ] ) }**

   **TO GROUP** specifies the Node Group in which the table is created. Currently, it cannot be used for HDFS tables. **TO NODE** is used for internal scale-out tools.

-  .. _en-us_topic_0000001099150744__lb144da954d4c4ac58c1e9ae1391e59ac:

   **PARTITION BY RANGE(partition_key)**

   Creates a range partition. **partition_key** is the name of the partition key.

   (1) Assume that the **VALUES LESS THAN** syntax is used.

   .. important::

      In this case, a maximum of four partition keys are supported.

   Data types supported by the partition keys are as follows: SMALLINT, INTEGER, BIGINT, DECIMAL, NUMERIC, REAL, DOUBLE PRECISION, CHARACTER VARYING(n), VARCHAR(n), CHARACTER(n), CHAR(n), CHARACTER, CHAR, TEXT, NVARCHAR2, NAME, TIMESTAMP[(p)] [WITHOUT TIME ZONE], TIMESTAMP[(p)] [WITH TIME ZONE], and DATE.

   (2) Assume that the **START END** syntax is used.

   .. important::

      In this case, only one partition key is supported.

   Data types supported by the partition key are as follows: SMALLINT, INTEGER, BIGINT, DECIMAL, NUMERIC, REAL, DOUBLE PRECISION, TIMESTAMP[(p)] [WITHOUT TIME ZONE], TIMESTAMP[(p)] [WITH TIME ZONE], and DATE.

-  **PARTITION partition_name VALUES LESS THAN ( { partition_value \| MAXVALUE } )**

   Specifies the information of partitions. **partition_name** is the name of a range partition. **partition_value** is the upper limit of range partition, and the value depends on the type of **partition_key**. **MAXVALUE** can specify the upper boundary of a range partition, and it is commonly used to specify the upper boundary of the last range partition.

   .. important::

      -  Upper boundaries must be specified for each partition.
      -  The types of upper boundaries must be the same as those of partition keys.
      -  In a partition list, partitions are arranged in ascending order of upper boundary values. Therefore, a partition with a certain upper boundary value is placed before another partition with a larger upper boundary value.
      -  If a partition key consists of multiple columns, the columns are used for partitioning in sequence. The first column is preferred to be used for partitioning. If the values of the first columns are the same, the second column is used. The subsequent columns are used in the same manner.

-  .. _en-us_topic_0000001099150744__li2094151861116:

   **PARTITION partition_name {START (partition_value) END (partition_value) EVERY (interval_value)}** **\|** **{START (partition_value) END (partition_value|MAXVALUE)} \| {START(partition_value)} \| {END (partition_value \| MAXVALUE)**}

   Specifies partition definitions.

   -  **partition_name**: name or name prefix of a range partition. It is the name prefix only in the following cases (assuming that **partition_name** is **p1**):

      -  If START+END+EVERY is used, the names of partitions will be defined as **p1_1**, **p1_2**, and the like. For example, if **PARTITION p1 START(1) END(4) EVERY(1)** is defined, the generated partitions are [1, 2), [2, 3), and [3, 4), and their names are **p1_1**, **p1_2**, and **p1_3**. In this case, **p1** is a name prefix.
      -  If the defined statement is in the first place and has **START** specified, the range (**MINVALUE**, **START**) will be automatically used as the first actual partition, and its name will be **p1_0**. The other partitions are then named **p1_1**, **p1_2**, and the like. For example, if **PARTITION p1 START(1), PARTITION p2 START(2)** is defined, generated partitions are (MINVALUE, 1), [1, 2), and [2, MAXVALUE), and their names will be **p1_0**, **p1_1**, and **p2**. In this case, **p1** is a name prefix and **p2** is a partition name. **MINVALUE** means the minimum value.

   -  **partition_value**: start point value or end point value of a range partition. The value depends on **partition_key** and cannot be **MAXVALUE**.
   -  **interval_value**: width of each partition for dividing the [**START**, **END**) range. It cannot be **MAXVALUE**. If the value of (**END** - **START**) divided by **EVERY** has a remainder, the width of only the last partition is less than the value of **EVERY**.
   -  **MAXVALUE**: maximum value. It is usually used to set the upper boundary for the last range partition.

   .. important::

      #. If the defined statement is in the first place and has **START** specified, the range (**MINVALUE**, **START**) will be automatically used as the first actual partition.
      #. The **START END** syntax must comply with the following rules:

         -  The value of **START** (if any, same for the following situations) in each **partition_start_end_item** must be smaller than that of **END**.
         -  In two adjacent **partition_start_end_item** statements, the value of the first **END** must be equal to that of the second **START**.
         -  The value of **EVERY** in each **partition_start_end_item** must be a positive number (in ascending order) and must be smaller than **END** minus **START**.
         -  Each partition includes the start value (unless it is **MINVALUE**) and excludes the end value. The format is as follows: [Start value, end value).
         -  Partitions created by the same **partition_start_end_item** belong to the same tablespace.
         -  If **partition_name** is a name prefix of a partition, the length must not exceed 57 bytes. If there are more than 57 bytes, the prefix will be automatically truncated.
         -  When creating or modifying a partitioned table, ensure that the total number of partitions in the table does not exceed the maximum value (32767).

      #. In statements for creating partitioned tables, **START END** and **LESS THAN** cannot be used together.
      #. The **START END** syntax in a partitioned table creation SQL statement will be replaced with the **VALUES LESS THAN** syntax when **gs_dump** is executed.

-  **{ ENABLE \| DISABLE } ROW MOVEMENT**

   Specifies the row movement switch.

   If the tuple value is updated on the partition key during the **UPDATE** action, the partition where the tuple is located is altered. Setting of this parameter enables error messages to be reported or movement of the tuple between partitions.

   Valid value:

   -  **ENABLE**: Row movement is enabled.
   -  **DISABLE** (default value): Disable row movement.

-  **NOT NULL**

   Indicates that the column is not allowed to contain **NULL** values. **ENABLE** can be omitted.

-  **NULL**

   Indicates that the column is allowed to contain **NULL** values. This is the default setting.

   This clause is only provided for compatibility with non-standard SQL databases. You are advised not to use this clause.

-  **CHECK (condition) [ NO INHERIT ]**

   Specifies an expression producing a Boolean result which new or updated rows must satisfy for an insert or update operation to succeed. Expressions evaluating to **TRUE** or **UNKNOWN** succeed. If any row of an insert or update operation produces a FALSE result, an error exception is raised and the insert or update does not alter the database.

   A check constraint specified as a column constraint should reference only the column's values, while an expression appearing in a table constraint can reference multiple columns.

   A constraint marked with **NO INHERIT** will not propagate to child tables.

   **ENABLE** can be omitted.

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

-  **INITIALLY IMMEDIATE \| INITIALLY DEFERRED**

   If a constraint is deferrable, this clause specifies the default time to check the constraint.

   -  If the constraint is **INITIALLY IMMEDIATE** (default value), it is checked after each statement.
   -  If the constraint is **INITIALLY DEFERRED**, it is checked only at the end of the transaction.

   The constraint check time can be altered using the **SET CONSTRAINTS** command.

-  **USING INDEX TABLESPACE tablespace_name**

   Allows selection of the tablespace in which the index associated with a **UNIQUE** or **PRIMARY KEY** constraint will be created. If not specified, **default_tablespace** is consulted, or the default tablespace in the database if **default_tablespace** is empty. The OBS tablespace is not supported.

Examples
--------

-  Example 1: Create a range-partitioned table **tpcds.web_returns_p1**. The table has eight partitions and the data type of their partition key is integer. The ranges of the partitions are: wr_returned_date_sk < 2450815, 2450815 <= wr_returned_date_sk < 2451179, 2451179 <= wr_returned_date_sk < 2451544, 2451544 <= wr_returned_date_sk < 2451910, 2451910 <= wr_returned_date_sk < 2452275, 2452275 <= wr_returned_date_sk < 2452640, 2452640 <= wr_returned_date_sk < 2453005, and wr_returned_date_sk >= 2453005.

   ::

      CREATE TABLE tpcds.web_returns_p1
      (
          WR_RETURNED_DATE_SK       INTEGER                       ,
          WR_RETURNED_TIME_SK       INTEGER                       ,
          WR_ITEM_SK                INTEGER               NOT NULL,
          WR_REFUNDED_CUSTOMER_SK   INTEGER                       ,
          WR_REFUNDED_CDEMO_SK      INTEGER                       ,
          WR_REFUNDED_HDEMO_SK      INTEGER                       ,
          WR_REFUNDED_ADDR_SK       INTEGER                       ,
          WR_RETURNING_CUSTOMER_SK  INTEGER                       ,
          WR_RETURNING_CDEMO_SK     INTEGER                       ,
          WR_RETURNING_HDEMO_SK     INTEGER                       ,
          WR_RETURNING_ADDR_SK      INTEGER                       ,
          WR_WEB_PAGE_SK            INTEGER                       ,
          WR_REASON_SK              INTEGER                       ,
          WR_ORDER_NUMBER           BIGINT                NOT NULL,
          WR_RETURN_QUANTITY        INTEGER                       ,
          WR_RETURN_AMT             DECIMAL(7,2)                  ,
          WR_RETURN_TAX             DECIMAL(7,2)                  ,
          WR_RETURN_AMT_INC_TAX     DECIMAL(7,2)                  ,
          WR_FEE                    DECIMAL(7,2)                  ,
          WR_RETURN_SHIP_COST       DECIMAL(7,2)                  ,
          WR_REFUNDED_CASH          DECIMAL(7,2)                  ,
          WR_REVERSED_CHARGE        DECIMAL(7,2)                  ,
          WR_ACCOUNT_CREDIT         DECIMAL(7,2)                  ,
          WR_NET_LOSS               DECIMAL(7,2)
      )
      WITH (ORIENTATION = COLUMN,COMPRESSION=MIDDLE)
      DISTRIBUTE BY HASH (WR_ITEM_SK)
      PARTITION BY RANGE(WR_RETURNED_DATE_SK)
      (
              PARTITION P1 VALUES LESS THAN(2450815),
              PARTITION P2 VALUES LESS THAN(2451179),
              PARTITION P3 VALUES LESS THAN(2451544),
              PARTITION P4 VALUES LESS THAN(2451910),
              PARTITION P5 VALUES LESS THAN(2452275),
              PARTITION P6 VALUES LESS THAN(2452640),
              PARTITION P7 VALUES LESS THAN(2453005),
              PARTITION P8 VALUES LESS THAN(MAXVALUE)
      );

-  Example 2: Create a range partitioned table **tpcds.web_returns_p2**. The table has eight partitions and the data type of their partition key is integer. The upper limit of the eighth partition is **MAXVALUE**.

   The ranges of the partitions are: wr_returned_date_sk < 2450815, 2450815 <= wr_returned_date_sk < 2451179, 2451179 <= wr_returned_date_sk < 2451544, 2451544 <= wr_returned_date_sk < 2451910, 2451910 <= wr_returned_date_sk < 2452275, 2452275 <= wr_returned_date_sk < 2452640, 2452640 <= wr_returned_date_sk < 2453005, and wr_returned_date_sk >= 2453005.

   Assume that *CN and DN data directory/pg_location/mount1/path1*, *CN and DN data directory/pg_location/mount2/path2*, *CN and DN data directory/pg_location/mount3/path3*, and *CN and DN data directory/pg_location/mount4/path4* are empty directories for which user **dwsadmin** has read and write permissions.

   ::

      CREATE TABLE tpcds.web_returns_p2
      (
          WR_RETURNED_DATE_SK       INTEGER                       ,
          WR_RETURNED_TIME_SK       INTEGER                       ,
          WR_ITEM_SK                INTEGER               NOT NULL,
          WR_REFUNDED_CUSTOMER_SK   INTEGER                       ,
          WR_REFUNDED_CDEMO_SK      INTEGER                       ,
          WR_REFUNDED_HDEMO_SK      INTEGER                       ,
          WR_REFUNDED_ADDR_SK       INTEGER                       ,
          WR_RETURNING_CUSTOMER_SK  INTEGER                       ,
          WR_RETURNING_CDEMO_SK     INTEGER                       ,
          WR_RETURNING_HDEMO_SK     INTEGER                       ,
          WR_RETURNING_ADDR_SK      INTEGER                       ,
          WR_WEB_PAGE_SK            INTEGER                       ,
          WR_REASON_SK              INTEGER                       ,
          WR_ORDER_NUMBER           BIGINT                NOT NULL,
          WR_RETURN_QUANTITY        INTEGER                       ,
          WR_RETURN_AMT             DECIMAL(7,2)                  ,
          WR_RETURN_TAX             DECIMAL(7,2)                  ,
          WR_RETURN_AMT_INC_TAX     DECIMAL(7,2)                  ,
          WR_FEE                    DECIMAL(7,2)                  ,
          WR_RETURN_SHIP_COST       DECIMAL(7,2)                  ,
          WR_REFUNDED_CASH          DECIMAL(7,2)                  ,
          WR_REVERSED_CHARGE        DECIMAL(7,2)                  ,
          WR_ACCOUNT_CREDIT         DECIMAL(7,2)                  ,
          WR_NET_LOSS               DECIMAL(7,2)
      )
      DISTRIBUTE BY HASH (WR_ITEM_SK)
      PARTITION BY RANGE(WR_RETURNED_DATE_SK)
      (
              PARTITION P1 VALUES LESS THAN(2450815),
              PARTITION P2 VALUES LESS THAN(2451179),
              PARTITION P3 VALUES LESS THAN(2451544),
              PARTITION P4 VALUES LESS THAN(2451910),
              PARTITION P5 VALUES LESS THAN(2452275),
              PARTITION P6 VALUES LESS THAN(2452640),
              PARTITION P7 VALUES LESS THAN(2453005),
              PARTITION P8 VALUES LESS THAN(MAXVALUE)
      )
      ENABLE ROW MOVEMENT;

-  Example 3: Use **START END** to create and modify a range partitioned table.

   Assume that **/home/dbadmin/startend_tbs1**, **/home/dbadmin/startend_tbs2**, **/home/dbadmin/startend_tbs3**, and **/home/dbadmin/startend_tbs4** are empty directories that user **dbadmin** has read and write permissions for.

   Create a partitioned table with the partition key of type integer.

   ::

      CREATE TABLE tpcds.startend_pt (c1 INT, c2 INT)

      DISTRIBUTE BY HASH (c1)
      PARTITION BY RANGE (c2) (
          PARTITION p1 START(1) END(1000) EVERY(200) ,
          PARTITION p2 END(2000),
          PARTITION p3 START(2000) END(2500) ,
          PARTITION p4 START(2500),
          PARTITION p5 START(3000) END(5000) EVERY(1000)
      )
      ENABLE ROW MOVEMENT;

   View the information of the partitioned table.

   ::

      SELECT relname, boundaries FROM pg_partition p where p.parentid='tpcds.startend_pt'::regclass ORDER BY 1;
         relname   | boundaries
      -------------+------------
       p1_0        | {1}
       p1_1        | {201}
       p1_2        | {401}
       p1_3        | {601}
       p1_4        | {801}
       p1_5        | {1000}
       p2          | {2000}
       p3          | {2500}
       p4          | {3000}
       p5_1        | {4000}
       p5_2        | {5000}
       tpcds.startend_pt |
      (12 rows)

   Import data and check the data volume in the partition.

   ::

      INSERT INTO tpcds.startend_pt VALUES (GENERATE_SERIES(0, 4999), GENERATE_SERIES(0, 4999));
      SELECT COUNT(*) FROM tpcds.startend_pt PARTITION FOR (0);
      count
      -------
      1
      (1 row)

      SELECT COUNT(*) FROM tpcds.startend_pt PARTITION (p3);
      count
      -------
      500
      (1 row)

   View the information of the partitioned table.

   ::

      SELECT relname, boundaries FROM pg_partition p where p.parentid='tpcds.startend_pt'::regclass ORDER BY 1;
         relname   | boundaries
      -------------+------------
       p1_0        | {1}
       p1_1        | {201}
       p1_2        | {401}
       p1_3        | {601}
       p1_4        | {801}
       p1_5        | {1000}
       p2          | {2000}
       p3          | {2500}
       p4          | {3000}
       p5_1        | {4000}
       p6_1        | {5300}
       p6_2        | {5600}
       p6_3        | {5900}
       p71         | {6000}
       q1_1        | {4250}
       q1_2        | {4500}
       q1_3        | {4750}
       q1_4        | {5000}
       tpcds.startend_pt |
      (19 rows)

-  Example 4: Create a partitioned table **customer_address** partitioned by month. The table has 13 partitions and the partition keys are dates.

   Create a partitioned table **customer_address**.

   ::

      CREATE TABLE customer_address
      (
          ca_address_sk       integer           NOT NULL,
          ca_address_date       date            NOT NULL
      )
      DISTRIBUTE BY HASH (ca_address_sk)
      PARTITION BY RANGE (ca_address_date)
      (
              PARTITION p202001 VALUES LESS THAN('20200101'),
              PARTITION p202002 VALUES LESS THAN('20200201'),
              PARTITION p202003 VALUES LESS THAN('20200301'),
              PARTITION p202004 VALUES LESS THAN('20200401'),
              PARTITION p202005 VALUES LESS THAN('20200501'),
              PARTITION p202006 VALUES LESS THAN('20200601'),
              PARTITION p202007 VALUES LESS THAN('20200701'),
              PARTITION p202008 VALUES LESS THAN('20200801'),
              PARTITION p202009 VALUES LESS THAN('20200901'),
              PARTITION p202010 VALUES LESS THAN('20201001'),
              PARTITION p202011 VALUES LESS THAN('20201101'),
              PARTITION p202012 VALUES LESS THAN('20201201'),
              PARTITION p202013 VALUES LESS THAN(MAXVALUE)
      );

   Insert data:

   ::

      INSERT INTO customer_address values('1','20200215');
      INSERT INTO customer_address values('7','20200805');
      INSERT INTO customer_address values('9','20201111');
      INSERT INTO customer_address values('4','20201231');

   Query a partition:

   ::

      SELECT * FROM customer_address PARTITION(p202009);
       ca_address_sk |   ca_address_date
      ---------------+---------------------
                   7 | 2020-08-05 00:00:00
      (1 row)

-  Example 5: Create multiple partition syntax at a time using the START END syntax.

   -  Create a partitioned table **day_part**. Each day is a partition, and the partition key is a date.

      ::

         CREATE table day_part(id int,d_time date)
         DISTRIBUTE BY HASH (id)
         PARTITION BY RANGE (d_time)
         (PARTITION p1 START('2022-01-01') END('2022-01-31') EVERY(interval '1 day'));
         ALTER TABLE  day_part ADD PARTITION pmax VALUES LESS THAN (maxvalue);

   -  Create a partitioned table **week_part**, seven days as a partition, and the partition key is a date.

      ::

         CREATE table week_part(id int,w_time date)
         DISTRIBUTE BY HASH (id)
         PARTITION BY RANGE (w_time)
         (PARTITION p1 START('2021-01-01') END('2022-01-01') EVERY(interval '7 day'));
         ALTER TABLE  week_part ADD PARTITION pmax VALUES LESS THAN (maxvalue);

   -  Create the partition table **month_part**, each month as a partition, and the partition key is a date.

      ::

         CREATE table month_part(id int,m_time date)
         DISTRIBUTE BY HASH (id)
         PARTITION BY RANGE (m_time)
         (PARTITION p1 START('2021-01-01') END('2022-01-01') EVERY(interval '1 month'));
         ALTER TABLE  month_part ADD PARTITION pmax VALUES LESS THAN (maxvalue);

Links
-----

:ref:`ALTER TABLE PARTITION <dws_06_0143>`, :ref:`DROP TABLE <dws_06_0208>`
