:original_name: dws_06_0358.html

.. _dws_06_0358:

ALTER MATERIALIZED VIEW
=======================

Function
--------

Modifies the properties of a materialized view. This syntax is supported only by clusters of version 8.2.1.220 or later.

Precautions
-----------

To use ALTER MATERIALIZED VIEW, ensure that **enable_matview** is set to **on**.

Syntax
------

-  Set whether to enable query rewrite for materialized views.

   ::

      ALTER MATERIALIZED VIEW [ IF EXISTS ] { materialized_view_name }
          [ ENABLE | DISABLE ] QUERY REWRITE;

-  Specify how materialized views are refreshed. Currently, only **COMPLETE** is available, which rebuilds the entire materialized view from scratch, re-executing the defining query.

   ::

      ALTER MATERIALIZED VIEW [ IF EXISTS ] { materialized_view_name }
          REFRESH [ [DEFAULT] | [ RESTRICT ] | [ CASCADE FORWARD ] | [ CASCADE BACKWARD ] | [ CASCADE ALL ] ] [ COMPLETE ] [ ON DEMAND ] [ [ START WITH (timestamptz) ] | [ EVERY (interval) ] ];

   -  **REFRESH ON DEMAND** indicates that the data is manually refreshed as required.
   -  **START WITH** specifies the first refresh time.
   -  **EVERY** specifies the refresh interval. The value can be **MONTH**, **DAY**, **HOUR**, **MINUTE**, or **SECOND**.

-  Change how materialized views refresh asynchronously in the background. This change applies only to materialized views that refresh this way. This syntax is supported only by clusters of version 9.1.0.200 or later. For details, see :ref:`REFRESH MATERIALIZED VIEW <dws_06_0361>`.

   ::

      ALTER MATERIALIZED VIEW [ IF EXISTS ] { materialized_view_name }
          REFRESH [DEFAULT] | [ RESTRICT ] | [ CASCADE FORWARD ] | [ CASCADE BACKWARD ] | [ CASCADE ALL ] ;

-  Change the owner of the materialized view.

   ::

      ALTER MATERIALIZED VIEW { materialized_view_name }
          OWNER TO new_owner;

-  Allow for setting table properties of materialized views. This syntax is supported only by clusters of 9.1.0.200 and later versions.

   ::

      ALTER MATERIALIZED VIEW { materialized_view_name }
          SET ( {storage_parameter = value} [, ... ] )
          | RESET ( storage_parameter [, ... ] )

-  Rename a view. Materialized views cannot be renamed across schemas. When renaming a materialized view, you can specify a schema name for **materialized_view_name** but cannot specify a schema name for **new_materialized_view_name**.

   ::

      ALTER MATERIALIZED VIEW [ IF EXISTS ] materialized_view_name
          RENAME TO new_materialized_view_name;

Parameter Description
---------------------

