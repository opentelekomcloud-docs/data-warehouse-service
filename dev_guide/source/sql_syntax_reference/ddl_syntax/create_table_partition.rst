:original_name: dws_06_0179.html

.. _dws_06_0179:

CREATE TABLE PARTITION
======================

Function
--------

**CREATE TABLE PARTITION** creates a partitioned table. **Partitioned table**: refers to splitting what is logically one large table into smaller physical pieces based on specific schemes. The table based on the logic is called a partitioned cable, and a physical piece is called a partition. Data is stored on these smaller physical pieces, namely, partitions, instead of the larger logical partitioned table.

Common partitioning policies include range partitioning, hash partitioning, list partitioning, and value partitioning.

In range partitioning, the table is partitioned into ranges defined by a key column or set of columns, with no overlap between the ranges of values assigned to different partitions. Each range has a dedicated partition for data storage.

Range partitioning maps data to partitions based on ranges of values of the partitioning key that you establish for each partition. This is the most commonly used partitioning policy. Currently, range partitioning only allows the use of the range partitioning policy.

List partitioning allocates records to partitions based on the key values in each partition. The key values do not overlap in different partitions. Create a partition for each group of key values to store corresponding data.

Range partitioning policy: Data is mapped to a created partition based on the partition key value. If the data can be mapped to, it is inserted into the specific partition; if it cannot be mapped to, error messages are returned.

In common partitioning policies, a data distribution range is defined based on one or more columns, and each partition carries data of a range. These columns are called partition keys.

.. note::

   -  Currently, row-store tables and column-store tables support only range partitioning and list partitioning.
   -  List partitioning is supported only by clusters of 8.1.3 and later versions.

Advantages of Partitioning
--------------------------

-  The performance of some types of queries can be greatly improved, especially when rows with high access rates in a table are located in a single partition or a few partitions. Partitioning narrows the range of data search and improves data access efficiency.
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
       [ DISTRIBUTE BY { REPLICATION | ROUNDROBIN | { [ HASH ] ( column_name ) } } ]
       [ TO { GROUP groupname | NODE ( nodename [, ... ] ) } ]
       PARTITION BY {
           {VALUES (partition_key)} |
           {RANGE (partition_key) ( partition_less_than_item [, ... ] )} |
           {RANGE (partition_key) ( partition_start_end_item [, ... ] )} |
           {LIST (partition_key) (list_partition_item [, ...])}
       } [ { ENABLE | DISABLE } ROW MOVEMENT ];

-  **column_constraint** is as follows:

   ::

      [ CONSTRAINT constraint_name ]
      { NOT NULL |
        NULL |
        CHECK ( expression ) |
        DEFAULT default_expr |
        UNIQUE [ NULLS [NOT] DISTINCT | NULLS IGNORE ] index_parameters |
        PRIMARY KEY index_parameters }
      [ DEFERRABLE | NOT DEFERRABLE | INITIALLY DEFERRED | INITIALLY IMMEDIATE ]

-  **table_constraint** is as follows:

   ::

      [ CONSTRAINT constraint_name ]
      { CHECK ( expression ) |
        UNIQUE [ NULLS [NOT] DISTINCT | NULLS IGNORE ] ( column_name [, ... ] ) index_parameters |
        PRIMARY KEY ( column_name [, ... ] ) index_parameters}
      [ DEFERRABLE | NOT DEFERRABLE | INITIALLY DEFERRED | INITIALLY IMMEDIATE ]

-  **like_option** is as follows:

   ::

      { INCLUDING | EXCLUDING } { DEFAULTS | CONSTRAINTS | INDEXES | STORAGE | COMMENTS | RELOPTIONS | DISTRIBUTION | ALL }

-  **index_parameters** is as follows:

   ::

      [ WITH ( {storage_parameter = value} [, ... ] ) ]

