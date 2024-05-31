:original_name: dws_06_0128.html

.. _dws_06_0128:

ALTER INDEX
===========

Function
--------

**ALTER INDEX** modifies the definition of an existing index.

Precautions
-----------

-  Only the owner of an index or a system administrator can run this statement.

Syntax
------

-  Rename a table index.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          RENAME TO new_name;

-  Modify the storage parameter of a table index.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          SET ( {storage_parameter = value} [, ... ] );

-  Reset the storage parameter of a table index.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          RESET ( storage_parameter [, ... ] ) ;

-  Set a table index or an index partition to be unavailable.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          [ MODIFY PARTITION index_partition_name ] UNUSABLE;

   .. note::

      The syntax cannot be used for column-store tables.

-  Rebuild a table index or index partition.

   ::

      ALTER INDEX index_name
          REBUILD [ PARTITION index_partition_name ];

-  Rename an index partition.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          RENAME PARTITION index_partition_name TO new_index_partition_name;

   .. note::

      **PG_OBJECT** does not support the record of the syntax when the last modification time of the index is recorded.

-  Add and modify the index comment.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          COMMENT 'text';

-  Delete the index comment.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          COMMENT '';
      ALTER INDEX [ IF EXISTS ] index_name
          COMMENT NULL;

Parameter Description
---------------------

-  **IF EXISTS**

   If the specified index does not exist, a notice instead of an error is sent.

-  **RENAME TO**

   Changes only the name of the index. There is no effect on the stored data.

-  **SET ( { STORAGE_PARAMETER = value } [, ...] )**

   Change one or more index-method-specific storage parameters. Note that the index contents will not be modified immediately by this command. You might need to rebuild the index with **REINDEX** to get the desired effects depending on parameters.

-  **RESET ( { storage_parameter } [, ...] )**

   Reset one or more index-method-specific storage parameters to the default value. Similar to the **SET** statement, **REINDEX** may be used to completely update the index.

-  **[ MODIFY PARTITION index_partition_name ] UNUSABLE**

   Sets the index on a table or index partition to be unavailable.

-  **REBUILD [ PARTITION index_partition_name ]**

   Recreates the index on a table or index partition.

-  **RENAME PARTITION**

   Renames an index partition.

-  **COMMENT comment_text**

   Adds, modifies, or deletes index comments.

-  **index_name**

   Specifies the index name to be modified.

-  **new_name**

   Specifies the new name for the index. The new index name cannot be the same as an existing table name in the database.

   Value range: a string that must comply with the identifier naming rules.

-  **storage_parameter**

   Specifies the name of an index-method-specific parameter.

-  **value**

   Specifies the new value for an index-method-specific storage parameter. This might be a number or a word depending on the parameter.

-  **new_index_partition_name**

   Specifies the new name of the index partition.

-  **index_partition_name**

   Specifies the name of the index partition.

-  **comment_text**

   Comment of an index.

Example
-------

-  Modifying Table Index

   Create a sample table named **tpcds.ship_mode_t1**.

   ::

      DROP TABLE IF EXISTS tpcds.ship_mode_t1;
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

      DROP INDEX IF EXISTS ds_ship_mode_t1_index1;
      CREATE UNIQUE INDEX ds_ship_mode_t1_index1 ON tpcds.ship_mode_t1(SM_SHIP_MODE_SK);

   Create an expression index on the **SM_CODE** column in the **tpcds.ship_mode_t1** table.

   ::

      DROP INDEX IF EXISTS ds_ship_mode_t1_index2;
      CREATE INDEX ds_ship_mode_t1_index2 ON tpcds.ship_mode_t1(SUBSTR(SM_CODE,1 ,4));

   Rename the **ds_ship_mode_t1_index1** index to **ds_ship_mode_t1_index5**:

   ::

      ALTER INDEX tpcds.ds_ship_mode_t1_index1 RENAME TO ds_ship_mode_t1_index5;

   Set the **ds_ship_mode_t1_index2** index as unusable:

   ::

      ALTER INDEX tpcds.ds_ship_mode_t1_index2 UNUSABLE;

   Rebuild the **ds_ship_mode_t1_index2** index:

   ::

      ALTER INDEX tpcds.ds_ship_mode_t1_index2 REBUILD;

-  Modifying Partition Index

   Create a sample table named **tpcds.customer_address_p1**.

   ::

      DROP TABLE IF EXISTS tpcds.customer_address_p1;
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

   Create the partitioned table index **ds_customer_address_p1_index2** with the name of the index partition specified.

   ::

      DROP INDEX IF EXISTS ds_customer_address_p1_index2;
      CREATE INDEX ds_customer_address_p1_index2 ON tpcds.customer_address_p1(CA_ADDRESS_SK) LOCAL
      (
          PARTITION CA_ADDRESS_SK_index1,
          PARTITION CA_ADDRESS_SK_index2,
          PARTITION CA_ADDRESS_SK_index3
      )
      ;

   Rename the partition index tpcds. as **ds_customer_address_p1_index2**.

   ::

      ALTER INDEX tpcds.ds_customer_address_p1_index2 RENAME PARTITION CA_ADDRESS_SK_index1 TO CA_ADDRESS_SK_index4;

   Modify the index comment:

   ::

      ALTER INDEX tpcds.ds_customer_address_p1_index2 COMMENT 'comment_ds_customer_address_p1_index2';

Links
-----

:ref:`CREATE INDEX <dws_06_0165>`, :ref:`DROP INDEX <dws_06_0195>`, :ref:`REINDEX <dws_06_0218>`
