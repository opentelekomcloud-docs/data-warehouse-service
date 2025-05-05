:original_name: dws_06_0165.html

.. _dws_06_0165:

CREATE INDEX
============

Function
--------

**CREATE INDEX** creates an index in a specified table.

Indexes are primarily used to enhance database performance (though inappropriate use can result in slower database performance). You are advised to create indexes on:

-  Columns that are often queried
-  Join conditions. For a query on joined columns, you are advised to create a composite index on the columns, for example, **select \* from t1 join t2 on t1.a=t2.a and t1.b=t2.b**. You can create a composite index on the **a** and **b** columns of table **t1**.
-  Columns having filter criteria (especially scope criteria) of a **where** clause
-  Columns that appear after **order by**, **group by**, and **distinct**.
-  Create a B-tree index for point queries.

The partitioned table does not support concurrent index creation, partial index creation, and **NULL FIRST**.

Precautions
-----------

-  Indexes consume storage and computing resources. Creating too many indexes has negative impact on database performance (especially the performance of data import. Therefore, you are advised to import the data before creating indexes). Create indexes only when they are necessary.
-  All functions and operators used in an index definition must be immutable, that is, their results must depend only on their arguments and never on any outside influence (such as the contents of another table or the current time). This restriction ensures that the behavior of the index is well-defined. When using a user-defined function on an index or in a **WHERE** statement, mark it as an immutable function.
-  A unique index created on a partitioned table must include a partition column and all the partition keys.
-  GaussDB(DWS) can only create local indexes on partitioned tables.
-  Column-store tables and HDFS tables support B-tree indexes. If the B-tree indexes are used, you cannot create expression and partial indexes.
-  Column-store tables support creating unique indexes using B-tree indexes.
-  Column-store and HDFS tables support psort indexes. If the psort indexes are used, you cannot create expression, partial, and unique indexes.
-  Column-store tables support GIN indexes, rather than partial indexes and unique indexes. If GIN indexes are used, you can create expression indexes. However, an expression in this situation cannot contain empty splitters, empty columns, or multiple columns.
-  Column-store indexes do not support **OR** query filter criteria and **inlist** operations.
-  A primary key or unique index cannot be created for a round-robin table.
-  Do not specify a tablespace when creating an unlogged table index.
-  Performing **CREATE INDEX** or **REINDEX** operations on a table triggers index rebuilding. During this process, data is dumped to a new data file, and once rebuilding is complete, the original file is deleted. For large tables, index rebuilding can consume a significant amount of disk space. Exercise caution when disk space is insufficient to prevent the cluster from becoming read-only.

.. warning::

   -  For a table where a large amount of data needs to be added, deleted, or modified, it is recommended that the number of indexes be less than or equal to three. The maximum number of indexes is five.
   -  Do not perform the **CREATE INDEX** and **REINDEX** operations on large tables during peak hours.
   -  For more information about development and design specifications, see "GaussDB(DWS) Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

Syntax
------

-  Create an index on a table.

   ::

      CREATE [ UNIQUE ] INDEX [ [IF NOT EXISTS] [ schema_name. ] index_name ] ON table_name [ USING method ]
          ({ { column_name | ( expression ) } [ COLLATE collation ] [ opclass ] [ ASC | DESC ] [ NULLS { FIRST | LAST } ] }[, ...] )
          [ NULLS [ NOT ] DISTINCT | NULLS IGNORE ]
          [ COMMENT 'text' ]
          [ WITH ( {storage_parameter = value} [, ... ] ) ]

          [ WHERE predicate ];

-  Create an index for a partitioned table.

   ::

      CREATE [ UNIQUE ] INDEX [ [IF NOT EXISTS] [ schema_name. ] index_name ] ON table_name [ USING method ]
          ( {{ column_name | ( expression ) } [ COLLATE collation ] [ opclass ] [ ASC | DESC ] [ NULLS LAST ] }[, ...] )
          [ NULLS [ NOT ] DISTINCT | NULLS IGNORE ]
          [ COMMENT 'text' ]
          LOCAL [ ( { PARTITION index_partition_name  } [, ...] ) ]
          [ WITH ( { storage_parameter = value } [, ...] ) ]
          ;

