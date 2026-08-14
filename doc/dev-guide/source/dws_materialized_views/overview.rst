:original_name: dws_04_1414.html

.. _dws_04_1414:

Overview
========

A materialized view is a special database object. Materialized views calculate complex query results in advance and store the results to storage media. During service query, the pre-calculated data is directly queried to accelerate the query.

Materialized views of DWS is supported only by clusters of version 8.2.1.220 or later. To use materialized views, ensure that :ref:`enable_matview <en-us_topic_0000001811490801__section17297153214352>` is set to **on**.

Basic Concepts
--------------

.. table:: **Table 1** Basic concepts of materialized views

   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Concept                           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
   +===================================+=================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | Base table                        | -  Tables on which the materialized view definition depends. The data of the materialized view comes from the calculation results of these tables.                                                                                                                                                                                                                                                                                                                                                                                                              |
   |                                   | -  Base tables can be DWS internal tables, foreign tables, partitioned tables, views, or materialized views.                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                                   | -  Temporary tables are not supported.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Refresh                           | After a materialized view is created, the data stored in the materialized view reflects only the status at the creation time. When the data in the base table changes, you need to refresh the materialized view to update the data.                                                                                                                                                                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
   |                                   | Refresh modes include full refresh and partition refresh.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
   |                                   | -  Full refresh: The data in the materialized view is recalculated each time the data is refreshed.                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   |                                   | -  Partition refresh: The data of a partition in the materialized view is refreshed each time.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Query rewriting                   | If you use materialized views to accelerate queries, when you enter a query that does not use materialized views, the system automatically rewrites the query to a query that uses materialized views. This method works by comparing the query with the materialized view definition. If a matching materialized view is identified, the query is automatically modified to use that materialized view. This allows for leveraging pre-calculated results from the materialized view, leading to significant enhancements in query performance and efficiency. |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Asynchronous and Synchronous Materialized Views
-----------------------------------------------

When the data in the base table changes after the materialized view is created, you need to refresh the materialized view to update the data. Asynchronous materialized views and synchronous materialized views use different refresh modes.

**Currently, DWS supports only asynchronous materialized views.**

-  Asynchronous materialized view: After the data in the base table changes, the materialized view needs to be refreshed periodically to update the data in it.

   -  Manual refresh: After service data is modified, run the **REFRESH MATERIALIZED VIEW** command to refresh the corresponding materialized view.
   -  Automatic refresh: Specify the refresh start time and refresh interval when the materialized view is created. The database periodically updates the data.

-  Synchronous materialized view: The materialized view data automatically changes after the base table data changes.

Advantages of Materialized Views
--------------------------------

-  **Higher query performance**

   Materialized views pre-calculate and store query results. When a query is initiated, materialized views can be queried and directly return results, improving query performance.

-  **Lower compute resource consumption**

   Executing complex queries multiple times requires significant CPU and memory resources. Materialized views help reduce the load of database resources by avoiding these repeated calculations.

-  **Data warehouse modeling**

   Materialized views can be used as the data access layer to support complex service logic and allow users to access preprocessed data without considering the complex SQL logic at the bottom layer.

Materialized View Syntax
------------------------

Create a materialized view.

::

   CREATE MATERIALIZED VIEW [ IF NOT EXISTS ] materialized_view_name
   [ ( column_name [, ...] ) ]
   [ BUILD { DEFERRED | IMMEDIATE } ]
   [ REFRESH [ [DEFAULT] | [ RESTRICT ] | [ CASCADE FORWARD ] | [ CASCADE BACKWARD ] | [ CASCADE ALL ] ] [ COMPLETE ] [ ON DEMAND ] [ [ START WITH (timestamptz) ] | [ EVERY (interval) ] ] ]
   [ { ENABLE | DISABLE } QUERY REWRITE ]
   [ WITH ( {storage_parameter = value} [, ... ] ) ]
   [ TABLESPACE tablespace_name ]
   [ DISTRIBUTE BY { REPLICATION | ROUNDROBIN | { HASH ( column_name [,...] ) } } ]
   [ PARTITION BY {(column_name) | date_trunc('time_unit', column_name)} ]
   AS query;

Refresh a materialized view.

::

   REFRESH MATERIALIZED VIEW
   {[schema.]materialized_view_name} [ [DEFAULT] | [ RESTRICT ] | [ CASCADE FORWARD ] | [ CASCADE BACKWARD ] | [ CASCADE ALL ] ]

