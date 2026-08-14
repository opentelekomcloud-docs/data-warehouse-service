:original_name: dws_04_1428.html

.. _dws_04_1428:

Refreshing a Nested Materialized View
=====================================

Cascading Refresh
-----------------

To simplify the process of refreshing materialized views, nested materialized views can be automatically refreshed in cascading mode.

There are five supported refresh modes: default, forcible, forward cascading, backward cascading, and full cascading. This means that the **refresh_behavior** value can be set to **DEFAULT**, **RESTRICT**, **CASCADE FORWARD**, **CASCADE BACKWARD**, or **CASCADE ALL**.

-  **DEFAULT**: Refresh a single materialized view. If there are no changes in the base table data, the refresh operation is skipped.
-  **RESTRICT**: Force a refresh on a single materialized view.
-  **CASCADE FORWARD**: Refresh the current materialized view, followed by refreshing the nested materialized views that depend on it in sequence.
-  **CASCADE BACKWARD**: Identify all materialized views that the current view depends on, refresh them from the bottom layer, and then refresh the current view.
-  **CASCADE ALL**: Perform backward cascading refresh and then forward cascading refresh.

.. note::

   If you do not specify a cascading refresh mode, the system will use the default mode, which skips the refresh operation when there are no changes in the data of lower-layer materialized views or tables.

Example:

The nested materialized view created in :ref:`Creating a Nested Materialized View <dws_04_1427>` is used as an example.

Perform a cascading refresh on the materialized view **mv5**.

.. code-block::

   REFRESH MATERIALIZED VIEW mv5 cascade all;

The cascading refresh process involves refreshing **mv1**, **mv5**, and **mv6** sequentially.

This is achieved by performing a backward cascading refresh on **mv5** followed by a forward cascading refresh.

.. code-block::

   REFRESH MATERIALIZED VIEW mv5 cascade backward;
   REFRESH MATERIALIZED VIEW mv5 cascade forward;

In a backward cascading refresh, all bottom-layer materialized views that **mv5** depends on are identified first, and then the materialized views are refreshed layer by layer from the bottom to the top.

.. code-block::

   REFRESH MATERIALIZED VIEW mv5 cascade backward;

Perform the cascading refresh on **mv1** first, then **mv5**.

For cascading refresh, all materialized views dependent on **mv5** are identified first, and then the materialized views are refreshed layer by layer from the bottom to the top.

Perform the cascading refresh on **mv5** first, then **mv6**.

Periodically Refreshing Materialized Views
------------------------------------------

When setting up materialized views that are refreshed periodically, you must define the refresh mode. If not specified, the default mode is used, and cascading refresh is not carried out.

Therefore, for materialized views, you can specify the cascading refresh mode during creation and make changes as needed.

Examples:

#. Create a scheduled task for refreshing materialized views.

   .. code-block::

      CREATE TABLE test_table(a int2, b int, c int8, d int) WITH (ORIENTATION = COLUMN, COLVERSION = 2.0) DISTRIBUTE BY hash(c);
      create materialized view mv_test_sync_default REFRESH START WITH('2020-01-01 12:12:12'::timestamptz) EVERY (interval '1 s')  ENABLE QUERY REWRITE as
      select  a,sum(b) b from test_table group by a;
      create materialized view mv_test_sync_restrict REFRESH restrict START WITH('2020-01-01 12:12:12'::timestamptz) EVERY (interval '1 s') ENABLE QUERY REWRITE as
      select  a,sum(b) b from test_table group by a;
      create materialized view mv_test_sync_forward REFRESH cascade forward START WITH('2020-01-01 12:12:12'::timestamptz) EVERY (interval '1 s') ENABLE QUERY REWRITE as
      select  a,sum(b) b from test_table group by a;
      create materialized view mv_test_sync_backward REFRESH cascade backward START WITH('2020-01-01 12:12:12'::timestamptz) EVERY (interval '1 s') ENABLE QUERY REWRITE as
      select  a,sum(b) b from test_table group by a;
      create materialized view mv_test_sync_all REFRESH cascade all START WITH('2020-01-01 12:12:12'::timestamptz) EVERY (interval '1 s') ENABLE QUERY REWRITE as
      select  a,sum(b) b from test_table group by a;

#. View the scheduled update task.

   .. code-block::

      SELECT what, category_id, is_broken, interval FROM scheduler.pg_task WHERE what LIKE '%mv_test%' ORDER BY 1;

   |image1|

#. Modify the cascading refresh mode.

   .. code-block::

      ALTER MATERIALIZED VIEW mv_test_sync_backward REFRESH START WITH('2020-01-01 15:15:15'::timestamptz) EVERY (interval '10 s') ;
      ALTER MATERIALIZED VIEW mv_test_sync_restrict REFRESH cascade all START WITH('2020-01-01 12:12:12'::timestamptz) EVERY (interval '20 s') ;
      ALTER MATERIALIZED VIEW mv_test_sync_all REFRESH  default;
      ALTER MATERIALIZED VIEW mv_test_sync_forward REFRESH default START WITH('2020-01-01 12:12:12'::timestamptz) EVERY (interval '50 s') ;

   |image2|

.. |image1| image:: /_static/images/en-us_image_0000002069905908.png
.. |image2| image:: /_static/images/en-us_image_0000002105745569.png
