:original_name: dws_06_0357.html

.. _dws_06_0357:

CREATE MATERIALIZED VIEW
========================

Function
--------

Creates a materialized view.

Precautions
-----------

-  The base table of a materialized view can be a row-store table, column-store table, H-Store table, partitioned table, or a specified partition, foreign table, or other materialized views. However, temporary tables (such as global, volatile, and common temporary tables) are not allowed. Starting from version 910.200, hot and cold tables are supported. Note that it is not possible to specify partitions for automatically partitioned tables.
-  Materialized views do not allow **INSERT**, **UPDATE**, **MERGE INTO**, or **DELETE** operations.
-  The result of each query to the materialized view saved to ensure that the query results are the same each time. After the **BUILD IMMEDIATE** or **REFRESH** operation, the correct result can be queried in the materialized view.
-  A node group cannot be specified for a materialized view using syntax. A node group can be specified for the base table of a materialized view. The materialized view can inherit the node group information from the base table. The node groups of multiple base tables must be the same.
-  To create a materialized view, you must have the CREATE permission for the schema and the SELECT permission for the base table or column.
-  To query a materialized view, you must have the SELECT permission on the materialized view.
-  To refresh a materialized view, you must have the INSERT permission on the materialized view and SELECT permission on the base table or column.
-  Materialized views support fine-grained permissions such as ANALYZE, VACUUM, ALTER, and DROP.
-  Materialized views support permission transfer through the **transfer with grant** option.
-  Materialized views do not support higher-level security control. In scenarios where the SELECT permission is restricted, for example, a base table has row-level access control or masking policies or its owner is a private user, materialized views cannot be created. If a materialized view already exists and an RLS or masking policy is added to the base table or the owner of the base table is changed to a private user, the materialized view can be queried but cannot be refreshed.

Constraints
-----------

-  Temporary tables cannot be included, including global temporary tables, volatile temporary tables, and common temporary tables.
-  The CTE in the definition of a materialized view cannot contain other DML statements except the SELECT statement.
-  The base table of a materialized view cannot have masking policies and row-level access control, or belong to a private user. Masking policies and row-level access control may cause result set errors, while private users may cause problems such as materialized view refresh failures.

Syntax
------

::

   CREATE MATERIALIZED VIEW [ IF NOT EXISTS ] materialized_view_name [ ( column_name [, ...] ) ]
   [ BUILD { DEFERRED | IMMEDIATE } ]
   [ REFRESH  [ COMPLETE ] [ ON DEMAND ] [ [ START WITH (timestamptz) ] | [ EVERY (interval) ] ] ]
   [ { ENABLE | DISABLE } QUERY REWRITE ]
   [ WITH ( {storage_parameter = value} [, ... ] ) ]
   [ DISTRIBUTE BY { REPLICATION | ROUNDROBIN | { HASH ( column_name [,...] ) } } ]
   AS query;

::

   CREATE MATERIALIZED VIEW [ IF NOT EXISTS ] materialized_view_name
   [ ( column_name [, ...] ) ]
   [ BUILD IMMEDIATE ]
   REFRESH AUTO
   ENABLE QUERY REWRITE
   [ WITH ( {storage_parameter = value} [, ... ] ) ]
   AS query;

.. _en-us_topic_0000001811634773__section1561019065710:

Parameter Description
---------------------

-  **BUILD DEFERRED \| IMMEDIATE**

   **IMMEDIATE** indicates that the latest data is included when the materialized view is created.

   **DEFERRED** indicates that data is included only when the materialized view is refreshed for the first time.

-  .. _en-us_topic_0000001811634773__li93006216815:

   **REFRESH**

   Specifies the refresh mode of a materialized view.

   After a materialized view is created, the data in the materialized view reflects only the status of the base table at the creation time. When the data in the base table changes, you need to refresh the materialized view (**REFRESH MATERIALIZED VIEW**) to update the data in the materialized view.

   -  Currently, only the **COMPLETE** refresh mode is supported, which refresh full data in the materialized view. Execute the query statement defined in the materialized view to update the materialized view.

   -  Refresh triggering mode.

      **ON DEMAND**: manual refresh on demand.

      **START WITH (timestamptz) \| EVERY (interval)**: scheduled refresh. **START WITH** specifies the first refresh time and **EVERY** specifies the refresh interval. The value can be **MONTH**, **DAY**, **HOUR**, **MINUTE**, or **SECOND**.

