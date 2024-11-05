:original_name: dws_06_0128.html

.. _dws_06_0128:

ALTER INDEX
===========

Function
--------

Modifies the definition of an existing index.

Precautions
-----------

-  Only the owner of an index or a system administrator can run this statement.

Syntax
------

-  Renames a table index. The new index name can be prefixed with the name of the schema where the original index is located, but the schema name cannot be changed at the same time.

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

-  IF EXISTS

   If the specified index does not exist, a notice instead of an error is sent.

-  RENAME TO

   Changes only the name of the index. There is no effect on the stored data.

-  SET ( { STORAGE_PARAMETER = value } [, ...] )

   Change one or more index-method-specific storage parameters. Note that the index contents will not be modified immediately by this command. You might need to rebuild the index with **REINDEX** to get the desired effects depending on parameters.

-  RESET ( { storage_parameter } [, ...] )

   Reset one or more index-method-specific storage parameters to the default value. Similar to the **SET** statement, **REINDEX** may be used to completely update the index.

-  [ MODIFY PARTITION index_partition_name ] UNUSABLE

   Sets the index on a table or index partition to be unavailable.

-  REBUILD [ PARTITION index_partition_name ]

   Recreates the index on a table or index partition.

-  RENAME PARTITION

   Renames an index partition.

-  COMMENT comment_text

   Adds, modifies, or deletes index comments.

-  **index_name**

   Specifies the index name to be modified.

-  **new_name**

   Specifies the new name for the index.

   Value range: a string that must comply with the identifier naming rules.

-  **storage_parameter**

   Specifies the name of an index-method-specific parameter.

-  **invisible**

   Controls whether the optimizer generates index scan plans.

   Value range:

   -  **ON** indicates that no index scan plan is generated.
   -  **OFF** indicates that an index scan plan is generated.

   Default value: **OFF**

-  **value**

   Specifies the new value for an index-method-specific storage parameter. This might be a number or a word depending on the parameter.

-  **new_index_partition_name**

   Specifies the new name of the index partition.

-  **index_partition_name**

   Specifies the name of the index partition.

-  **comment_text**

   Comment of an index.

Examples
--------

Rename the existing index **ds_ship_mode_t1_index1** to **tpcds. ds_ship_mode_t1_index5**. The original schema name **tpcds** is prefixed to the new name.

::

   ALTER INDEX tpcds.ds_ship_mode_t1_index1 RENAME TO tpcds.ds_ship_mode_t1_index5;

Set the **ds_ship_mode_t1_index2** index as unusable:

::

   ALTER INDEX tpcds.ds_ship_mode_t1_index2 UNUSABLE;

Rebuild the **ds_ship_mode_t1_index2** index:

::

   ALTER INDEX tpcds.ds_ship_mode_t1_index2 REBUILD;

Rename a partitioned table index:

::

   ALTER INDEX tpcds.ds_customer_address_p1_index2 RENAME PARTITION CA_ADDRESS_SK_index1 TO CA_ADDRESS_SK_index4;

Modify the index comment:

::

   ALTER INDEX tpcds.ds_customer_address_p1_index2 COMMENT 'comment_ds_customer_address_p1_index2';

Links
-----

:ref:`CREATE INDEX <dws_06_0165>`, :ref:`DROP INDEX <dws_06_0195>`, :ref:`REINDEX <dws_06_0218>`