Parameters
----------

-  **UNIQUE**

   Causes the system to check for duplicate values in the table when the index is created (if data exists) and each time data is added. Attempts to insert or update data which would result in duplicate entries will generate an error.

   Currently, only B-tree indexes of row-store tables and column-store tables support unique indexes.

-  **IF NOT EXISTS**

   If **IF NOT EXISTS** is specified and no index with the same name exists, the index can be created. If an index with the same name already exists, the system will simply display a message stating that the index already exists and will not perform any additional operations during creation. When **IF NOT EXISTS** is used, the index name must be specified. This parameter is supported only by clusters of version 9.1.0 or later.

-  **schema_name**

   Name of the schema where the index to be created is located. The specified schema name must be the same as the schema of the table.

-  **index_name**

   Specifies the name of the index to be created. The schema of the index is the same as that of the table.

   Value range: a string. It must comply with the naming convention.

-  **table_name**

   Specifies the name of the table to be indexed (optionally schema-qualified).

   Value range: an existing table name

-  **USING method**

   Specifies the name of the index method to be used.

   Valid value:

   -  **btree**: The B-tree index uses a structure that is similar to the B+ tree structure to store data key values, facilitating index search. **btree** supports comparison queries with ranges specified.
   -  **gin**: GIN indexes are reverse indexes and can process values that contain multiple keys (for example, arrays).
   -  **gist**: GiST indexes are suitable for the set data type and multidimensional data types, such as geometric and geographic data types.
   -  **Psort**: psort index. It is used to perform partial sort on column-store tables.

   Row-based tables support the following index types: **btree** (default), **gin**, and **gist**. Column-based tables support the following index types: **Psort** (default), **btree**, and **gin**.

-  **column_name**

   Specifies the name of a column of the table.

   Multiple columns can be specified if the index method supports multi-column indexes. A maximum of 32 columns can be specified.

-  **expression**

   Specifies an expression based on one or more columns of the table. The expression usually must be written with surrounding parentheses, as shown in the syntax. However, the parentheses can be omitted if the expression has the form of a function call.

   Expression can be used to obtain fast access to data based on some transformation of the basic data. For example, an index computed on upper(col) would allow the clause WHERE upper(col) = 'JIM' to use an index.

   If an expression contains **IS NULL**, the index for this expression is invalid. In this case, you are advised to create a partial index.

-  **COLLATE collation**

   Assigns a collation to the column (which must be of a collatable data type). If no collation is specified, the default collation is used.

-  **opclass**

   Specifies the name of an operator class. Specifies an operator class for each column of an index. The operator class identifies the operators to be used by the index for that column. For example, a B-tree index on the type int4 would use the **int4_ops** class; this operator class includes comparison functions for values of type int4. In practice, the default operator class for the column's data type is sufficient. The operator class applies to data with multiple sorts. For example, we might want to sort a complex-number data type either by absolute value or by real part. We could do this by defining two operator classes for the data type and then selecting the proper class when making an index.

-  **ASC**

   Indicates ascending sort order (default). This option is supported only by row storage.

-  **DESC**

   Indicates descending sort order. This option is supported only by row storage.

-  **NULLS FIRST**

   Specifies that nulls sort before not-null values. This is the default when **DESC** is specified.

-  **NULLS LAST**

   Specifies that nulls sort after not-null values. This is the default when **DESC** is not specified.

