:original_name: dws_06_0128.html

.. _dws_06_0128:

ALTER INDEX
===========

Function
--------

**ALTER INDEX** modifies the definition of an existing index.

There are several sub-forms:

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

Parameter Description
---------------------

-  **index_name**

   Specifies the index name to be modified.

-  **new_name**

   Specifies the new name for the index.

   Value range: a string that must comply with the identifier naming rules.

-  **storage_parameter**

   Specifies the name of an index-method-specific parameter.

-  **value**

   Specifies the new value for an index-method-specific storage parameter. This might be a number or a word depending on the parameter.

-  **new_index_partition_name**

   Specifies the new name of the index partition.

-  **index_partition_name**

   Specifies the name of the index partition.

Examples
--------

Rename the **ds_ship_mode_t1_index1** index to **ds_ship_mode_t1_index5**.

::

   ALTER INDEX tpcds.ds_ship_mode_t1_index1 RENAME TO ds_ship_mode_t1_index5;

Set the **ds_ship_mode_t1_index2** index as unusable.

::

   ALTER INDEX tpcds.ds_ship_mode_t1_index2 UNUSABLE;

Rebuild the **ds_ship_mode_t1_index2** index.

::

   ALTER INDEX tpcds.ds_ship_mode_t1_index2 REBUILD;

Rename a partitioned table index.

::

   ALTER INDEX tpcds.ds_customer_address_p1_index2 RENAME PARTITION CA_ADDRESS_SK_index1 TO CA_ADDRESS_SK_index4;

Links
-----

:ref:`CREATE INDEX <dws_06_0165>`, :ref:`DROP INDEX <dws_06_0195>`, :ref:`REINDEX <dws_06_0218>`
