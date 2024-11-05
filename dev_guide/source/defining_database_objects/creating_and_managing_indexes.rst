:original_name: dws_04_0038.html

.. _dws_04_0038:

Creating and Managing Indexes
=============================

Indexes accelerate the data access speed but also add the processing time of the insert, update, and delete operations. Therefore, before creating an index, consider whether it is necessary and determine the columns where indexes will be created. You can determine whether to add an index for a table by analyzing the service processing and data use of applications, as well as columns that are frequently used as search criteria or need to be sorted.

Index type
----------

-  **btree**: The B-tree index uses a structure that is similar to the B+ tree structure to store data key values, facilitating index search. **btree** supports comparison queries with ranges specified.
-  **gin**: GIN indexes are reverse indexes and can process values that contain multiple keys (for example, arrays).
-  **gist**: GiST indexes are suitable for the set data type and multidimensional data types, such as geometric and geographic data types.
-  **Psort**: psort index. It is used to perform partial sort on column-store tables.

Row-based tables support the following index types: **btree** (default), **gin**, and **gist**. Column-based tables support the following index types: **Psort** (default), **btree**, and **gin**.

.. note::

   Create a B-tree index for point queries.

Index Selection Principles
--------------------------

Indexes are created based on columns in database tables. When creating indexes, you need to determine the columns, which can be:

-  Columns that are frequently searched: The search efficiency can be improved.
-  The uniqueness of the columns and the data sequence structures is ensured.
-  Columns that usually function as foreign keys and are used for connections. Then the connection efficiency is improved.
-  Columns that are usually searched for by a specified scope. These indexes have already been arranged in a sequence, and the specified scope is contiguous.
-  Columns that need to be arranged in a sequence. These indexes have already been arranged in a sequence, so the sequence query time is accelerated.
-  Columns that usually use the WHERE clause. Then the condition decision efficiency is increased.
-  Fields that are frequently used after keywords, such as **ORDER BY**, **GROUP BY**, and **DISTINCT**.

   .. note::

      -  After an index is created, the system automatically determines when to reference it. If the system determines that indexing is faster than sequenced scanning, the index will be used.
      -  After an index is successfully created, it must be synchronized with the associated table to ensure new data can be accurately located. Therefore, data operations increase. Therefore, delete unnecessary indexes periodically.

Creating an Index
-----------------

GaussDB(DWS) supports four methods for creating indexes. For details, see :ref:`Table 1 <en-us_topic_0000001460722648__en-us_topic_0000001233563161_tee4c2092e720429fb74de11b2aab4a23>`.

.. note::

   -  After an index is created, the system automatically determines when to reference it. If the system determines that indexing is faster than sequenced scanning, the index will be used.

   -  After an index is successfully created, it must be synchronized with the associated table to ensure new data can be accurately located. Therefore, data operations increase. Therefore, delete unnecessary indexes periodically.

.. _en-us_topic_0000001460722648__en-us_topic_0000001233563161_tee4c2092e720429fb74de11b2aab4a23:

.. table:: **Table 1** **Indexing Method**

   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Indexing Method  | Description                                                                                                                                                                                                                                                                                                                                                     |
   +==================+=================================================================================================================================================================================================================================================================================================================================================================+
   | Unique index     | Refers to an index that constrains the uniqueness of an index attribute or an attribute group. If a table declares unique constraints or primary keys, GaussDB(DWS) automatically creates unique indexes (or composite indexes) for columns that form the primary keys or unique constraints. Currently, only B-tree can create a unique index in GaussDB(DWS). |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Composite index  | Refers to an index that can be defined for multiple attributes of a table. Currently, composite indexes can be created only for B-tree in GaussDB(DWS) and a maximum of 32 columns can share a composite index.                                                                                                                                                 |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Partial index    | Refers to an index that can be created for subsets of a table. This indexing method contains only tuples that meet condition expressions.                                                                                                                                                                                                                       |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Expression index | Refers to an index that is built on a function or an expression calculated based on one or more attributes of a table. An expression index works only when the queried expression is the same as the created expression.                                                                                                                                        |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Run the following command to create an ordinary table:

   ::

      CREATE TABLE tpcds.customer_address_bak AS TABLE tpcds.customer_address;

-  Create a common index.

   You need to query the following information in the **tpcds.customer_address_bak** table:

   ::

      SELECT ca_address_sk FROM tpcds.customer_address_bak WHERE ca_address_sk=14888;

   Generally, the database system needs to scan the **tpcds.customer_address_bak** table row by row to find all matched tuples. If the size of the **tpcds.customer_address_bak** table is large but only a few (possibly zero or one) of the WHERE conditions are met, the performance of this sequential scan is low. If the database system uses an index to maintain the ca_address_sk attribute, the database system only needs to search a few tree layers for the matched tuples. This greatly improves data query performance. Furthermore, indexes can improve the update and delete operation performance in the database.

   Run the following command to create an index:

   ::

      CREATE INDEX index_wr_returned_date_sk ON tpcds.customer_address_bak (ca_address_sk);

-  Create a unique index.

   If a table declares a unique constraint or primary key, GaussDB(DWS) automatically creates a unique index (possibly a multi-column index) on the columns that form the primary key or unique constraint. If no unique constraint or primary key is specified during table creation, you can run the CREATE INDEX statement to create an index.

   ::

      CREATE UNIQUE INDEX unique_index ON tpcds.customer_address_bak(ca_address_sk);

-  Create a multi-column index.

   Assume you need to frequently query records with **ca_address_sk** being **5050** and **ca_street_number** smaller than **1000** in the **tpcds.customer_address_bak** table. Run the following command:

   ::

      SELECT ca_address_sk,ca_address_id FROM tpcds.customer_address_bak WHERE ca_address_sk = 5050 AND ca_street_number < 1000;

   Run the following command to define a multiple-column index on **ca_address_sk** and **ca_street_number** columns:

   ::

      CREATE INDEX more_column_index ON tpcds.customer_address_bak(ca_address_sk ,ca_street_number );

-  Create a partition index.

   If you only want to find records whose **ca_address_sk** is **5050**, you can create a partial index to facilitate your query.

   ::

      CREATE INDEX part_index ON tpcds.customer_address_bak(ca_address_sk) WHERE ca_address_sk = 5050;

-  Create an expression index.

   Assume you need to frequently query records with **ca_street_number** smaller than **1000**, run the following command:

   ::

      SELECT * FROM tpcds.customer_address_bak WHERE trunc(ca_street_number) < 1000;

   The following expression index can be created for this query task:

   ::

      CREATE INDEX para_index ON tpcds.customer_address_bak (trunc(ca_street_number));

Querying an Index
-----------------

-  Run the following command to query all indexes defined by the system and users:

   ::

      SELECT RELNAME FROM PG_CLASS WHERE RELKIND='i';

-  Run the following command to query information about a specified index:

   ::

      \di+ index_wr_returned_date_sk

Recreating an Index
-------------------

-  Recreate the index **index_wr_returned_date_sk**.

   ::

      REINDEX INDEX index_wr_returned_date_sk;

-  Recreate all indexes of a table.

   ::

      REINDEX TABLE tpcds.customer_address_bak;

Deleting an Index
-----------------

You can use the **DROP INDEX** statement to delete indexes.

::

   DROP INDEX index_wr_returned_date_sk;