-  NULLS [ NOT ] DISTINCT \| NULLS IGNORE

   Specifies how NULL values of index columns in a Unique index are processed.

   Default value: This parameter is left empty by default. NULL values can be inserted repeatedly.

   When the inserted data is compared with the original data in the table, the NULL value can be processed in any of the following ways:

   -  NULLS DISTINCT: NULL values are unequal and can be inserted repeatedly.
   -  NULLS NOT DISTINCT: NULL values are equal. If all index columns are NULL, NULL values cannot be inserted repeatedly. If some index columns are NULL, data can be inserted only when non-null values are different.
   -  NULLS IGNORE: NULL values are skipped during the equivalent comparison. If all index columns are NULL, NULL values can be inserted repeatedly. If some index columns are NULL, data can be inserted only when non-null values are different.

   The following table lists the behaviors of the three processing modes.

   .. table:: **Table 1** Processing of NULL values in index columns in unique indexes

      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | Constraint         | All Index Columns Are NULL     | Some Index Columns Are NULL.                                                                               |
      +====================+================================+============================================================================================================+
      | NULLS DISTINCT     | Can be inserted repeatedly.    | Can be inserted repeatedly.                                                                                |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | NULLS NOT DISTINCT | Cannot be inserted repeatedly. | Cannot be inserted if the non-null values are equal. Can be inserted if the non-null values are not equal. |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | NULLS IGNORE       | Can be inserted repeatedly.    | Cannot be inserted if the non-null values are equal. Can be inserted if the non-null values are not equal. |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+

-  **COMMENT 'text'**

   Specifies the comment of an index.

-  **WITH ( {storage_parameter = value} [, ... ] )**

   Specifies the name of an index-method-specific storage parameter.

   Valid value:

   Only the GIN index supports the **FASTUPDATE** and **GIN_PENDING_LIST_LIMIT** parameters. The indexes other than GIN and psort support the **FILLFACTOR** parameter. All indexes support the **INVISIBLE** parameter.

   -  FILLFACTOR

      The fillfactor for an index is a percentage between 10 and 100.

      Value range: 10-100

   -  FASTUPDATE

      Specifies whether fast update is enabled for the GIN index.

      Valid value: **ON** and **OFF**

      Default: **ON**

   -  GIN_PENDING_LIST_LIMIT

      Specifies the maximum capacity of the pending list of the GIN index when fast update is enabled for the GIN index.

      Value range: 64-INT_MAX. The unit is KB.

      Default value: The default value of **gin_pending_list_limit** depends on **gin_pending_list_limit** specified in GUC parameters. By default, the value is **4** MB.

   -  INVISIBLE

      Controls whether the optimizer generates index scan plans.

      Value range:

      -  **ON** indicates that no index scan plan is generated.
      -  **OFF** indicates that an index scan plan is generated.

      Default value: **OFF**

-  **WHERE predicate**

   Creates a partial index. A partial index is an index that contains entries for only a portion of a table, usually a portion that is more useful for indexing than the rest of the table. For example, if you have a table that contains both billed and unbilled orders where the unbilled orders take up a small fraction of the total table and yet that is an often used section, you can improve performance by creating an index on just that portion. Another possible application is to use **WHERE** with **UNIQUE** to enforce uniqueness over a subset of a table.

   Value range: predicate expression can refer only to columns of the underlying table, but it can use all columns, not just the ones being indexed. Presently, subquery and aggregate expressions are also forbidden in **WHERE**.

-  **PARTITION index_partition_name**

   Specifies the name of the index partition.

   Value range: a string. It must comply with the naming convention.

Examples
--------

