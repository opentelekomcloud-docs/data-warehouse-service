:original_name: dws_04_1046.html

.. _dws_04_1046:

PGXC_STAT_TABLE_DIRTY
=====================

**PGXC_STAT_TABLE_DIRTY** displays statistics about all the tables on all the CNs and DNs in the current cluster, and the dirty page rate of tables on a single CN or DN. This view is supported only by clusters of version 8.1.3 or later.

.. note::

   The statistics of this view depend on the **ANALYZE** operation. To obtain the most accurate information, perform the **ANALYZE** operation on the table first.

.. table:: **Table 1** PGXC_STAT_TABLE_DIRTY columns

   +-------------------+-------------------------+--------------------------------------------+
   | Name              | Type                    | Description                                |
   +===================+=========================+============================================+
   | nodename          | text                    | Node name                                  |
   +-------------------+-------------------------+--------------------------------------------+
   | schema            | name                    | Schema name of the table                   |
   +-------------------+-------------------------+--------------------------------------------+
   | tablename         | name                    | Table name                                 |
   +-------------------+-------------------------+--------------------------------------------+
   | partname          | name                    | Partition name of the partitioned table    |
   +-------------------+-------------------------+--------------------------------------------+
   | last_vacuum       | timestampwith time zone | Time of the last manual **VACUUM**         |
   +-------------------+-------------------------+--------------------------------------------+
   | last_autovacuum   | timestampwith time zone | Time of the last **AUTOVACUUM**            |
   +-------------------+-------------------------+--------------------------------------------+
   | last_analyze      | timestampwith time zone | Time of the last manual **ANALYZE**        |
   +-------------------+-------------------------+--------------------------------------------+
   | last_antoanalyze  | timestampwith time zone | Time of the last **AUTOANALYZE**           |
   +-------------------+-------------------------+--------------------------------------------+
   | vacuum_count      | bigint                  | Number of times **VACUUM** operations      |
   +-------------------+-------------------------+--------------------------------------------+
   | autovacuum_count  | bigint                  | Number of **AUTOVACUUM** operations        |
   +-------------------+-------------------------+--------------------------------------------+
   | analyze_count     | bigint                  | Number of **ANALYZE** operations           |
   +-------------------+-------------------------+--------------------------------------------+
   | autoanalyze_count | bigint                  | Number of **AUTOANALYZE_COUNT** operations |
   +-------------------+-------------------------+--------------------------------------------+
   | n_tup_ins         | bigint                  | Number of rows inserted                    |
   +-------------------+-------------------------+--------------------------------------------+
   | n_tup_upd         | bigint                  | Number of rows updated                     |
   +-------------------+-------------------------+--------------------------------------------+
   | n_tup_del         | bigint                  | Number of rows deleted                     |
   +-------------------+-------------------------+--------------------------------------------+
   | n_tup_hot_upd     | bigint                  | Number of rows with HOT updates            |
   +-------------------+-------------------------+--------------------------------------------+
   | n_tup_change      | bigint                  | Number of changed rows after **ANALYZE**   |
   +-------------------+-------------------------+--------------------------------------------+
   | n_live_tup        | bigint                  | Estimated number of live rows              |
   +-------------------+-------------------------+--------------------------------------------+
   | n_dead_tup        | bigint                  | Estimated number of dead rows              |
   +-------------------+-------------------------+--------------------------------------------+
   | dirty_rate        | bigint                  | Dirty page rate of a single CN or DN       |
   +-------------------+-------------------------+--------------------------------------------+
   | last_data_changed | timestampwith time zone | Time when a table was last modified        |
   +-------------------+-------------------------+--------------------------------------------+

Suggestion
----------

-  Before running **VACUUM FULL** on a system catalog with a high dirty page rate, ensure that no user is performing operations on it.
-  You are advised to run **VACUUM FULL** to tables (excluding system catalogs) whose dirty page rate exceeds 80% or run it based on service scenarios.

Scenarios
---------

#. Query the overall dirty page rate of all the user tables in a database.

   ::

      select
          t1.schema,
          t1.tablename,
          t1.total_ins,
          t1.total_upd,
          t1.total_del,
          t1. total_tup_hot_upd,
          t1.total_change,
          t1.total_live,
          t1.total_dead,
          t1.total_dirty_rate,
          t1.max_dirty,
          t2.max_node,
          t1.min_dirty,
          t2.min_node
      from
          (select
              a.schema,
              a.tablename,
              sum(a.n_tup_ins) as total_ins,
              sum(a.n_tup_upd) as total_upd,
              sum(a.n_tup_del) as total_del,
              sum(a.n_tup_hot_upd) as total_tup_hot_upd,
              sum(a.n_tup_change) as total_change,
              sum(a.n_live_tup) as total_live,
              sum(a.n_dead_tup) as total_dead,
              Round((total_dead / (total_dead + total_live + 0.0001) * 100),2) AS total_dirty_rate,
              max(a.dirty_rate) as max_dirty,
              min(a.dirty_rate) as min_dirty
          from pg_catalog.pgxc_stat_table_dirty a where a.partname is null and a.schema not in ('pg_toast','cstore','gs_logical_cluster','sys','dbms_om','information_schema','pg_catalog','dbms_output','dbms_random','utl_raw','utl_raw dbms_sql','dbms_lob') group by a.tablename, a.schema
          ) t1,
          (select distinct
          tablename, schema,
          first_value(nodename) over(partition by tablename, schema order by dirty_rate) as min_node,
          first_value(nodename) over(partition by tablename, schema order by dirty_rate desc) as max_node
          from (select * from pg_catalog.pgxc_stat_table_dirty)) t2
      where t1.tablename = t2.tablename and t1.schema = t2.schema;

#. Query the overall dirty page rate of all the tables (user tables and system catalogs) in a database.

   ::

      select
          t1.schema,
          t1.tablename,
          t1.total_ins,
          t1.total_upd,
          t1.total_del,
          t1. total_tup_hot_upd,
          t1.total_change,
          t1.total_live,
          t1.total_dead,
          t1.total_dirty_rate,
          t1.max_dirty,
          t2.max_node,
          t1.min_dirty,
          t2.min_node
      from
          (select
              a.schema,
              a.tablename,
              sum(a.n_tup_ins) as total_ins,
              sum(a.n_tup_upd) as total_upd,
              sum(a.n_tup_del) as total_del,
              sum(a.n_tup_hot_upd) as total_tup_hot_upd,
              sum(a.n_tup_change) as total_change,
              sum(a.n_live_tup) as total_live,
              sum(a.n_dead_tup) as total_dead,
              Round((total_dead / (total_dead + total_live + 0.0001) * 100),2) AS total_dirty_rate,
              max(a.dirty_rate) as max_dirty,
              min(a.dirty_rate) as min_dirty
          from pg_catalog.pgxc_stat_table_dirty a where a.partname is null group by a.tablename, a.schema
          ) t1,
          (select distinct
          tablename, schema,
          first_value(nodename) over(partition by tablename, schema order by dirty_rate) as min_node,
          first_value(nodename) over(partition by tablename, schema order by dirty_rate desc) as max_node
          from (select * from pg_catalog.pgxc_stat_table_dirty)) t2
      where t1.tablename = t2.tablename and t1.schema = t2.schema;

#. Query all system catalogs in a database.

   ::

      select * from pgxc_stat_table_dirty where schema in ('pg_toast','cstore','gs_logical_cluster','sys','dbms_om','information_schema','pg_catalog','dbms_output','dbms_random','utl_raw','utl_raw dbms_sql','dbms_lob');