-  **REFRESH** **AUTO**

   Specifies the refresh method of a materialized view.

-  **ENABLE \| DISABLE QUERY REWRITE**

   Indicates whether query rewriting is supported. By default, query rewriting is not supported.

   When **ENABLE QUERY REWRITE** is specified, the GUC parameter **mv_rewrite_rule** must be set to enable query rewriting for materialized views.

   .. note::

      Query rewriting means that when a base table is queried, if a materialized view is created for the base table, the database system automatically determines whether the pre-calculated result in the materialized view can be used to process the query.

      If a materialized view can be used, the pre-calculated result is directly read from the materialized view to accelerate the query.

-  **WITH ( { storage_parameter = value } [, ... ] )**

   -  ORIENTATION

      Specifies the storage mode (row-store, column-store) for table data. This parameter cannot be modified once it is set.

      -  Value range:

         -  **ROW** indicates that table data is stored in rows.

            **ROW** applies to OLTP service, which has many interactive transactions. An interaction involves many columns in the table. Using ROW can improve the efficiency.

         -  **COLUMN** indicates that the data is stored in columns.

            **COLUMN** applies to the data warehouse service, which has a large amount of aggregation computing, and involves a few column operations.

      -  Default value:

         **row**: creates a row-store table.

   -  Materialized views do not support the following storage types: foreign tables and time series tables.

   -  enable_foreign_table_query_rewrite

      Specifies whether to allow query rewriting on materialized views that contain foreign tables. This parameter must be used together with **ENABLE QUERY REWRITE**.

      The materialized view cannot detect the data changes in the foreign table. Specify this option if you want to enable query rewriting for materialized views that contain foreign tables.

      Value range:

      -  **on**: allows query rewriting on materialized views that contain foreign tables.
      -  **off**: does not allow query rewriting on materialized views that contain foreign tables.

      Default value: **off**

   -  bitmap_columns

      The bitmap index is only applicable to the **hstore_opt** table. To use it, enable the table-level parameter **enable_hstore_opt** and set **bitmap_columns** to the specified column. This is supported only by clusters of version 9.1.0.200 or later.

   -  secondary_part_num

      Specifies the number of level-2 partitions in a column-store table. This parameter applies only to H-Store column-store tables. This is supported only by clusters of version 9.1.0.200 or later.

      Value range: **1** to **32**

      Default value: **8**

   -  enable_hstore_opt

      When the **enable_hstore_opt** table-level parameter is enabled, the **enable_hstore** table-level parameter is also automatically enabled by default. This is supported only by clusters of version 9.1.0.200 or later.

      Default value: **false**

   -  enable_turbo_store

      Determines whether to create a turbo table (column-store tables). The parameter is only valid for column-store tables. This is supported only by clusters of version 9.1.0.200 or later.

      Default value: **off**

   -  mv_analyze_mode

      Determines the automatic analysis method for materialized views. This is supported only by clusters of version 9.1.0.200 or later.

      Value range:

      **none**: indicates that the materialized view does not automatically execute **ANALYZE** after being refreshed.

      **light**: indicates that the materialized view performs light analysis after being refreshed.

      Default value: **light**

   -  mv_pck_column

      Specifies a partial clustering storage for a materialized view. During the data import process to a column-store table, the data is partially sorted according to the specified column(s). This is supported only by clusters of version 9.1.0.200 or later.

      Enable **mv_pck_column** and set it to a specified column.

   -  mv_support_function_type

      Enables the use of function attributes in the query statements when creating materialized views. This is supported only by clusters of version 9.1.0.200 or later.

      Value range:

      **stable**: indicates that functions of the STABLE and IMMUTABLE types can be used in query statements.

      **volatile**: indicates that functions of the VOLATILE, STABLE, and IMMUTABLE types can be used in query statements.

      Default value: empty

   -  excluded_inactive_tables

      Specifies that the materialized view will not be invalidated when there are data changes in the underlying base table. This is supported only by clusters of version 9.1.0.200 or later.

      Set **excluded_inactive_tables** to **schemaName1.tableName1,schemaName2.tableName2**.

      Default value: empty

   -  force_rewrite_timeout

      Allows query rewrite within a specified time interval after refresh, regardless of the freshness of the data in the materialized view. This is supported only by clusters of version 9.1.0.200 or later.

      The unit is second. The default value is **0**.

