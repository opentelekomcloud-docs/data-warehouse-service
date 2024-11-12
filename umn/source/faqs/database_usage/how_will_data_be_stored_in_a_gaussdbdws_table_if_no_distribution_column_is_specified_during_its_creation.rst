:original_name: dws_03_2127.html

.. _dws_03_2127:

How Will Data Be Stored in a GaussDB(DWS) Table If No Distribution Column Is Specified During Its Creation?
===========================================================================================================

.. note::

   For clusters of 8.1.2 or later, you can use the GUC parameter **default_distribution_mode** to query and set the default table distribution mode.

If no distribution column is specified during table creation, data is stored as follows:

-  Scenario 1

   If the primary key or unique constraint is included during table creation, hash distribution is selected. The distribution column is the column corresponding to the primary key or unique constraint.

   ::

      CREATE TABLE warehouse1
      (
          W_WAREHOUSE_SK            INTEGER            PRIMARY KEY,
          W_WAREHOUSE_ID            CHAR(16)              NOT NULL,
          W_WAREHOUSE_NAME          VARCHAR(20)
      );
      NOTICE:  CREATE TABLE / PRIMARY KEY will create implicit index "warehouse1_pkey" for table "warehouse1"
      CREATE TABLE

      SELECT getdistributekey('warehouse1');
       getdistributekey
      ------------------
       w_warehouse_sk
      (1 row)

-  Scenario 2

   If the primary key or unique constraint is not included during table creation but there are columns whose data types can be used as distribution columns, hash distribution is selected. The distribution column is the first column whose data type can be used as a distribution column.

   ::

      CREATE TABLE warehouse2
      (
          W_WAREHOUSE_SK            INTEGER                       ,
          W_WAREHOUSE_ID            CHAR(16)              NOT NULL,
          W_WAREHOUSE_NAME          VARCHAR(20)
      );
      NOTICE:  The 'DISTRIBUTE BY' clause is not specified. Using 'w_warehouse_sk' as the distribution column by default.
      HINT:  Please use 'DISTRIBUTE BY' clause to specify suitable data distribution column.
      CREATE TABLE

      SELECT getdistributekey('warehouse2');
       getdistributekey
      ------------------
       w_warehouse_sk
      (1 row)

-  Scenario 3

   If the primary key or unique constraint is not included during table creation and no column whose data type can be used as a distribution column exists, round-robin distribution is selected.

   .. code-block::

      CREATE TABLE warehouse3
      (
          W_WAREHOUSE_ID            CHAR(16)              NOT NULL,
          W_WAREHOUSE_NAME          VARCHAR(20)
      );
      NOTICE:  The 'DISTRIBUTE BY' clause is not specified. Using 'w_warehouse_id' as the distribution column by default.
      HINT:  Please use 'DISTRIBUTE BY' clause to specify suitable data distribution column.
      CREATE TABLE

      SELECT getdistributekey('warehouse3');
       getdistributekey
      ------------------
       w_warehouse_id
      (1 row)
