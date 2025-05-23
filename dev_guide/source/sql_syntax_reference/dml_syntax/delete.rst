:original_name: dws_06_0231.html

.. _dws_06_0231:

DELETE
======

Function
--------

Deletes rows that satisfy the **WHERE** clause from the specified table. If the **WHERE** clause does not exist, all rows in the table will be deleted. The result is a valid, but an empty table.

Precautions
-----------

-  You must have the DELETE permission on the table to delete from it, as well as the SELECT permission for any table in the **USING** clause or whose values are read in the **condition**.
-  For replication tables, **DELETE** can be performed only in the following two scenarios:

   -  Scenarios with primary key constraints.
   -  Scenario where the execution plan can be pushed down.

-  For column-store tables, the **RETURNING** clause is currently not supported.

.. warning::

   -  Avoid using **UPDATE** or **DELETE** to modify or delete a large volume of data and use **TRUNCATE PARTITION** or **DROP PARTITION** instead.
   -  For more information about development and design specifications, see "GaussDB(DWS) Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

Syntax
------

::

   [ WITH [ RECURSIVE ] with_query [, ...] ]
       DELETE [/*+ plan_hint */] FROM [ ONLY ] table_name [ * ] [ [ AS ] alias ]
       [ PARTITION ( partition_name ) | PARTITION FOR ( partition_key_value [, ...] ) ]
       [ USING using_list ]
       [ WHERE condition | WHERE CURRENT OF cursor_name ]
       [ RETURNING { * | { output_expr [ [ AS ] output_name ] } [, ...] } ];

Parameter Description
---------------------

-  **WITH [ RECURSIVE ] with_query [, ...]**

   The **WITH** clause allows you to specify one or more subqueries that can be referenced by name in the primary query, equal to temporary table.

   If **RECURSIVE** is specified, it allows a **SELECT** subquery to reference itself by name.

   The with_query detailed format is as follows:

   .. code-block::

      with_query_name [ ( column_name [, ...] ) ] AS
      ( {select | values | insert | update | delete} )

   -- **with_query_name** specifies the name of the result set generated by a subquery. Such names can be used to access the result sets of

   **column_name** specifies the column name displayed in the subquery result set.

   Each subquery can be a **SELECT**, **VALUES**, **INSERT**, **UPDATE** or **DELETE** statement.

-  **plan_hint** clause

   Following the keyword in the **/*+ \*/** format, hints are used to optimize the plan generated by a specified statement block. For details, see "GaussDB(DWS) Performance Tuning" > "SQL Tuning" > "Hint-based Tuning" in *Data Warehouse Service (DWS) Developer Guide*.

-  **ONLY**

   If **ONLY** is specified, only that table is deleted. If **ONLY** is not specified, this table and all its sub-tables are deleted.

-  **table_name**

   Specifies the name (optionally schema-qualified) of a target table.

   Value range: an existing table name

-  **alias**

   Specifies the alias for the target table.

   Value range: a string. It must comply with the naming convention.

-  **partition_name**

   Specifies the name of a partition. Only clusters of version 8.2.1 or later support this option.

   Value range: An existing partition name.

-  **partition_key_value**

   Specifies the key value of a partition.

   The value specified by **PARTITION FOR ( partition_key_value [, ...] )** can uniquely identify a partition.

   Value range: value range of the partition key for the partition to be renamed

-  **using_list**

   Specifies the **USING** clause.

-  **condition**

   Specifies an expression that returns a value of type boolean. Only rows for which this expression returns **true** will be deleted.

-  **WHERE CURRENT OF cursor_name**

   Not supported currently. Only syntax interface is provided.

-  **output_expr**

   Specifies an expression to be computed and returned by the **DELETE** command after each row is deleted. The expression can use any column names of the table. Write \* to return all columns.

-  **output_name**

   Specifies a name to use for a returned column.

   Value range: a string. It must comply with the naming convention.

Examples
--------

Create the **tpcds.customer_address_bak** table:

::

   CREATE TABLE tpcds.customer_address_bak AS TABLE tpcds.customer_address;

Delete employees whose **ca_address_sk** is less than **14888** in the **tpcds.customer_address_bak** table:

.. code-block:: text

   DELETE FROM tpcds.customer_address_bak WHERE ca_address_sk < 14888;

Delete the employees whose **ca_address_sk** is **14891**, **14893**, and **14895** from **tpcds. customer_address_bak**:

.. code-block:: text

   DELETE FROM tpcds.customer_address_bak WHERE ca_address_sk in (14891,14893,14895);

Delete all data in the **tpcds.customer_address_bak** table:

.. code-block:: text

   DELETE FROM tpcds.customer_address_bak;

Use a subquery (to delete the row-store table **tpcds.warehouse_t30**) to obtain a temporary table **temp_t**, and then query all data in the temporary table **temp_t**:

::

   WITH temp_t AS (DELETE FROM tpcds.warehouse_t30 RETURNING *) SELECT * FROM temp_t ORDER BY 1;

Delete partition **p1** from the partitioned table **test_range_row**:

::

   CREATE TABLE test_range_row(a int, d int)
   DISTRIBUTE BY hash(a) PARTITION BY RANGE(d)
   (
       PARTITION p1 values LESS THAN (60),
       PARTITION p2 values LESS THAN (75),
       PARTITION p3 values LESS THAN (90),
       PARTITION p4 VALUES LESS THAN (maxvalue)
   );
   INSERT OVERWRITE INTO test_range_row PARTITION(p1) VALUES(55,51);
   INSERT OVERWRITE INTO test_range_row PARTITION(p3) VALUES(85,80);

   DELETE FROM test_range_row PARTITION(p1);