Delete a materialized view.

::

   DROP MATERIALIZED VIEW [ IF EXISTS ]
   {[schema.]materialized_view_name} [, ...] [ CASCADE | RESTRICT ];

Modify a materialized view.

::

   ALTER MATERIALIZED VIEW [ IF EXISTS ] { materialized_view_name }
       [ ENABLE | DISABLE ] QUERY REWRITE;
   ALTER MATERIALIZED VIEW [ IF EXISTS ] { materialized_view_name }
       REFRESH [ [DEFAULT] | [ RESTRICT ] | [ CASCADE FORWARD ] | [ CASCADE BACKWARD ] | [ CASCADE ALL ] ] [ COMPLETE ] [ ON DEMAND ] [ [ START WITH (timestamptz) ] | [ EVERY (interval) ] ];
   ALTER MATERIALIZED VIEW [ IF EXISTS ] { materialized_view_name }
       REFRESH [DEFAULT] | [ RESTRICT ] | [ CASCADE FORWARD ] | [ CASCADE BACKWARD ] | [ CASCADE ALL ] ;
   ALTER MATERIALIZED VIEW { materialized_view_name }
       OWNER TO new_owner;
   ALTER MATERIALIZED VIEW { materialized_view_name }
       SET ( {storage_parameter = value} [, ... ] )
       | RESET ( storage_parameter [, ... ] )

For details about the parameters of the materialized view syntax, see "CREATE MATERIALIZED VIEW" and "ALTER MATERIALIZED VIEW" in *SQL Syntax Reference*.

Application Scenarios
---------------------

Materialized views apply to the following scenarios:

-  The query result does not change frequently, that is, the base table is stable and the execution frequency is high.
-  The query involves time-consuming operations, such as aggregation and join. For queries involving multiple table join and aggregation operations, materialized views can pre-calculate and store these time-consuming operations to avoid re-executing these operations during each query.
-  The query result involves only a small part of data in the table.

   .. table:: **Table 2** Comparison between materialized views, views, and tables

      +--------------------+-------------+--------------------+--------------------------+
      | Database Object    | Performance | Persistent Storage | Simplified Service Logic |
      +====================+=============+====================+==========================+
      | Materialized views | Y           | Y                  | Y                        |
      +--------------------+-------------+--------------------+--------------------------+
      | Views              | ``-``       | ``-``              | Y                        |
      +--------------------+-------------+--------------------+--------------------------+
      | Tables             | ``-``       | Y                  | ``-``                    |
      +--------------------+-------------+--------------------+--------------------------+

Permissions
-----------

-  To create a materialized view, you must have the **CREATE** permission on the schema and the **SELECT** permission on the base table or column.
-  To query a materialized view, you must have the **SELECT** permission on the materialized view.
-  To refresh a materialized view, you must have the **INSERT** permission on the view and the **SELECT** permission on the base table or column.
-  To delete a materialized view, you must have the **DROP TABLE** permission.

Constraints
-----------

-  Temporary tables cannot be included, including global, volatile, and common temporary tables.
-  The CTE in the definition of a materialized view cannot contain other DML statements except the **SELECT** statement.
-  The base table of a materialized view cannot have data masking policies or row-level access control. It also cannot be owned by a private user. The usage of masking policies and row-level access control may lead to errors in the result set, while materialized views owned by private users may cause refresh failures.
-  9.1.0.200 and later versions support the **stable** and **volatile** functions. Note that materialized views cannot detect function content changes. If function changes need to be fed back to the materialized view as soon as possible, specify **REFRESH MATERIALIZED VIEW RESTRICT** to refresh the materialized view or feed back the data changes to the materialized view.
-  The materialized view cannot detect the parameter modification that affects the query execution. Therefore, you need to refresh the materialized view again.

Usage Suggestions
-----------------

-  Storage resources: Materialized views occupy extra storage space. Therefore, you need to consider the disk capacity.
-  Refresh maintenance: Refreshing the materialized view, whether done automatically or manually, uses system resources. If the data in the base table changes often, the refresh process can impact system performance.
-  Asynchronous materialized views do not update automatically when the original data changes. If the base table data is modified, the view data may become outdated. You need to periodically update the view data to ensure data consistency.
-  Design complexity: When designing and creating materialized views, it is crucial to consider the expected query mode and data access mode to optimize performance.