-  Create a sample table named **tpcds.ship_mode_t1**.

   ::

      CREATE TABLE tpcds.ship_mode_t1
      (
          SM_SHIP_MODE_SK           INTEGER               NOT NULL,
          SM_SHIP_MODE_ID           CHAR(16)              NOT NULL,
          SM_TYPE                   CHAR(30)                      ,
          SM_CODE                   CHAR(10)                      ,
          SM_CARRIER                CHAR(20)                      ,
          SM_CONTRACT               CHAR(20)
      )
      DISTRIBUTE BY HASH(SM_SHIP_MODE_SK);

   Create a unique index on the **SM_SHIP_MODE_SK** column in the **tpcds.ship_mode_t1** table.

   ::

      CREATE UNIQUE INDEX ds_ship_mode_t1_index1 ON tpcds.ship_mode_t1(SM_SHIP_MODE_SK);

   Create a UNIQUE index on the **SM_SHIP_MODE_SK** column in the **tpcds.ship_mode_t1** table and specify how to process null values.

   ::

      CREATE UNIQUE INDEX ds_ship_mode_t1_index5 ON tpcds.ship_mode_t1(SM_SHIP_MODE_SK) NULLS NOT DISTINCT;

   Add comment to the index when creating an index on the **SM_SHIP_MODE_SK** column of table **tpcds.ship_mode_t1**.

   ::

      CREATE INDEX ds_ship_mode_t1_index_comment ON tpcds.ship_mode_t1(SM_SHIP_MODE_SK) COMMENT 'index';

   Create a B-tree index on the **SM_SHIP_MODE_SK** column in the **tpcds.ship_mode_t1** table.

   ::

      CREATE INDEX ds_ship_mode_t1_index4 ON tpcds.ship_mode_t1 USING btree(SM_SHIP_MODE_SK);

   Create an expression index on the **SM_CODE** column in the **tpcds.ship_mode_t1** table.

   ::

      CREATE INDEX ds_ship_mode_t1_index2 ON tpcds.ship_mode_t1(SUBSTR(SM_CODE,1 ,4));

   Create a partial index on the **SM_SHIP_MODE_SK** column where **SM_SHIP_MODE_SK** is greater than **10** in the **tpcds.ship_mode_t1** table.

   .. code-block::

      CREATE UNIQUE INDEX ds_ship_mode_t1_index3 ON tpcds.ship_mode_t1(SM_SHIP_MODE_SK) WHERE SM_SHIP_MODE_SK>10;

-  Create a sample table named **tpcds.customer_address_p1**.

   ::

      CREATE TABLE tpcds.customer_address_p1
      (
          CA_ADDRESS_SK             INTEGER               NOT NULL,
          CA_ADDRESS_ID             CHAR(16)              NOT NULL,
          CA_STREET_NUMBER          CHAR(10)                      ,
          CA_STREET_NAME            VARCHAR(60)                   ,
          CA_STREET_TYPE            CHAR(15)                      ,
          CA_SUITE_NUMBER           CHAR(10)                      ,
          CA_CITY                   VARCHAR(60)                   ,
          CA_COUNTY                 VARCHAR(30)                   ,
          CA_STATE                  CHAR(2)                       ,
          CA_ZIP                    CHAR(10)                      ,
          CA_COUNTRY                VARCHAR(20)                   ,
          CA_GMT_OFFSET             DECIMAL(5,2)                  ,
          CA_LOCATION_TYPE          CHAR(20)
      )
      DISTRIBUTE BY HASH(CA_ADDRESS_SK)
      PARTITION BY RANGE(CA_ADDRESS_SK)
      (
         PARTITION p1 VALUES LESS THAN (3000),
         PARTITION p2 VALUES LESS THAN (5000) ,
         PARTITION p3 VALUES LESS THAN (MAXVALUE)
      )
      ENABLE ROW MOVEMENT;

   Create the partitioned table index **ds_customer_address_p1_index1** with the name of the index partition not specified.

   ::

      CREATE INDEX ds_customer_address_p1_index1 ON tpcds.customer_address_p1(CA_ADDRESS_SK) LOCAL;

   Create the partitioned table index **ds_customer_address_p1_index2** with the name of the index partition specified.

   ::

      CREATE INDEX ds_customer_address_p1_index2 ON tpcds.customer_address_p1(CA_ADDRESS_SK) LOCAL
      (
          PARTITION CA_ADDRESS_SK_index1,
          PARTITION CA_ADDRESS_SK_index2,
          PARTITION CA_ADDRESS_SK_index3
      )
      ;

   Create the partitioned table index **ds_customer_address_p1_index_comment** and add index comments.

   ::

      CREATE INDEX ds_customer_address_p1_index_comment ON tpcds.customer_address_p1(CA_ADDRESS_SK) COMMENT 'index' LOCAL
      (
          PARTITION CA_ADDRESS_SK_index1,
          PARTITION CA_ADDRESS_SK_index2,
          PARTITION CA_ADDRESS_SK_index3
      )
      ;

Helpful Links
-------------

:ref:`ALTER INDEX <dws_06_0128>`, :ref:`DROP INDEX <dws_06_0195>`
