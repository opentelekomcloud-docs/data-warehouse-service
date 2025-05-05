:original_name: dws_04_0028.html

.. _dws_04_0028:

Creating and Managing GaussDB(DWS) Tables
=========================================

Creating a Table
----------------

You can run the **CREATE TABLE** command to create a table. When creating a table, you can define the following information:

-  Columns and data type of the table.
-  Table or column constraints that restrict a column or the data contained in a table. For details, see :ref:`Definition of Table Constraints <en-us_topic_0000001811610437__en-us_topic_0000001233681653_section88441546443>`.
-  Distribution policy of a table, which determines how the GaussDB (DWS) database divides data between segments. For details, see :ref:`Definition of Table Distribution <en-us_topic_0000001811610437__en-us_topic_0000001233681653_section14268111013444>`.
-  Table storage format. For details, see :ref:`Selecting a GaussDB(DWS) Table Storage Model <dws_04_0026>`.
-  Partition table information. For details, see :ref:`Creating and Managing GaussDB(DWS) Partitioned Tables <dws_04_0037>`.

Example: Use **CREATE TABLE** to create a table **web_returns_p1**, use **wr_item_sk** as the distribution key, and sets the range distribution function through **wr_returned_date_sk**.

::

   CREATE TABLE web_returns_p1
   (
       wr_returned_date_sk       integer,
       wr_returned_time_sk       integer,
       wr_item_sk                integer NOT NULL,
       wr_refunded_customer_sk   integer
   )
   WITH (orientation = column)
   DISTRIBUTE BY HASH (wr_item_sk)
   PARTITION BY RANGE(wr_returned_date_sk)
   (
       PARTITION p2019 START(20191231) END(20221231) EVERY(10000),
       PARTITION p0 END(maxvalue)
   );

.. _en-us_topic_0000001811610437__en-us_topic_0000001233681653_section88441546443:

Definition of Table Constraints
-------------------------------

You can define constraints on columns and tables to restrict data in a table. However, there are the following restrictions:

-  The primary key constraint and unique constraint in the table must contain a distribution column.
-  Column-store tables support the **PARTIAL CLUSTER KEY** and table-level primary key and unique constraints, but do not support table-level foreign key constraints.
-  Only the **NULL**, **NOT NULL**, and **DEFAULT** constant values can be used as column-store table column constraints.

.. table:: **Table 1** Table constraints

   +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
   | Constraint             | Description                                                                                                                                                                                                                                                                                                                                                                     | Example                                                                                  |
   +========================+=================================================================================================================================================================================================================================================================================================================================================================================+==========================================================================================+
   | Check constraint       | A CHECK constraint allows you to specify that values in a specific column must satisfy a Boolean (true) expression.                                                                                                                                                                                                                                                             | Create the **products** table. The **price** column must be positive.                    |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                          |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 | ::                                                                                       |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                          |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    CREATE TABLE products                                                                 |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    (                                                                                     |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |       product_no integer,                                                                |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |       name text,                                                                         |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |       price numeric CHECK (price > 0)                                                    |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    );                                                                                    |
   +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
   | NOT NULL constraint    | A NOT NULL constraint specifies that a column cannot have null values. A non-null constraint is always written as a column constraint.                                                                                                                                                                                                                                          | Create the **products** table. The values of **product_no** and **name** cannot be null. |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                          |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 | ::                                                                                       |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                          |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    CREATE TABLE products                                                                 |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    (                                                                                     |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        product_no integer NOT NULL,                                                      |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        name text NOT NULL,                                                               |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        price numeric                                                                     |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    );                                                                                    |
   +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
   | UNIQUE constraint      | A UNIQUE constraint specifies that the values in a column or a group of columns are all unique. If **DISTRIBUTE BY REPLICATION** is not specified, the column table that contains only unique values must contain distribution columns.                                                                                                                                         | Create the **products** table. The values of **product_no** must be unique.              |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                          |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 | ::                                                                                       |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                          |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    CREATE TABLE products                                                                 |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    (                                                                                     |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        product_no integer UNIQUE,                                                        |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        name text,                                                                        |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        price numeric                                                                     |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    )DISTRIBUTE BY HASH(product_no);                                                      |
   +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
   | Primary key constraint | A primary key constraint is the combination of a UNIQUE constraint and a NOT NULL constraint. If **DISTRIBUTE BY REPLICATION** is not specified, the column set with a primary key constraint must contain distributed columns. If a table has a primary key, the column (or group of columns) of the primary key is selected as the distribution keys of the table by default. | Create the **products** table. The primary key constraint is **product_no**.             |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                          |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 | ::                                                                                       |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                          |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    CREATE TABLE products                                                                 |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    (                                                                                     |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        product_no integer PRIMARY KEY,                                                   |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        name text,                                                                        |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        price numeric                                                                     |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    )DISTRIBUTE BY HASH(product_no);                                                      |
   +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
   | Partial cluster key    | Partial cluster key can minimize or maximize sparse indexes to quickly filter base tables. Partial cluster key can specify multiple columns, but you are advised to specify no more than two columns.                                                                                                                                                                           | Create the **products** table with PCK set to **product_no**:                            |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                          |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 | ::                                                                                       |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                          |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    CREATE TABLE products                                                                 |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    (                                                                                     |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        product_no integer,                                                               |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        name text,                                                                        |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        price numeric,                                                                    |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |        PARTIAL CLUSTER KEY(product_no)                                                   |
   |                        |                                                                                                                                                                                                                                                                                                                                                                                 |    ) WITH (ORIENTATION = COLUMN);                                                        |
   +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001811610437__en-us_topic_0000001233681653_section14268111013444:

