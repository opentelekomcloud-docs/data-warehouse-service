:original_name: dws_06_0153.html

.. _dws_06_0153:

CLUSTER
=======

Function
--------

This syntax clusters tables by their indexes.

-  **CLUSTER** clusters the table specified by table name based on the index specified by index name. The index specified by index name must have been defined in the specified table.
-  When a table is clustered, it is physically reordered based on the index information. Clustering is a one-time operation: when the table is subsequently updated, the changes are not clustered. That is, no attempt is made to store new or updated rows according to their index order.
-  When a table is clustered, DWS records which index the table was clustered by. The form **CLUSTER table_name** reclusters the table using the same index used earlier. You can also use **SET WITHOUT CLUSTER** of **CLUSTER** or **ALTER TABLE** to set indexes for subsequent clustering operations or clear any previous settings.
-  **CLUSTER** without any parameters reclusters all tables that have been clustered in the database owned by the current user. If **CLUSTER** is invoked by the system administrator, it indicates all tables that have been clustered.
-  When a table is being clustered, an **ACCESS EXCLUSIVE** lock is acquired on it. This prevents any other database operations (both reads and writes) from operating on the table until the clustering is complete.

Precautions
-----------

-  **CLUSTER** cannot be executed in transactions.
-  **CLUSTER** cannot be performed on HDFS tables.
-  **CLUSTER** cannot be performed on global temporary tables.
-  Performing a **CLUSTER** operation on a table triggers a rebuilding process where data is dumped into a new data file. Once the process is complete, the original file is deleted. Keep in mind that if the table is large, this process can consume a significant amount of disk space. When the disk space is insufficient, exercise caution when performing the **CLUSTER** operation on large tables to prevent the cluster from being read-only.
-  In the storage-compute decoupled architecture, a message is displayed indicating that operations on the entire cluster database are not supported.

-  Only row-store B-tree indexes support **CLUSTER**.
-  In cases where you are accessing single rows randomly within a table, the actual order of the data in the table is unimportant. However, if you tend to access some data more than others, and there is an index that groups them together, you will benefit from using **CLUSTER**. If you request a range of index values from a table or if an index value matches multiple rows, **CLUSTER** helps. It does this by storing related rows on the same page. This saves disk access time and speeds up queries.

-  During clustering, the system creates a temporary copy of the table in the index sequence and a temporary copy of each index in the table. The disk must have enough space for both the table and its index.
-  CLUSTER stores clustering information. You can manually cluster tables initially, then set up a periodic maintenance script. This allows tables to be automatically clustered regularly.

-  Because the optimizer records statistics about the ordering of tables, it is advisable to run **ANALYZE** on the newly clustered table. Otherwise, the optimizer might select an incorrect query plan.

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

.. table:: **Table 1** CLUSTER parameters

   +----------------+-------------------------------------------+--------------------------------+
   | Parameter      | Description                               | Value Range                    |
   +================+===========================================+================================+
   | VERBOSE        | Enables the display of progress messages. | ``-``                          |
   +----------------+-------------------------------------------+--------------------------------+
   | table_name     | Specifies the name of the table.          | Name of an existing table.     |
   +----------------+-------------------------------------------+--------------------------------+
   | index_name     | Specifies the index name.                 | Name of an existing index.     |
   +----------------+-------------------------------------------+--------------------------------+
   | partition_name | Specifies the partition name.             | Name of an existing partition. |
   +----------------+-------------------------------------------+--------------------------------+

Examples
--------

Create a partitioned table.

::

   DROP SCHEMA IF EXISTS tpcds CASCADE;
   CREATE SCHEMA tpcds;
   CREATE TABLE tpcds.inventory_p1
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

   CREATE INDEX ds_inventory_p1_index1 ON tpcds.inventory_p1 (INV_ITEM_SK) LOCAL;

Cluster the **tpcds.inventory_p1** table.

::

   CLUSTER tpcds.inventory_p1 USING ds_inventory_p1_index1;

Cluster the **p3** partition.

::

   CLUSTER tpcds.inventory_p1 PARTITION (p3) USING ds_inventory_p1_index1;

Cluster the tables in the database.

::

   CLUSTER;