-  TABLESPACE tablespace_name

   Specifies a tablespace for V3 storage. If **default_tablespace** is empty, the default tablespace of the database is used. This is supported only by clusters of version 9.1.0.200 or later.

-  **DISTRIBUTE BY**

   Specifies how the table is distributed or replicated between DNs.

   Value range:

   -  **REPLICATION**: Each row in the table exists on all DNs, that is, each DN has complete table data.
   -  **ROUNDROBIN**: Each row in the table is sent to each DN in turn. Therefore, data is evenly distributed on each DN. This value is supported only in 8.1.2 or later.
   -  **HASH**: Each row of the table will be placed into all the DNs based on the hash value of the specified column.

   Default value: determined by the **default_distribution_mode** parameter.

   .. note::

      When the materialized view is distributed in hash mode, data skew may occur. To check for data skews in materialized views, follow the same procedures used for detecting data skews in regular tables. For details, see "Checking for Data Skew" in the *Data Warehouse Service (DWS) Developer Guide*. If data skew is identified in a materialized view, perform data skew optimization at the storage layer by referring to "Optimizing Data Skew" in the *Data Warehouse Service (DWS) Developer Guide*.

-  **AS query**

   Creates a materialized view based on the query result.

Examples
--------

Create a base table and insert data into the base table.

::

   CREATE TABLE t1 (a int, b int) DISTRIBUTE BY HASH(a);
   INSERT INTO t1 SELECT x,x FROM generate_series(1,10) x;

Create a materialized view with the default option **BUILD IMMEDIATE**.

::

   CREATE MATERIALIZED VIEW mv1 AS SELECT * FROM t1;

Create a materialized view in column-store.

::

   CREATE MATERIALIZED VIEW mv2 WITH(orientation = column) AS SELECT * FROM t1;

Create a materialized view that is manually refreshed as required.

::

   CREATE MATERIALIZED VIEW mv3 BUILD DEFERRED REFRESH ON DEMAND AS SELECT * FROM t1;

Create a materialized view with a scheduled refresh time.

::

   CREATE MATERIALIZED VIEW mv4 BUILD DEFERRED REFRESH START WITH(trunc(sysdate)) EVERY (interval '1 day') AS SELECT * FROM t1;

Create a materialized view with a bitmap index.

::

   CREATE MATERIALIZED VIEW mv1
   with (ORIENTATION = COLUMN, enable_hstore=true, enable_hstore_opt=on, bitmap_columns='col1')  AS SELECT * FROM base_table;

Create a materialized view and specify the number of level-2 partitions in a column-store table.

::

   CREATE MATERIALIZED VIEW mv
   WITH (ORIENTATION=COLUMN, ENABLE_HSTORE=ON, enable_hstore_opt=on, mv_pck_column='c3', secondary_part_column = 'c2', secondary_part_num = 8)  AS SELECT * FROM base_table;

Create a materialized view and specify the PCK column for sorting.

::

   CREATE MATERIALIZED VIEW mv
   WITH (ORIENTATION=COLUMN, ENABLE_HSTORE=ON, enable_hstore_opt=on, mv_pck_column='col3')  AS SELECT * FROM base_table;

Create a materialized view and specify the analysis method.

::

   CREATE MATERIALIZED VIEW mv1 enable query rewrite with(excluded_inactive_tables='matview_basic."T1",matview_basic."a=b"',mv_analyze_mode='none') as SELECT * FROM base_table;

Create a V3 materialized view.

::

   CREATE MATERIALIZED VIEW mv1
   with (orientation=column, enable_hstore=true, compression=low, enable_hstore_opt=on, COLVERSION = 3.0) TABLESPACE cu_obs_tbs distribute by hash(scope_name)
   AS SELECT * FROM dicttbl_low;

Create a materialized view that contains a foreign table for query rewriting.

::

   CREATE MATERIALIZED VIEW mv1 with (enable_foreign_table_query_rewrite = true) as SELECT * FROM base_table;

Create a materialized view and specify that volatile functions can be used in the query statement.

::

   CREATE MATERIALIZED VIEW mv_date with(mv_support_function_type = 'volatile') as select to_date(a) from t_date;

Helpful Links
-------------

:ref:`ALTER MATERIALIZED VIEW <dws_06_0358>`, :ref:`DROP MATERIALIZED VIEW <dws_06_0360>`, :ref:`REFRESH MATERIALIZED VIEW <dws_06_0361>`