Definition of Table Distribution
--------------------------------

GaussDB(DWS) supports the following distribution modes: replication, hash, and roundrobin.

.. note::

   The roundrobin distribution mode is supported only by cluster version 8.1.2 or later.

+-----------------------+----------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Policy                | Description                                                                                  | Scenario                                                                                                  | Advantages/Disadvantages                                                                                                                                                                                                                                |
+=======================+==============================================================================================+===========================================================================================================+=========================================================================================================================================================================================================================================================+
| Replication           | Full data in a table is stored on each DN in the cluster.                                    | Small tables and dimension tables                                                                         | -  The advantage of replication is that each DN has full data of the table. During the join operation, data does not need to be redistributed, reducing network overheads and reducing plan segments (each plan segment starts a corresponding thread). |
|                       |                                                                                              |                                                                                                           | -  The disadvantage of replication is that each DN retains the complete data of the table, resulting in data redundancy. Generally, replication is only used for small dimension tables.                                                                |
+-----------------------+----------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Hash                  | Table data is distributed on all DNs in the cluster.                                         | Fact tables containing a large amount of data                                                             | -  The I/O resources of each node can be used during data read/write, greatly improving the read/write speed of a table.                                                                                                                                |
|                       |                                                                                              |                                                                                                           | -  Generally, a large table (containing over 1 million records) is defined as a hash table.                                                                                                                                                             |
+-----------------------+----------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Polling (Round-robin) | Each row in the table is sent to each DN in turn. Data can be evenly distributed on each DN. | Fact tables that contain a large amount of data and cannot find a proper distribution column in hash mode | -  Round-robin can avoid data skew, improving the space utilization of the cluster.                                                                                                                                                                     |
|                       |                                                                                              |                                                                                                           | -  Round-robin does not support local DN optimization like a hash table does, and the query performance of Round-robin is usually lower than that of a hash table.                                                                                      |
|                       |                                                                                              |                                                                                                           | -  If a proper distribution column can be found for a large table, use the hash distribution mode with better performance. Otherwise, define the table as a round-robin table.                                                                          |
+-----------------------+----------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Selecting a Distribution Key**

If the hash distribution mode is used, a distribution key must be specified for the user table. When a record is inserted, the system hashes it based on the distribution key and then stores it on the corresponding DN.

Select a hash distribution key based on the following principles:

#. **The values of the distribution key should be discrete so that data can be evenly distributed on each DN.** You can select the primary key of the table as the distribution key. For example, for a person information table, choose the ID number column as the distribution key.

#. **Do not select the column that has a constant filter.** For example, if a constant constraint (for example, zqdh= '000001') exists on the **zqdh** column in some queries on the **dwcjk** table, you are not advised to use **zqdh** as the distribution key.

#. **With the above principles met, you can select join conditions as distribution keys**, so that join tasks can be pushed down to DNs for execution, reducing the amount of data transferred between the DNs.

   For a hash table, an inappropriate distribution key may cause data skew or poor I/O performance on certain DNs. Therefore, you need to check the table to ensure that data is evenly distributed on each DN. You can run the following SQL statements to check for data skew:

   ::

      select
      xc_node_id, count(1)
      from tablename
      group by xc_node_id
      order by xc_node_id desc;

   **xc_node_id** corresponds to a DN. Generally, **over 5% difference between the amount of data on different DNs is regarded as data skew. If the difference is over 10%, choose another distribution key.**

