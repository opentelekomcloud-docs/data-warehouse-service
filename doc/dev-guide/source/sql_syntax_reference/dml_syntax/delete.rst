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
   -  For more information about development and design specifications, see "DWS Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

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

.. table:: **Table 1** DELETE parameters

   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                             | Description                                                                                                                                                                                                                                                      | Value Range                                                                                                                                                         |
   +=======================================+==================================================================================================================================================================================================================================================================+=====================================================================================================================================================================+
   | WITH [ RECURSIVE ] with_query [, ...] | The **WITH** clause allows you to specify one or more subqueries that can be referenced by name in the primary query, equal to temporary table.                                                                                                                  | ``-``                                                                                                                                                               |
   |                                       |                                                                                                                                                                                                                                                                  |                                                                                                                                                                     |
   |                                       | If **RECURSIVE** is specified, it allows a **SELECT** subquery to reference itself by name.                                                                                                                                                                      |                                                                                                                                                                     |
   |                                       |                                                                                                                                                                                                                                                                  |                                                                                                                                                                     |
   |                                       | The format of **with_query** is as follows:                                                                                                                                                                                                                      |                                                                                                                                                                     |
   |                                       |                                                                                                                                                                                                                                                                  |                                                                                                                                                                     |
   |                                       | .. code-block::                                                                                                                                                                                                                                                  |                                                                                                                                                                     |
   |                                       |                                                                                                                                                                                                                                                                  |                                                                                                                                                                     |
   |                                       |    with_query_name [ ( column_name [, ...] ) ] AS                                                                                                                                                                                                                |                                                                                                                                                                     |
   |                                       |    ( {select | values | insert | update | delete} )                                                                                                                                                                                                              |                                                                                                                                                                     |
   |                                       |                                                                                                                                                                                                                                                                  |                                                                                                                                                                     |
   |                                       | -- **with_query_name** specifies the name of the result set generated by a subquery. Such names can be used to access the result sets of                                                                                                                         |                                                                                                                                                                     |
   |                                       |                                                                                                                                                                                                                                                                  |                                                                                                                                                                     |
   |                                       | **column_name** specifies the column name displayed in the subquery result set.                                                                                                                                                                                  |                                                                                                                                                                     |
   |                                       |                                                                                                                                                                                                                                                                  |                                                                                                                                                                     |
   |                                       | Each subquery can be a **SELECT**, **VALUES**, **INSERT**, **UPDATE** or **DELETE** statement.                                                                                                                                                                   |                                                                                                                                                                     |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | **plan_hint** clause                  | Following the keyword in the **/*+ \*/** format, hints are used to optimize the plan generated by a specified statement block. For details, see "DWS Performance Tuning" > "SQL Tuning" > "Hint-based Tuning" in *Data Warehouse Service (DWS) Developer Guide*. | ``-``                                                                                                                                                               |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ONLY                                  | If **ONLY** is specified, only the specified table is deleted. If **ONLY** is not specified, the specified table and all its inherited tables are deleted.                                                                                                       | ``-``                                                                                                                                                               |
   |                                       |                                                                                                                                                                                                                                                                  |                                                                                                                                                                     |
   |                                       | This parameter is reserved only for compatibility with PostgreSQL. DWS does not support inherited tables.                                                                                                                                                        |                                                                                                                                                                     |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name                            | Specifies the name (optionally schema-qualified) of a target table.                                                                                                                                                                                              | An existing table name.                                                                                                                                             |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | alias                                 | Specifies the alias for the target table.                                                                                                                                                                                                                        | A string, which must comply with the naming convention. For details, see :ref:`Identifier Naming Conventions <en-us_topic_0000001811634529__section1475018612353>`. |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_name                        | Specifies the name of a partition. Only clusters of version 8.2.1 or later support this option.                                                                                                                                                                  | An existing partition name.                                                                                                                                         |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_key_value                   | Specifies the key value of a partition.                                                                                                                                                                                                                          | Value range of the partition key for the partition to be renamed.                                                                                                   |
   |                                       |                                                                                                                                                                                                                                                                  |                                                                                                                                                                     |
   |                                       | The value specified by **PARTITION FOR ( partition_key_value [, ...] )** can uniquely identify a partition.                                                                                                                                                      |                                                                                                                                                                     |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | using_list                            | Specifies the **USING** clause.                                                                                                                                                                                                                                  | ``-``                                                                                                                                                               |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | condition                             | Specifies an expression that returns a value of type boolean. Only rows for which this expression returns **true** will be deleted.                                                                                                                              | ``-``                                                                                                                                                               |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | WHERE CURRENT OF cursor_name          | Not supported currently. Only syntax interface is provided.                                                                                                                                                                                                      | ``-``                                                                                                                                                               |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | output_expr                           | Specifies an expression to be computed and returned by the **DELETE** command after each row is deleted. The expression can use any column names of the table. Write \* to return all columns.                                                                   | ``-``                                                                                                                                                               |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | output_name                           | Specifies a name to use for a returned column.                                                                                                                                                                                                                   | A string, which must comply with the naming convention. For details, see :ref:`Identifier Naming Conventions <en-us_topic_0000001811634529__section1475018612353>`. |
   +---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

Create the table **tpcds.customer_address**.

::

   DROP SCHEMA IF EXISTS tpcds CASCADE;
   CREATE SCHEMA tpcds;
   CREATE TABLE tpcds.customer_address
   (
       ca_address_sk             bigint               not null ,
       ca_address_id             char(16)              not null,
       ca_street_number          char(10)                      ,
       ca_street_name            varchar(60)                   ,
       ca_street_type            char(15)                      ,
       ca_suite_number           char(10)                      ,
       ca_city                   varchar(60)                   ,
       ca_county                 varchar(30)                   ,
       ca_state                  char(2)                       ,
       ca_zip                    char(10)                      ,
       ca_country                varchar(20)                   ,
       ca_gmt_offset             decimal(5,2)                  ,
       ca_location_type          char(20)
   )
    with (orientation = column)
   distribute by hash (ca_address_sk);

Delete employees with **ca_address_sk** less than **14888** from **tpcds.customer_address**.

.. code-block:: text

   DELETE FROM tpcds.customer_address WHERE ca_address_sk < 14888;

Delete employees with **ca_address_sk** equal to **14891**, **14893**, and **14895** from **tpcds.customer_address**.

.. code-block:: text

   DELETE FROM tpcds.customer_address WHERE ca_address_sk in (14891,14893,14895);

Delete all data from **tpcds.customer_address**.

.. code-block:: text

   DELETE FROM tpcds.customer_address;

Use a subquery (to delete the row-store table **tpcds.warehouse_t30**) to obtain a temporary table **temp_t**, and then query all data in the temporary table **temp_t**.

::

   CREATE TABLE tpcds.warehouse_t30 (a int, b int);
   WITH temp_t AS (DELETE FROM tpcds.warehouse_t30 RETURNING *) SELECT * FROM temp_t ORDER BY 1;

Delete the specified partition **p1** from the partitioned table **test_range_row**.

::

   DROP TABLE IF EXISTS test_range_row;
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