-  .. _en-us_topic_0000001460561364__li1147714355320:

   **partition_less_than_item** is as follows:

   ::

      PARTITION partition_name VALUES LESS THAN ( { partition_value | MAXVALUE } )

-  **partition_start_end_item** is as follows:

   ::

      PARTITION partition_name {
              {START(partition_value) END (partition_value) EVERY (interval_value)} |
              {START(partition_value) END ({partition_value | MAXVALUE})} |
              {START(partition_value)} |
              {END({partition_value | MAXVALUE})}
      }

-  .. _en-us_topic_0000001460561364__li135021622911:

   list_partition_item:

   ::

      PARTITION partition_name VALUES ( { (partition_value) [, ...] | DEFAULT } )

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

   Columns and constraints copied by **LIKE** are not merged with the same name. If the same name is specified explicitly or in another **LIKE** clause, an error is reported.

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

      The valid values for column-store tables are **YES**/**NO** and **LOW**/**MIDDLE**/**HIGH**, and the default is **LOW**.

      .. note::

         Currently, row-store table compression is not supported.

   -  MAX_BATCHROW

      Specifies the maximum of a storage unit during data loading process. The parameter is only valid for column-store tables.

      Value range: 10000 to 60000

      Default value: **60000**

   -  PARTIAL_CLUSTER_ROWS

      Specifies the number of records to be partial cluster stored during data loading process. The parameter is only valid for column-store tables.

      Value range: The valid value is no less than 100000. The value is the multiple of **MAX_BATCHROW**.

   -  enable_delta

      Specifies whether to enable delta tables in column-store tables. The parameter is only valid for column-store tables.

      Default value: **off**

   -  DELTAROW_THRESHOLD

      A reserved parameter. The parameter is only valid for column-store tables.

      The value ranges from **0** to **60000**. The default value is **6000**.

   -  COLD_TABLECPACE

      Specifies the OBS tablespace for the cold partitions in a multi-temperature table. This parameter is available only to partitioned column-store tables and cannot be modified. It must be used together with **storage_policy**. The parameter **STORAGE_POLICY** can be left unconfigured. In this case, the default value **default_obs_tbs** is used.

      Valid value: a valid OBS tablespace name

   -  STORAGE_POLICY

      Specifies the rule for switching between hot and cold partitions. This parameter is used only for multi-temperature tables. It must be used together with **cold_tablespace**.

      Value range: *Cold and hot switchover policy name*:*Cold and hot switchover threshold*. Currently, only LMT and HPN policies are supported. LMT indicates that the switchover is performed based on the last update time of partitions. HPN indicates the switchover is performed based on a fixed number of reserved hot partitions.

      -  **LMT:[**\ *day*\ **]**: Switch the hot partition data that is not updated in the last *[day]* days to the OBS tablespace as cold partition data. *[day]* is an integer ranging from 0 to 36500, in days.
      -  **HPN:[**\ *hot_partition_num*\ **]**: [*hot_partition_num*] indicates the number of hot partitions (with data) to be retained. The rule is to find the maximum sequence ID of the partitions with data. The partitions without data whose sequence ID is greater than the maximum sequence ID are hot partitions, and [*hot_partition_num*] partitions are retained as hot partitions in descending order according to the sequence ID. A partition whose sequence ID is smaller than the minimum sequence ID of the retained hot partition is a cold partition. During hot and cold partition switchover, data needs to be migrated to the OBS tablespace. *[hot_partition_num]* is an integer ranging from **0** to **1600**.

         .. important::

            -  The hybrid data warehouse (standalone) does not support cold and hot partition switchover.
            -  For a LIST partition, you are advised to use the HPN policy with caution. Otherwise, the new partition may not be a hot partition.

   -  .. _en-us_topic_0000001460561364__li672910401685:

      PERIOD

      Specifies the period of automatically creating partitions and enables the automatic partition creation function. Only row-store and column-store range partitioned tables, time series tables, and cold and hot tables are supported. The partition key must be unique and its type can only be TIMESTAMP[(p)] [WITHOUT TIME ZONE], TIMESTAMP[(p)] [WITH TIME ZONE] or DATE. maxvalue partitions are not supported. The value of **(nowTime - boundaryTime)/PERIOD** must be less than the upper limit of the number of partitions, where **nowTime** indicates the current time and **boundaryTime** indicates the earliest partition boundary time. It cannot be used on midrange computers, acceleration clusters, or single-node clusters.

      Value range: 1 hour ~ 100 years

      .. important::

         -  In a database compatible with Teradata or MySQL, if the partition key type is **DATE**, **PERIOD** cannot be less than 1 day.

         -  If PERIOD is set when a partitioned table is created, you can specify only the partition key. Two default partitions are created during table creation. The time ranges of the two default partitions are both PERIOD. The boundary time of the first default partition is the first hour, day, week, month, or year past the current time. The time unit is selected based on the maximum unit of PERIOD. The boundary time of the second default partition is the boundary time of the first partition plus PERIOD. Assume that the current time is 2022-02-17 16:32:45, and the boundary of the first default partition is described in :ref:`Table 1 <en-us_topic_0000001460561364__table1398181711544>`.

            For more information about the default partitions, see :ref:`Example 8 <en-us_topic_0000001460561364__li1238894117165>`.

         -  The hybrid data warehouse (standalone) does not support automatic partition creation.

      .. _en-us_topic_0000001460561364__table1398181711544:

      .. table:: **Table 1** Partition boundaries

         ======= =================== ===================================
         period  Maximum PERIOD Unit Boundary of First Default Partition
         ======= =================== ===================================
         1hour   Hour                2022-02-17 17:00:00
         1day    Day                 2022-02-18 00:00:00
         1month  Month               2022-03-01 00:00:00
         13month Year                2023-01-01 00:00:00
         ======= =================== ===================================

   -  .. _en-us_topic_0000001460561364__li49277207810:

      TTL

      Specifies the partition expiration time in partition management and enables the automatic partition deletion function. This parameter cannot be set separately. You must set **PERIOD** in advance or at the same time. The value of this parameter must be greater than or equal to that of **PERIOD**.

      Value range: 1 hour ~ 100 years

      .. note::

         -  PERIOD indicates that data is partitioned by time period. The partition size may affect the query performance. The :ref:`proc_add_partition (relname regclass, boundaries_interval interval) <en-us_topic_0000001510281985__en-us_topic_0000001444998754_section9462151915274>` function is automatically invoked to create a partition after each period. Time To Live (TTL) specifies the data storage period of the table. The data that exceeds the TTL period will be cleared. To do this, the :ref:`proc_drop_partition (relname regclass, older_than interval) <en-us_topic_0000001510281985__en-us_topic_0000001444998754_section9128833152714>` function is automatically invoked based on the period. The **PERIOD** and **TTL** values are of the Interval type, for example, **1 hour**, **1 day**, **1 week**, **1 month**, **1 year**, and **1 month 2 day 3 hour**.
         -  The hybrid data warehouse (standalone) does not support automatic partition deletion.

   -  COLVERSION

      Specifies the version of the column-store format. Switching between different storage formats is supported. However, the storage format of a partitioned table cannot be switched.

      Valid value:

      **1.0**: Each column in a column-store table is stored in a separate file. The file name is **relfilenode.C1.0**, **relfilenode.C2.0**, **relfilenode.C3.0**, or similar.

      **2.0**: All columns of a column-store table are combined and stored in a file. The file is named **relfilenode.C1.0**.

      Default value: **2.0**

      The value of **COLVERSION** can only be set to **2.0** for OBS multi-temperature tables.

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

-  **DISTRIBUTE BY**

   Specifies how the table is distributed or replicated between DNs.

   Valid value:

   -  **REPLICATION**: Each row in the table exists on all DNs, that is, each DN has complete table data.
   -  **ROUNDROBIN**: Each row in the table is sent to each DN in turn. Therefore, data is evenly distributed on each DN. This value is supported only in 8.1.2 or later.
   -  **HASH (column_name)**: Each row of the table will be placed into all the DNs based on the hash value of the specified column.

   .. important::

      -  When **DISTRIBUTE BY HASH (column_name)** is specified, the primary key and its unique index must contain the **column_name** column.
      -  When **DISTRIBUTE BY HASH (column_name)** in a referenced table is specified, the foreign key of the reference table must contain the **column_name** column.

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

-  **TO { GROUP groupname \| NODE ( nodename [, ... ] ) }**

   **TO GROUP** specifies the Node Group in which the table is created. Currently, it cannot be used for HDFS tables. **TO NODE** is used for internal scale-out tools.

-  .. _en-us_topic_0000001460561364__lb144da954d4c4ac58c1e9ae1391e59ac:

   **PARTITION BY RANGE(partition_key)**

   The syntax specifying range partitioning. **partition_key** indicates the name of a partition key.

   (1) Assume that the **VALUES LESS THAN** syntax is used.

   .. important::

      In this case, a maximum of four partition keys are supported, and the partition key must be a column name. If there are multiple partition keys, a column name can appear only once, and two adjacent partition keys must be separated by a comma (,).

   Data types supported by the partition keys are as follows: SMALLINT, INTEGER, BIGINT, DECIMAL, NUMERIC, REAL, DOUBLE PRECISION, CHARACTER VARYING(n), VARCHAR(n), CHARACTER(n), CHAR(n), CHARACTER, CHAR, TEXT, NVARCHAR2, NAME, TIMESTAMP[(p)] [WITHOUT TIME ZONE], TIMESTAMP[(p)] [WITH TIME ZONE], and DATE.

   (2) Assume that the **START END** syntax is used.

   .. important::

      In this case, only one partition key is supported.

   Data types supported by the partition key are as follows: SMALLINT, INTEGER, BIGINT, DECIMAL, NUMERIC, REAL, DOUBLE PRECISION, TIMESTAMP[(p)] [WITHOUT TIME ZONE], TIMESTAMP[(p)] [WITH TIME ZONE], and DATE.

-  **PARTITION BY LIST (partition_key,[...])**

   The syntax specifying list partitioning. **partition_key** indicates the name of a partition key.

   .. important::

      In list partitioning, a partition key has a maximum of four columns.

   In list partitioning, the partition key supports the following data types: TINYINT, SMALLINT, INTEGER, BIGINT, NUMERIC/DECIMAL, TEXT, NVARCHAR2, VARCHAR(n), CHAR, BPCHAR, TIME, TIME WITH TIMEZONE, TIMESTAMP, TIMESTAMP WITH TIME ZONE, DATE, INTERVAL and SMALLDATETIME.

-  **partition_less_than_item**

   ::

      PARTITION partition_name VALUES LESS THAN ( { partition_value | DEFAULT } )

   Partition definition syntax in range partitioning. **partition_name** is the name of a range partition. **partition_value** is the upper limit of range partition, and the value depends on the type of **partition_key**. **MAXVALUE** can specify the upper boundary of a range partition, and it is commonly used to specify the upper boundary of the last range partition.

   .. important::

      -  Upper boundaries must be specified for each partition.
      -  The types of upper boundaries must be the same as those of partition keys.
      -  In a partition list, partitions are arranged in ascending order of upper boundary values. Therefore, a partition with a certain upper boundary value is placed before another partition with a larger upper boundary value.
      -  If a partition key consists of multiple columns, the columns are used for partitioning in sequence. The first column is preferred to be used for partitioning. If the values of the first columns are the same, the second column is used. The subsequent columns are used in the same manner.

-  .. _en-us_topic_0000001460561364__li2094151861116:

   **partition_start_end_item**

   ::

      PARTITION partition_name {START (partition_value) END (partition_value) EVERY (interval_value)}
                             | {START (partition_value) END (partition_value|MAXVALUE)}
                             | {START(partition_value)}
                             | {END (partition_value| MAXVALUE)}

   The syntax of using the start value and interval value to define a range partition. The parameters are described as follows:

   -  **partition_name**: name or name prefix of a range partition. It is the name prefix only in the following cases (assuming that **partition_name** is **p1**):

      -  Using START+END+EVERY will result in partition names such as **p1_1**, **p1_2**, and the like. For example, if **PARTITION p1 START(1) END(4) EVERY(1)** is defined, the generated partitions are [1, 2), [2, 3), and [3, 4), and their names are **p1_1**, **p1_2**, and **p1_3**. In this case, **p1** is a name prefix.
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

-  list_partition_item

   ::

      PARTITION partition_name VALUES ( { (partition_value) [, ... ] | DEFAULT } )

   Partition definition syntax in list partitioning. **partition_name** indicates the partition name. **partition_value** is an enumerated value of the list partition boundary. The value depends on the type of **partition_key**. **DEFAULT** indicates the default partition boundary.

   .. important::

      The following conventions and constraints apply to list partitioned tables:

      -  .. _en-us_topic_0000001460561364__li105701736194813:

         The partition whose boundary value is **DEFAULT** is the default partition.

      -  Each list partitioned table can have only one DEFAULT partition.

      -  The number of partitions in a partitioned table cannot exceed 32767, and the number of boundary values of all partitions cannot exceed 32767.

      -  Regardless of the number of partition keys, the boundary of the DEFAULT partition can only be DEFAULT.

      -  If a partition key consists of multiple columns, each **partition_value** must contain the values of all partition keys. If a partition key contains only one column, the parentheses on both sides of **partition_value** can be omitted. For details, see :ref:`Example 4: Creating a List Partition <en-us_topic_0000001460561364__li72564306344>`.

      -  If the partition key consists of multiple columns, compare the values in the columns one by one. If a value is different from another value, regardless of their columns, they are different values.

      -  Each value of **partition_value** must be unique.

      -  When data is inserted, if its partition key and value falls into the boundary of a non-DEFAULT partition, the data is written to the partition. Otherwise, the data is written to the DEFAULT partition.

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

-  **UNIQUE [ NULLS [NOT] DISTINCT \| NULLS IGNORE ] index_parameters**

   **UNIQUE [ NULLS [NOT] DISTINCT \| NULLS IGNORE ] ( column_name [, ... ] ) index_parameters**

   Specifies that a group of one or more columns of a table can contain only unique values.

   The **[ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ]** field is used to specify how to process null values in the index column of the Unique index.

   Default value: NULL can be inserted repeatedly.

   When the inserted data is compared with the original data in the table, the NULL value can be processed in any of the following ways:

   -  NULLS DISTINCT: NULL values are unequal and can be inserted repeatedly.
   -  NULLS NOT DISTINCT: NULL values are equal. If all index columns are NULL, NULL values cannot be inserted repeatedly. If some index columns are NULL, data can be inserted only when non-null values are different.
   -  NULLS IGNORE: NULL values are skipped during the equivalent comparison. If all index columns are NULL, NULL values can be inserted repeatedly. If some index columns are NULL, data can be inserted only when non-null values are different.

   The following table lists the behaviors of the three processing modes.

   .. table:: **Table 2** Processing of NULL values in index columns in unique indexes

      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | Constraint         | All Index Columns Are NULL     | Some Index Columns Are NULL                                                                                |
      +====================+================================+============================================================================================================+
      | NULLS DISTINCT     | Can be inserted repeatedly.    | Can be inserted repeatedly.                                                                                |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | NULLS NOT DISTINCT | Cannot be inserted repeatedly. | Cannot be inserted if the non-null values are equal. Can be inserted if the non-null values are not equal. |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | NULLS IGNORE       | Can be inserted repeatedly.    | Cannot be inserted if the non-null values are equal. Can be inserted if the non-null values are not equal. |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+

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

Examples
--------

-  Example 1: Use the **LESS THAN** syntax to create a range partitioned table.

   The range partitioned table **customer_address** has four partitions and their partition keys are of the integer type. The ranges of the partitions are as follows: **ca_address_sk** < 2450815, 2450815 <= **ca_address_sk** < 2451179, 2451179 <= **ca_address_sk** < 2451544, 2451544 <= **ca_address_sk**.

   ::

      CREATE TABLE customer_address
      (
          ca_address_sk       INTEGER                  NOT NULL   ,
          ca_address_id       CHARACTER(16)            NOT NULL   ,
          ca_street_number    CHARACTER(10)                       ,
          ca_street_name      CHARACTER varying(60)               ,
          ca_street_type      CHARACTER(15)                       ,
          ca_suite_number     CHARACTER(10)
      )
      DISTRIBUTE BY HASH (ca_address_sk)
      PARTITION BY RANGE(ca_address_sk)
      (
              PARTITION P1 VALUES LESS THAN(2450815),
              PARTITION P2 VALUES LESS THAN(2451179),
              PARTITION P3 VALUES LESS THAN(2451544),
              PARTITION P4 VALUES LESS THAN(MAXVALUE)
      );

   View the information of the partitioned table.

   .. code-block::

      SELECT relname, boundaries FROM pg_partition p where p.parentid='customer_address'::regclass ORDER BY 1;
           relname      | boundaries
      ------------------+------------
       customer_address |
       p1               | {2450815}
       p2               | {2451179}
       p3               | {2451544}
       p4               | {NULL}
      (5 rows)

   Query the number of rows in the **P1** partition:

   ::

      SELECT count(*) FROM customer_address PARTITION (P1);
      SELECT count(*) FROM customer_address PARTITION FOR (2450815);

-  Example 2: Use the **START END** syntax to create a column-store range partitioned table.

   .. code-block::

      CREATE TABLE customer_address_SE
      (
          ca_address_sk       INTEGER                  NOT NULL   ,
          ca_address_id       CHARACTER(16)            NOT NULL   ,
          ca_street_number    CHARACTER(10)                       ,
          ca_street_name      CHARACTER varying(60)               ,
          ca_street_type      CHARACTER(15)                       ,
          ca_suite_number     CHARACTER(10)
      )
      WITH (ORIENTATION = COLUMN)
      DISTRIBUTE BY HASH (ca_address_sk)
      PARTITION BY RANGE(ca_address_sk)
      (
          PARTITION p1 START(1) END(1000) EVERY(200),
          PARTITION p2 END(2000),
          PARTITION p3 START(2000) END(5000)
      );

   View the information of the partitioned table.

   .. code-block::

      SELECT relname, boundaries FROM pg_partition p where p.parentid='customer_address_SE'::regclass ORDER BY 1;
           relname      | boundaries
      ---------------------+------------
       customer_address_se |
       p1_0                | {1}
       p1_1                | {201}
       p1_2                | {401}
       p1_3                | {601}
       p1_4                | {801}
       p1_5                | {1000}
       p2                  | {2000}
       p3                  | {5000}
      (9 rows)

-  Example 3: Create a list partitioned table with partition keys.

   ::

      CREATE TABLE data_list
      (
          id int,
          time int,
          sarlay decimal(12,2)
      )
      PARTITION BY LIST (time)
      (
              PARTITION P1 VALUES (202209),
              PARTITION P2 VALUES (202210,202208),
              PARTITION P3 VALUES (202211),
              PARTITION P4 VALUES (202212),
              PARTITION P5 VALUES (202301)
      );

-  .. _en-us_topic_0000001460561364__li72564306344:

   Example 4: Create list partitioned tables with partition keys.

   A partitioned table has two partition keys: **period** and **city**.

   ::

      CREATE TABLE sales_info
      (
      sale_time  timestamptz,
      period     int,
      city       text,
      price      numeric(10,2),
      remark     varchar2(100)
      )
      DISTRIBUTE BY HASH(sale_time)
      PARTITION BY LIST (period, city)
      (
      PARTITION north_2022 VALUES (('202201', 'north1'), ('202202', 'north2')),
      PARTITION south_2022 VALUES (('202201', 'south1'), ('202202', 'south2'), ('202203', 'south2')),
      PARTITION rest VALUES (DEFAULT)
      );

-  Example 5: Create a partitioned table with automatic partition management but without specified partitions. Set **PERIOD** to 1 day and the partition key to **time**.

   ::

      CREATE TABLE time_part
       (
          id integer,
          time timestamp
       ) with (PERIOD='1 day')
       partition by range(time);

   Two default partitions are created during table creation. The boundary time of the first default partition is the start of the day later than the current time, that is, 2022-12-13 00:00:00. The boundary time of the second default partition is the boundary time of the first partition plus PERIOD, that is, 2022-12-13 00:00:00+1 day=2022-12-14 00:00:00.

   ::

      SELECT now();
                    now
      -------------------------------
       2022-12-12 20:41:21.603172+08
      (1 row)

      SELECT relname, boundaries FROM pg_partition p where p.parentid='time_part'::regclass ORDER BY 1;
          relname     |       boundaries
      ----------------+-------------------------
       default_part_1 | {"2022-12-13 00:00:00"}
       default_part_2 | {"2022-12-14 00:00:00"}
       time_part      |
      (3 rows)

-  Example 6: Run the following command to create a partitioned table with automatic partition management and specified partitions:

   ::

      CREATE TABLE CPU(
          id integer,
          idle numeric,
          IO numeric,
          scope text,
          IP text,
          time timestamp
      ) with (TTL='7 days',PERIOD='1 day')
      partition by range(time)
      (
          PARTITION P1 VALUES LESS THAN('2022-01-05 16:32:45'),
          PARTITION P2 VALUES LESS THAN('2022-01-06 16:56:12')
      );

-  Example 7: Create a partitioned table **customer_address** partitioned by month. The table has 13 partitions and the partition keys are dates.

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

-  .. _en-us_topic_0000001460561364__li1238894117165:

   Example 8: Use **START END** to create a partitioned table with multiple partitions at a time.

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

-  Example 9: Create a table for hot and cold data.

   Only a column-store partitioned table is supported. Use the default OBS tablespace. Set LMT to 30 for cold and hot switchover rules.

   ::

      CREATE TABLE cold_hot_table
      (
          W_WAREHOUSE_ID            CHAR(16)              NOT NULL,
          W_WAREHOUSE_NAME          VARCHAR(20)                   ,
          W_STREET_NUMBER           CHAR(10)                      ,
          W_STREET_NAME             VARCHAR(60)                   ,
          W_STREET_ID               CHAR(15)                      ,
          W_SUITE_NUMBER            CHAR(10)
      )
      WITH (ORIENTATION = COLUMN, storage_policy = 'LMT:30')
      DISTRIBUTE BY HASH (W_WAREHOUSE_ID)
      PARTITION BY RANGE(W_STREET_ID)
      (
          PARTITION P1 VALUES LESS THAN(100000),
          PARTITION P2 VALUES LESS THAN(200000),
          PARTITION P3 VALUES LESS THAN(300000),
          PARTITION P4 VALUES LESS THAN(MAXVALUE)
      )ENABLE ROW MOVEMENT;

Helpful Links
-------------

:ref:`ALTER TABLE PARTITION <dws_06_0143>`, :ref:`DROP TABLE <dws_06_0208>`
