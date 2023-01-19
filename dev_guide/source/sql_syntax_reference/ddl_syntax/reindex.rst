:original_name: dws_06_0218.html

.. _dws_06_0218:

REINDEX
=======

Function
--------

**REINDEX** rebuilds an index using the data stored in the index's table, replacing the old copy of the index.

There are several scenarios in which **REINDEX** can be used:

-  An index has become corrupted, and no longer contains valid data.

-  An index has become "bloated", that is, it contains many empty or nearly-empty pages.

-  You have altered a storage parameter (such as fillfactor) for an index, and wish to ensure that the change has taken full effect.

   An index build with the **CONCURRENTLY** option failed, leaving an "invalid" index.

Precautions
-----------

Index reconstruction of the **REINDEX DATABASE** or **SYSTEM** type cannot be performed in transaction blocks.

Syntax
------

-  Rebuild a general index.

   ::

      REINDEX { INDEX |  TABLE | DATABASE | SYSTEM } name [ FORCE ];

-  Rebuild an index partition.

   ::

      REINDEX  { TABLE } name
          PARTITION partition_name [ FORCE  ];

Parameter Description
---------------------

-  **INDEX**

   Recreates the specified index.

-  **TABLE**

   Recreates all indexes of the specified table. If the table has a secondary TOAST table, that is reindexed as well.

-  **DATABASE**

   Recreates all indexes within the current database. Indexes on the shared system directory will also be processed. This form of **REINDEX** cannot be executed within a transaction block.

-  **SYSTEM**

   Recreates all indexes on system catalogs within the current database. Indexes on user tables are not processed.

-  **name**

   Name of the specific index, table, or database to be reindexed. Index and table names can be schema-qualified.

   .. note::

      **REINDEX DATABASE** and **SYSTEM** can create indexes for only the current database. Therefore, **name** must be the same as the current database name.

-  **FORCE**

   This is an obsolete option. It is ignored if specified.

-  **partition_name**

   Specifies the name of the partition or index partition to be reindexed.

   Value range:

   -  If it is **REINDEX INDEX**, specify the name of an index partition.
   -  If it is **REINDEX TABLE**, specify the name of a partition.

.. important::

   Index reconstruction of the **REINDEX DATABASE** or **SYSTEM** type cannot be performed in transaction blocks.

Examples
--------

Rebuild a single index.

::

   REINDEX INDEX tpcds.tpcds_customer_index1;

Rebuild all indexes on the **tpcds.customer_t1** table.

::

   REINDEX TABLE tpcds.customer_t1;
