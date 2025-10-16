:original_name: dws_06_0358.html

.. _dws_06_0358:

ALTER MATERIALIZED VIEW
=======================

Function
--------

Modifies the properties of a materialized view.

Precautions
-----------

None

Syntax
------

::

   ALTER MATERIALIZED VIEW [ IF EXISTS ] { materialized_view_name }
       [ ENABLE | DISABLE ] QUERY REWRITE;
   ALTER MATERIALIZED VIEW [ IF EXISTS ] { materialized_view_name }
       REFRESH [  [ COMPLETE ] [ ON DEMAND ] [ [ START WITH (timestamptz) ] | [ EVERY (interval) ] ];
   ALTER MATERIALIZED VIEW { materialized_view_name }
       OWNER TO new_owner;
   ALTER MATERIALIZED VIEW { materialized_view_name }
       SET ( {storage_parameter = value} [, ... ] )
       | RESET ( storage_parameter [, ... ] )

Parameter Description
---------------------

-  **ENABLE \| DISABLE QUERY REWRITE**

   Indicates whether to enable query rewriting for a materialized view.

   After enabling query rewriting for the materialized view, refresh the materialized view to ensure the data is up-to-date.

-  **REFRESH [ COMPLETE ] [ ON DEMAND ] [ [ START WITH (timestamptz) ] \| [EVERY (interval)] ]**

   Modifies the method of refreshing a materialized view.

   -  Currently, only the **COMPLETE** refresh mode is supported, which refresh full data in the materialized view. Execute the query statement defined in the materialized view to update the materialized view.

   -  Refresh the triggering method.

      **ON DEMAND**: manual refresh on demand.

      **START WITH (timestamptz) \| EVERY (interval)**: scheduled refresh. **START WITH** specifies the first refresh time. **EVERY** specifies the refresh interval. The value can be **MONTH**, **DAY**, **HOUR**, **MINUTE**, or **SECOND**.

-  **SET ( {storage_parameter = value} [, ... ] ) \| RESET ( storage_parameter [, ... ] )**

   Allows for setting table properties of materialized views. This syntax is supported only by clusters of 9.1.0.200 and later versions.

   Parameters such as mv_pck_column, bitmap_columns, enable_foreign_table_query_rewrite, excluded_inactive_tables, force_rewrite_timeout and mv_analyze_mode can be set. For details, see :ref:`Parameter Description <en-us_topic_0000001811634773__section1561019065710>`.

-  **OWNER TO new_owner**

   Change the owner of a materialized view.

Examples
--------

Enable query rewriting for a materialized view.

::

   ALTER MATERIALIZED VIEW mv1 ENABLE QUERY REWRITE;
   NOTICE:  REFRESH MATERIALIZED VIEW should be executed to enable query rewrite.
   ALTER MATERIALIZED VIEW

Modify the table properties of a materialized view.

::

   ALTER MATERIALIZED VIEW mv1 SET (force_rewrite_timeout=100);
   ALTER MATERIALIZED VIEW mv1 SET (mv_pck_column='col1');
   ALTER MATERIALIZED VIEW mv1 SET (enable_foreign_table_query_rewrite = true);

Change the refresh time of the materialized view.

::

   ALTER MATERIALIZED VIEW mv1 REFRESH START WITH('2025-01-01 15:15:15'::timestamptz) EVERY (interval '60 s');

Helpful Links
-------------

:ref:`CREATE MATERIALIZED VIEW <dws_06_0357>`, :ref:`DROP MATERIALIZED VIEW <dws_06_0360>`, :ref:`REFRESH MATERIALIZED VIEW <dws_06_0361>`