#. You are not advised to add a column as a distribution key, especially add a new column and use the SEQUENCE value to fill the column. (Sequences may cause performance bottlenecks and unnecessary maintenance costs.)

View the data in the table.
---------------------------

-  Run the following command to query information about all tables in a database in the system catalog **pg_tables**:

   ::

      SELECT * FROM pg_tables;

-  Run the **\\d+** command of the **gsql** tool to query table attributes:

   ::

      \d+ customer_t1;

-  Run the following command to query the data volume of table **customer_t1**:

   ::

      SELECT count(*) FROM customer_t1;

-  Run the following command to query all data in table **customer_t1**:

   ::

      SELECT * FROM customer_t1;

-  Run the following command to query data in column **c_customer_sk**:

   ::

      SELECT c_customer_sk FROM customer_t1;

-  Run the following command to filter repeated data in column **c_customer_sk**:

   ::

      SELECT DISTINCT( c_customer_sk ) FROM customer_t1;

-  Run the following command to query all data whose column **c_customer_sk** is **3869**:

   ::

      SELECT * FROM customer_t1 WHERE c_customer_sk = 3869;

-  Run the following command to sort data based on column **c_customer_sk**.

   ::

      SELECT * FROM customer_t1 ORDER BY c_customer_sk;

Deleting Data in a Table
------------------------

.. caution::

   Exercise caution when running the **DROP TABLE** and **TRUNCATE TABLE** statements. After a table is deleted, data cannot be restored.

-  Delete the **customer_t1** table from the database.

   ::

      DROP TABLE customer_t1;

-  You can use **DELETE** or **TRUNCATE** to clear rows in a table without removing the definition of the table.

   Delete all rows from the **customer_t1** table.

   ::

      TRUNCATE TABLE customer_t1;

   Delete all rows from the **customer_t1** table.

   .. code-block:: text

      DELETE FROM customer_t1;

   Delete all records whose **c_customer_sk** is **3869** from the **customer_t1** table.

   .. code-block:: text

      DELETE FROM customer_t1 WHERE c_customer_sk = 3869;

Managing UNLOGGED Tables
------------------------

UNLOGGED indicates an unlogged table. Unlogged tables are faster than regular tables because data written to them is not written to the WALs. However, an unlogged table is automatically cleared after a crash or unclean shutdown, incurring data loss risks. The contents of an unlogged table are also not replicated to standby servers. Any indexes created on an unlogged table are not automatically logged as well.

Usage scenario: Unlogged tables do not ensure safe data. Users can back up data before using unlogged tables; for example, users should back up the data before a system upgrade. When creating an unlogged table, disable cnretry (that is, set the GUC parameter **max_query_retry_times** to **0**).

Troubleshooting: If data is missing in the indexes of unlogged tables due to some unexpected operations such as an unclean shutdown, users should re-create the indexes with errors.

-  Starting from version 9.1.0, UNLOGGED tables are automatically saved in the **pg_unlogged** tablespace and cannot be moved or assigned to other tablespaces.

-  After an earlier version is upgraded to 9.1.0, the UNLOGGED table created in the earlier version is still stored in the original tablespace.

Version 9.1.0 has a script called **switch_unlogged_tablespace.py** that can move unlogged tables to optimize the recovery time objective (RTO). This script works together with the GUC parameter **enable_unlogged_tablespace_compat**.

#. The script is stored in the **$GPHOME/script** directory. You can use the **-?** command to obtain help information.

   |image1|

#. Migrate all unlogged tables (recommended).

   ::

      python3 switch_unlogged_tablespace.py -t switch

3. After the migration, the GUC parameter **enable_unlogged_tablespace_compat** is automatically set to **off**.

.. important::

   After the upgrade to 9.1.0, you are advised to perform the following two steps to improve the instance restart RTO:

   #. Use the **switch_unlogged_tablespace.py** script to migrate all unlogged tables to the **pg_unlogged** tablespace.
   #. If the old version does not use any unlogged table, you are advised to set the GUC parameter **enable_unlogged_tablespace_compat** to **OFF**.

.. |image1| image:: /_static/images/en-us_image_0000001972893569.png
