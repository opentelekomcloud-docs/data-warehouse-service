:original_name: dws_04_1427.html

.. _dws_04_1427:

Creating a Nested Materialized View
===================================

DWS enables the creation of nested materialized views, which involve using other materialized views in defining a view. By nesting materialized views, you can merge multiple views to generate more intricate and comprehensive query results.

The GUC parameter :ref:`mv_max_cascade_depth <en-us_topic_0000001811490801__section4711162714313>` is used to specify the maximum depth of a nested materialized view. To ensure refresh performance, the maximum depth is 10. The default value is 3.

Constraints
-----------

1. Nesting of pre-aggregated views is not allowed.

2. Pre-aggregated materialized views cannot be based on other materialized views.

3. The refresh mode of a materialized view cannot be set or changed unless it is refreshed periodically.

4. The maximum depth for nested materialized views is 10, with no support for deeper nesting layers currently.

5. Nested materialized views cannot be created during an upgrade.

Syntax
------

The syntax for creating a nested materialized view is similar to creating a regular materialized view. In the view definition statement, the base table can be another materialized view. Use **refresh_behavior** to schedule periodic refreshes and specify the refresh mode.

.. code-block::

   CREATE MATERIALIZED VIEW [ IF NOT EXISTS ] materialized_view_name
   [ ( column_name [, ...] ) ]
   [ BUILD { DEFERRED | IMMEDIATE } ]
   [ REFRESH [ refresh_behavior] [ COMPLETE ] [ ON DEMAND ] [ [ START WITH (timestamptz) ] | [ EVERY (interval) ] ] ]
   [ { ENABLE | DISABLE } QUERY REWRITE ]
   [ WITH ( {storage_parameter = value} [, ... ] ) ]
   [ TABLESPACE tablespace_name ]
   [ DISTRIBUTE BY { REPLICATION | ROUNDROBIN | { HASH ( column_name [,...] ) } } ]
   [ PARTITION BY {(column_name) | date_trunc('time_unit', column_name)} ]
   AS query;

Viewing a Nested Materialized View
----------------------------------

The **relid** column in the **pg_matview_depend** system catalog stores the OIDs of materialized views.

.. code-block::

   \d+ pg_matview_depend
   Table "pg_catalog.pg_matview_depend"
   Column             |   Type    | Modifiers | Storage | Stats target | Description
   -------------------+-----------+-----------+---------+--------------+-------------
   matviewid          | oid       | not null  | plain   |              |
   relid              | oid       | not null  | plain   |              |
   partids            | oidvector | not null  | plain   |              |
   contain_entire_rel | boolean   | not null  | plain   |              |
   Indexes:
   "pg_matview_depend_mvid_relid_index" UNIQUE, btree (matviewid, relid) TABLESPACE pg_default
   "pg_matview_depend_relid_index" btree (relid) TABLESPACE pg_default
   Replica Identity: NOTHING
   Has OIDs: no

Examples
--------

1. Create base tables, materialized views, and nested materialized views.

.. code-block::

   CREATE TABLE t1(a int, b int, c int);
   CREATE TABLE t2(a int, b int, c int);
   CREATE MATERIALIZED VIEW mv1 ENABLE QUERY REWRITE as select * from t1 WHERE a = 1;
   CREATE MATERIALIZED VIEW mv2 ENABLE QUERY REWRITE as select * from t2 WHERE b = 1;
   CREATE MATERIALIZED VIEW mv3 ENABLE QUERY REWRITE as select * from mv2 WHERE b = 1;
   CREATE MATERIALIZED VIEW mv4 ENABLE QUERY REWRITE as select t1.a as t1a,t1.b as t1b,t1.c as t1c,t2.a as t2a,t2.b as t2b,t2.c as t2c from t1 left join t2 on t1.a = t2.a WHERE t2.b = 1;
   CREATE MATERIALIZED VIEW mv5 ENABLE QUERY REWRITE as (select * from mv1) union all (select * from t1 WHERE a = 1);
   CREATE MATERIALIZED VIEW mv6 ENABLE QUERY REWRITE as select * from mv4 left join (select * from mv1 union all select * from mv5)mv5 on mv4.t1a = mv5.a;

2. In the SQL statement above, tables **t1** and **t2** are created, followed by the creation of materialized views **mv1** and **mv2** based on **t1** and **t2**. Subsequently, create materialized views **mv3**, **mv4**, **mv5**, and **mv6** sequentially using **t1**, **t2**, **mv1**, and **mv2**.

Nested materialized views offer a simpler definition and more flexible usage compared to single-layer materialized views.

3. The figure below illustrates the creation relationship and dependency between base tables and materialized views.


.. figure:: /_static/images/en-us_image_0000002121960909.png
   :alt: **Figure 1** Dependency between nested materialized views

   **Figure 1** Dependency between nested materialized views

4. When there are changes in the data at the lowest level, you must refresh the materialized view to ensure its data validity. For example, changes in the **t1** table require updates in the **mv1**, **mv4**, **mv5**, and **mv6** tables. Refreshing a single materialized view in cascading mode automatically updates all associated materialized views.

.. code-block::

   REFRESH MATERIALIZED VIEW mv6 cascade backward;

5. Once all data in a nested materialized view is valid, the system automatically matches and retrieves the data when executing a query statement. It then automatically converts the query statement into a materialized view query, significantly reducing the computational load in the query statement.
