:original_name: dws_06_0128.html

.. _dws_06_0128:

ALTER INDEX
===========

Function
--------

**ALTER INDEX** modifies the definition of an index.

Precautions
-----------

Only the owner of an index or a system administrator can run this statement.

Syntax
------

-  Rename a table index. The new index name can be prefixed with the name of the schema where the original index is located, but the schema name cannot be changed at the same time.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          RENAME TO new_name;
      ALTER INDEX [ IF EXISTS ] index_name
          RENAME TO schema.new_name;

-  Modify the storage parameter of a table index.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          SET ( {storage_parameter = value} [, ... ] );

-  Modify the status flag of an index.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          SET ( {invisible = value} [, ... ] );

-  Reset the storage parameter of a table index.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          RESET ( storage_parameter [, ... ] ) ;

-  Set a table index or an index partition to be unavailable.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          [ MODIFY PARTITION index_partition_name ] UNUSABLE;

-  Rebuild a table index or index partition.

   ::

      ALTER INDEX index_name
          REBUILD [ PARTITION index_partition_name ] [WITHOUT UNUSABLE];

-  Rename an index partition. When **PG_OBJECT** record the last modification time of an index, this syntax cannot be recorded.

   ::

      ALTER INDEX [ IF EXISTS ] index_name
          RENAME PARTITION index_partition_name TO new_index_partition_name;

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

.. table:: **Table 1** ALTER INDEX parameters

   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | Parameter                                          | Description                                                                                                                                                                                                                         | Value Range                                                                                                        |
   +====================================================+=====================================================================================================================================================================================================================================+====================================================================================================================+
   | IF EXISTS                                          | If the specified index does not exist, a prompt instead of an error is returned.                                                                                                                                                    | ``-``                                                                                                              |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | RENAME TO                                          | Renames an index. It has no impact on the stored data.                                                                                                                                                                              | ``-``                                                                                                              |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | SET ( { STORAGE_PARAMETER = value } [, ...] )      | Changes one or more index-method-specific storage parameters. This command does not change the index content right away. Depending on the settings, you might need to rebuild the index with **REINDEX** to get the desired result. | For details about the value range and usage, see :ref:`Table 2 <en-us_topic_0000001811634713__table157114373494>`. |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | RESET ( { storage_parameter } [, ...] )            | Resets one or more index method-specific storage parameters of an index to default values. Similar to the **SET** statement, **REINDEX** may be used to completely update the index.                                                | ``-``                                                                                                              |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | [ MODIFY PARTITION index_partition_name ] UNUSABLE | Sets the index on a table or index partition to be unavailable.                                                                                                                                                                     | ``-``                                                                                                              |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | REBUILD [ PARTITION index_partition_name ]         | Recreates the index on a table or index partition.                                                                                                                                                                                  | ``-``                                                                                                              |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | WITHOUT UNUSABLE                                   | Indexes that are in an UNUSABLE state are not rebuilt when rebuilding indexes in tables or index partitions.                                                                                                                        | ``-``                                                                                                              |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | RENAME PARTITION                                   | Renames an index partition.                                                                                                                                                                                                         | ``-``                                                                                                              |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | COMMENT comment_text                               | Adds, modifies, or deletes index comments.                                                                                                                                                                                          | ``-``                                                                                                              |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | index_name                                         | Specifies the index name to be modified.                                                                                                                                                                                            | Name of an existing index.                                                                                         |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | new_name                                           | Specifies the new name for the index.                                                                                                                                                                                               | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.   |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | storage_parameter                                  | Specifies the name of an index-method-specific parameter.                                                                                                                                                                           | ``-``                                                                                                              |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | invisible                                          | Controls whether the optimizer generates index scan plans. The default value is **OFF**.                                                                                                                                            | **ON** or **OFF**.                                                                                                 |
   |                                                    |                                                                                                                                                                                                                                     |                                                                                                                    |
   |                                                    |                                                                                                                                                                                                                                     | -  **ON** indicates that no index scan plan is generated.                                                          |
   |                                                    |                                                                                                                                                                                                                                     | -  **OFF** indicates that an index scan plan is generated.                                                         |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | value                                              | Specifies the new value for an index-method-specific storage parameter. This might be a number or a word depending on the parameter.                                                                                                | ``-``                                                                                                              |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | new_index_partition_name                           | Specifies the new name of the index partition.                                                                                                                                                                                      | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.   |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | index_partition_name                               | Specifies the name of the index partition.                                                                                                                                                                                          | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.   |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
   | comment_text                                       | Specifies the comment of an index.                                                                                                                                                                                                  | ``-``                                                                                                              |
   +----------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001811634713__table157114373494:

.. table:: **Table 2** WITH ( {storage_parameter = value} [, ... ] ) parameter description

   +----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------+
   | Storage Parameter          | Description                                                                                                                                                                                                                                                                                                                                                                                                                             | Example                                                                                            |
   +============================+=========================================================================================================================================================================================================================================================================================================================================================================================================================================+====================================================================================================+
   | **FILLFACTOR**             | **This parameter is unavailable for GIN and PSORT indexes**. It indicates the fill factor of an index and its value is a percentage between 10 to 100. Index pages are packed only to the indicated percentage and the remaining space is used for subsequent INSERT and UPDATE operations, thereby reducing page splits and improving performance.                                                                                     | Change the fillfactor of an index to 60%.                                                          |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | This parameter determines the percentage of space on index pages to be packed, which affects the database performance and storage space usage. You can adjust its value to reduce page splits caused by subsequent INSERT and UPDATE operations, optimizing the storage and performance of indexes.                                                                                                                                     | ::                                                                                                 |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | **Impact of FILLFACTOR on index performance:**                                                                                                                                                                                                                                                                                                                                                                                          |    ALTER INDEX tpcds.ds_ship_mode_t1_index7 SET (FILLFACTOR = 60);                                 |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | -  **Performance**: When an index page is fully packed and a new record needs to be inserted, the page will be split, consuming more I/O resources and time. A lower fillfactor can reduce page splits and improve the INSERT and UPDATE performance, but will increase the storage space usage. A higher fillfactor can improve the storage space usage, but may cause more page splits and degrade the INSERT and UPDATE performance. |                                                                                                    |
   |                            | -  **Storage space**: A lower fillfactor increases the free space of each data page. A higher fillfactor stores data more compactly, reducing disk space usage.                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | **How to configure FILLFACTOR:**                                                                                                                                                                                                                                                                                                                                                                                                        |                                                                                                    |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | -  For tables that are frequently updated, set **FILLFACTOR** to a small value, for example, **60** to **80**, to reserve space and reduce page splits.                                                                                                                                                                                                                                                                                 |                                                                                                    |
   |                            | -  For tables that are rarely updated, set **FILLFACTOR** to a large value, for example, **90** to **100**, to maximize storage space.                                                                                                                                                                                                                                                                                                  |                                                                                                    |
   +----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------+
   | **INVISIBLE**              | Controls whether the optimizer generates index scan plans.                                                                                                                                                                                                                                                                                                                                                                              | Not allow the optimizer to generate any index scan plan.                                           |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | **Scenario**: If you want to disable indexes temporarily during maintenance or test the impact of indexes on performance, you can use this parameter.                                                                                                                                                                                                                                                                                   | ::                                                                                                 |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | **Value range**:                                                                                                                                                                                                                                                                                                                                                                                                                        |    ALTER INDEX tpcds.ds_ship_mode_t1_index9 SET (INVISIBLE = ON);                                  |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | -  **ON** indicates that no index scan plan is generated.                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                    |
   |                            | -  **OFF** indicates that an index scan plan is generated.                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                    |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | **Default value**: **OFF**                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                    |
   +----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------+
   | **FASTUPDATE**             | **This parameter is available only for GIN indexes.** It controls whether to enable fast update for GIN indexes. Once enabled, data changes are temporarily stored in the pending list.                                                                                                                                                                                                                                                 | Enable fast update and set the maximum size of the pending list of the index to 8 MB.              |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | **Value range**: **ON** or **OFF**                                                                                                                                                                                                                                                                                                                                                                                                      | ::                                                                                                 |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | **Default value**: **ON**                                                                                                                                                                                                                                                                                                                                                                                                               |    ALTER INDEX tpcds.ds_ship_mode_t1_index10 SET (FASTUPDATE = ON, GIN_PENDING_LIST_LIMIT = 8192); |
   +----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------+
   | **GIN_PENDING_LIST_LIMIT** | **This parameter is supported only by GIN indexes**. When fast update is enabled for GIN indexes, you can use this parameter to set the maximum size of the pending list of GIN indexes.                                                                                                                                                                                                                                                |                                                                                                    |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | The pending list is a data structure specific to GIN indexes and is used to temporarily store index update operations. When **FASTUPDATE** is enabled for a GIN index, new index items are not directly written to the main index structure, but are stored in the pending list. When conditions are met, the index items are merged into the main index in batches.                                                                    |                                                                                                    |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | **Value range**: **64** to *INT_MAX*. The unit is KB.                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                    |
   |                            |                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                    |
   |                            | **Default value**: determined by the value of **gin_pending_list_limit** (4 MB by default).                                                                                                                                                                                                                                                                                                                                             |                                                                                                    |
   +----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------+

Examples
--------

Create a sample table named **tpcds.ship_mode_t1**.

::

   DROP SCHEMA IF EXISTS tpcds CASCADE;
   CREATE SCHEMA tpcds;
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

Rename the **ds_ship_mode_t1_index1** index to **ds_ship_mode_t1_index5**.

::

   ALTER INDEX tpcds.ds_ship_mode_t1_index1 RENAME TO ds_ship_mode_t1_index5;

Set the **ds_ship_mode_t1_index2** index as unusable.

::

   ALTER INDEX tpcds.ds_ship_mode_t1_index2 UNUSABLE;

Rebuild the **ds_ship_mode_t1_index2** index.

::

   ALTER INDEX tpcds.ds_ship_mode_t1_index2 REBUILD;

Links
-----

:ref:`CREATE INDEX <dws_06_0165>`, :ref:`DROP INDEX <dws_06_0195>`, and :ref:`REINDEX <dws_06_0218>`