.. table:: **Table 1** ALTER MATERIALIZED VIEW parameters

   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                       | Description                                                                                                                                                                                    | Value Range                                                                                                                                                                                                                                                                                                                                                |
   +=================================+================================================================================================================================================================================================+============================================================================================================================================================================================================================================================================================================================================================+
   | IF EXISTS                       | If the specified materialized view does not exist, a message instead of an error is returned.                                                                                                  | ``-``                                                                                                                                                                                                                                                                                                                                                      |
   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | materialized_view_name          | Specifies the name of the materialized view to be modified.                                                                                                                                    | Name of an existing materialized view.                                                                                                                                                                                                                                                                                                                     |
   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ENABLE \| DISABLE QUERY REWRITE | Specifies whether to enable query rewrite.                                                                                                                                                     | Disabled by default.                                                                                                                                                                                                                                                                                                                                       |
   |                                 |                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                            |
   |                                 |                                                                                                                                                                                                | When **ENABLE QUERY REWRITE** is specified, you need to configure the GUC parameter **mv_rewrite_rule** to enable query rewrite of materialized views.                                                                                                                                                                                                     |
   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | REFRESH                         | Specifies how materialized views are refreshed.                                                                                                                                                | -  Refresh method: **DEFAULT** (default refresh) and **RESTRICT** (forcible refresh). You can specify the refresh method only for materialized views that are asynchronously refreshed in the background.                                                                                                                                                  |
   |                                 |                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                            |
   |                                 | When the data in base tables changes, you need to run **REFRESH MATERIALIZED VIEW** to update the data in the materialized views.                                                              | -  Refresh direction: When using materialized views in a cascading nested manner, the refresh direction can be specified as **FORWARD**, **BACKWARD**, or **ALL**. You can specify the refresh direction only for materialized views that are asynchronously refreshed in the background. For details, see :ref:`REFRESH MATERIALIZED VIEW <dws_06_0361>`. |
   |                                 |                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                            |
   |                                 |                                                                                                                                                                                                | -  Currently, only **COMPLETE** is available, which rebuilds the entire materialized view from scratch, re-executing the defining query.                                                                                                                                                                                                                   |
   |                                 |                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                            |
   |                                 |                                                                                                                                                                                                | -  How to trigger a refresh:                                                                                                                                                                                                                                                                                                                               |
   |                                 |                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                            |
   |                                 |                                                                                                                                                                                                |    **ON DEMAND**: manual refresh on demand.                                                                                                                                                                                                                                                                                                                |
   |                                 |                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                            |
   |                                 |                                                                                                                                                                                                |    **START WITH (timestamptz) \| EVERY (interval)**: scheduled refresh. **START WITH** specifies the first refresh time. **EVERY** specifies the refresh interval and its value can be **MONTH**, **DAY**, **HOUR**, **MINUTE**, or **SECOND**.                                                                                                            |
   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | storage_parameter               | Allows for setting table properties of materialized views.                                                                                                                                     | For details, see :ref:`Parameter Description <en-us_topic_0000001811634773__section1561019065710>`.                                                                                                                                                                                                                                                        |
   |                                 |                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                            |
   |                                 | Parameters such as **mv_pck_column**, **bitmap_columns**, **enable_foreign_table_query_rewrite**, **excluded_inactive_tables**, **force_rewrite_timeout**, and **mv_analyze_mode** can be set. |                                                                                                                                                                                                                                                                                                                                                            |
   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | new_owner                       | Specifies the owner of a materialized view.                                                                                                                                                    | Name of an existing user.                                                                                                                                                                                                                                                                                                                                  |
   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | new_materialized_view_name      | Indicates the name of the new materialized view.                                                                                                                                               | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.                                                                                                                                                                                                                                           |
   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

Enable the materialized view function.

::

   set enable_matview = on;

Create a base table and insert data into the base table.

::

   DROP TABLE IF EXISTS t1 CASCADE;
   CREATE TABLE t1 (a int, b int)
   WITH (ORIENTATION = COLUMN )
   DISTRIBUTE BY HASH(a);
   INSERT INTO t1 SELECT x,x FROM generate_series(1,10) x;

Create a materialized view with a specified column-store method.

::

   CREATE MATERIALIZED  VIEW mv1 WITH(orientation = column, enable_foreign_table_query_rewrite = false ) AS SELECT * FROM t1;

Enable query rewrite for a materialized view.

::

   ALTER MATERIALIZED VIEW mv1 ENABLE QUERY REWRITE;

Modify the table properties of a materialized view.

::

   ALTER MATERIALIZED VIEW mv1 SET (force_rewrite_timeout=100);
   ALTER MATERIALIZED VIEW mv1 SET (mv_pck_column='b');
   ALTER MATERIALIZED VIEW mv1 SET (enable_foreign_table_query_rewrite = true);

Change the refresh time of the materialized view.

::

   ALTER MATERIALIZED VIEW mv1 REFRESH START WITH('2025-01-01 15:15:15'::timestamptz) EVERY (interval '60 s');

Helpful Links
-------------

:ref:`CREATE MATERIALIZED VIEW <dws_06_0357>`, :ref:`DROP MATERIALIZED VIEW <dws_06_0360>`, :ref:`REFRESH MATERIALIZED VIEW <dws_06_0361>`
