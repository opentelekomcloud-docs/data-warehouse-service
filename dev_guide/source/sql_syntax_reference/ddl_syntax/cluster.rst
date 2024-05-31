:original_name: dws_06_0153.html

.. _dws_06_0153:

CLUSTER
=======

Function
--------

Cluster a table according to an index.

**CLUSTER** instructs GaussDB(DWS) to cluster the table specified by **table_name** based on the index specified by **index_name**. The index specified by **index_name** must have been defined in the specified table.

When a table is clustered, it is physically reordered based on the index information. Clustering is a one-time operation: when the table is subsequently updated, the changes are not clustered. That is, no attempt is made to store new or updated rows according to their index order.

When a table is clustered, GaussDB(DWS) records which index the table was clustered by. The form **CLUSTER table_name** reclusters the table using the same index used previously. You can also use the **CLUSTER** or **SET WITHOUT CLUSTER** forms of **ALTER TABLE** to set the index to be used for future cluster operations, or to clear any previous setting.

**CLUSTER** without any parameter reclusters all the previously-clustered tables in the current database that the calling user owns, or all such tables if called by an administrator.

When a table is being clustered, an **ACCESS EXCLUSIVE** lock is acquired on it. This prevents any other database operations (both reads and writes) from operating on the table until the **CLUSTER** is finished.

Precautions
-----------

-  Only row-store B-tree indexes support **CLUSTER**.
-  In cases where you are accessing single rows randomly within a table, the actual order of the data in the table is unimportant. However, if you tend to access some data more than others, and there is an index that groups them together, you will benefit from using **CLUSTER**. If you are requesting a range of indexed values from a table, or a single indexed value that has multiple rows that match, **CLUSTER** will help because once the index identifies the table page for the first row that matches, all other rows that match are probably already on the same table page, and so you save disk accesses and speed up the query.

-  During clustering, the system creates a temporary copy of the table created based on the index sequence and a temporary copy of each index in the table. Therefore, you need free space on disk at least equal to the sum of the table size and the index sizes.
-  Because **CLUSTER** remembers the clustering information, you can manually cluster the tables once, and then set a maintenance script that runs periodically to execute **CLUSTER** without any parameters. In this way, the tables are periodically reclustered.

-  Because the optimizer records statistics about the ordering of tables, it is advisable to run **ANALYZE** on the newly clustered table. Otherwise, the optimizer might make poor choices of query plans.
-  **CLUSTER** cannot be executed in transactions.
-  When a **CLUSTER** operation is performed on a table, it triggers table rebuilding. During this rebuilding process, data is dumped into a new data file. Once the rebuilding is complete, the original file is deleted. However, it is important to note that if the table is large, the rebuilding process can consume a significant amount of disk space. When the disk space is insufficient, exercise caution when performing the **CLUSTER** operation on large tables to prevent the cluster from being read-only.

Syntax
------

-  Cluster a table.

   ::

      CLUSTER [ VERBOSE ] table_name [ USING index_name ];

-  Cluster a partition.

   ::

      CLUSTER [ VERBOSE ] table_name PARTITION ( partition_name ) [ USING index_name ];

-  Cluster the table that has previously been clustered.

   ::

      CLUSTER [ VERBOSE ];

Parameter Description
---------------------

-  **VERBOSE**

   Enables the display of progress messages.

-  **table_name**

   Specifies the name of the table.

   Value range: an existing table name

-  **index_name**

   Name of this index

   Value range: An existing index name.

-  **partition_name**

   Specifies the partition name.

   Value range: An existing partition name.

Examples
--------

Create a partitioned table.

::

   CREATE TABLE inventory_p1
   (
       INV_DATE_SK               INTEGER               NOT NULL,
       INV_ITEM_SK               INTEGER               NOT NULL,
       INV_WAREHOUSE_SK          INTEGER               NOT NULL,
       INV_QUANTITY_ON_HAND      INTEGER
   )
   DISTRIBUTE BY HASH(INV_ITEM_SK)
   PARTITION BY RANGE(INV_DATE_SK)
   (
           PARTITION P1 VALUES LESS THAN(2451179),
           PARTITION P2 VALUES LESS THAN(2451544),
           PARTITION P3 VALUES LESS THAN(2451910),
           PARTITION P4 VALUES LESS THAN(2452275),
           PARTITION P5 VALUES LESS THAN(2452640),
           PARTITION P6 VALUES LESS THAN(2453005),
           PARTITION P7 VALUES LESS THAN(MAXVALUE)
   );

Create an index named **ds_inventory_p1_index1**.

::

   CREATE INDEX ds_inventory_p1_index1 ON inventory_p1 (INV_ITEM_SK) LOCAL;

Cluster the **inventory_p1** table.

::

   CLUSTER inventory_p1 USING ds_inventory_p1_index1;

Cluster the **p3** partition.

::

   CLUSTER inventory_p1 PARTITION (p3) USING ds_inventory_p1_index1;

Cluster the tables that can be clustered in the database.

::

   CLUSTER;
