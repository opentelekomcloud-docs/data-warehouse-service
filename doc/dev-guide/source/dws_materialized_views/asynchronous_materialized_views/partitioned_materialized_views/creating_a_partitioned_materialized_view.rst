:original_name: dws_04_1423.html

.. _dws_04_1423:

Creating a Partitioned Materialized View
========================================

DWS allows you to create partitioned materialized views in the following scenarios:

-  Partitions are aligned proportionally. The partitioning conditions of materialized views are the same as those of base tables.
-  Partitions are aligned based on time granularity, for example, daily partitioning for the base table and monthly partitioning for the materialized view.
-  Partitions are aligned for multiple base tables with identical partition conditions, automatically mapping the base tables to the materialized view.
-  The base table has multiple partition keys and the first key is chosen as the partition key for creating a materialized view.

Syntax
------

.. code-block::

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

Constraints
-----------

-  You can only use range partitioning.
-  The materialized view only supports a single partition key, which must be the first partition key of the base table. The base table can have a combined partition key.
-  Rollup mode is supported only for time type, using **date_trunc** for rollup calculation. Supported rollup intervals include day, month, quarter, and year.
-  Columns that generate null values in outer joins cannot be used as partition keys.
-  **disable full group by** in MySQL compatibility mode is not supported.
-  Pre-aggregated materialized views cannot be used to create partitioned materialized views.
-  Query rewriting for a specified partition is not allowed.
-  The base table of the partitioned materialized view does not support **SPLIT** and **MERGE** operations.
-  When both sides of a union meet the partition constraint check of the materialized view, a materialized view bound to multiple tables is created.
-  If **JOIN** is used to join base tables and the partition key is set to a public column, a materialized view bound to multiple tables is created if the joined base tables meet the partition constraint detection of the materialized view.
-  Query statements cannot contain sublinks.
-  Query statements cannot contain subplans.
-  Data modification using DML and DDL statements is not allowed. Data can only be updated by refreshing.

Examples
--------

Create fact tables **fact_table** and **fact_table1**.

::

   CREATE TABLE fact_table (start_time TIMESTAMPTZ NOT NULL, id int)PARTITION BY RANGE(start_time)(
   PARTITION p1 VALUES LESS THAN ('2024-01-01'),
   PARTITION p2 VALUES LESS THAN ('2024-01-02'),
   PARTITION p3 VALUES LESS THAN ('2024-02-03'),
   PARTITION p4 VALUES LESS THAN ('2024-03-01'),
   PARTITION p5 VALUES LESS THAN ('2024-03-21'),
   PARTITION p6 VALUES LESS THAN ('2024-04-02'));

::

   CREATE TABLE fact_table1 (start_time TIMESTAMPTZ NOT NULL, id int)PARTITION BY RANGE(start_time)(
   PARTITION p1 VALUES LESS THAN ('2024-01-01'),
   PARTITION p2 VALUES LESS THAN ('2024-01-02'),
   PARTITION p3 VALUES LESS THAN ('2024-02-03'),
   PARTITION p4 VALUES LESS THAN ('2024-03-01'),
   PARTITION p5 VALUES LESS THAN ('2024-03-21'),
   PARTITION p6 VALUES LESS THAN ('2024-04-02'));

Create a dimension table **dimen_table**.

::

   CREATE TABLE dimen_table (start_time TIMESTAMPTZ NOT NULL, id INT);

**Scenario 1: Align partitions proportionally.**

Create the materialized view **level_normal**. Ensure that the partition of the materialized view matches that of the fact_table table, with identical partition conditions and no specific data type requirements.

::

   CREATE MATERIALIZED VIEW level_normal enable query rewrite
   DISTRIBUTE BY HASH(start_time)
   PARTITION BY start_time
   AS SELECT fact_table.start_time, dimen_table.id FROM fact_table JOIN dimen_table ON fact_table.id = dimen_table.id;

|image1|

**Scenario 2: Adjust the partitioning time granularity.**

Use the **date_trunc** parameter on the partition key to create a materialized view with coarser partitioning time granularity compared to the base table. DWS updates the corresponding roll-up partition in the materialized view after detecting data changes in the base table partition.

You can use the **date_trunc('time_unit', column_name)** parameter to specify the roll-up targets. The materialized view partition covers a wider time range compared to the base table partition. For instance, if the base table is partitioned by day, the materialized view can be partitioned by month. The **date_trunc** parameter adjusts the time granularity in the order of day, quarter, year, or month. Supported data types include time, timestamp, and timestamptz.

::

   CREATE MATERIALIZED VIEW level_month enable query rewrite
   DISTRIBUTE BY HASH(start_time)
   PARTITION BY date_trunc('month', start_time)
   AS SELECT fact_table.start_time, dimen_table.id FROM fact_table JOIN dimen_table ON fact_table.id = dimen_table.id;

|image2|

**Scenario 3: Align partitions for multiple base tables.**

If multiple base tables use the same partition key type and partition range, you can create a materialized view bound to multiple base tables.

::

   CREATE MATERIALIZED VIEW level_normal enable query rewrite
   DISTRIBUTE BY HASH(start_time)
   PARTITION BY start_time
   AS SELECT start_time, id FROM fact_table union SELECT start_time, id FROM fact_table1;

::

   CREATE MATERIALIZED VIEW level_normal enable query rewrite
   DISTRIBUTE BY HASH(start_time)
   PARTITION BY start_time
   AS SELECT fact_table1.start_time, fact_table.id, fact_table1.id as id1
   FROM fact_table inner join fact_table1 on fact_table.start_time=fact_table1.start_time;

|image3|

**Scenario 4: When the base table has multiple partition keys, use the first partition key of the base table to create a materialized view.**

::

   CREATE TABLE IF NOT EXISTS  multi_time_base (
       new_start_time timestamp with time zone NOT NULL,
       id int,
       v1 int
   ) PARTITION BY RANGE(new_start_time,id) (
       PARTITION p1 VALUES LESS THAN ('2022-01-01',400),
       PARTITION p2 VALUES LESS THAN ('2022-01-05',400),
       PARTITION p3 VALUES LESS THAN ('2022-01-05',500),
       PARTITION p4 VALUES LESS THAN ('2022-02-15',500),
       PARTITION p5 VALUES LESS THAN ('2022-02-16',500),
       PARTITION p6 VALUES LESS THAN ('2022-03-10',500)
   );
   CREATE MATERIALIZED VIEW multi_time_mv
   PARTITION BY (date_trunc('month', new_start_time))
   AS SELECT new_start_time,id FROM multi_time_base;

.. |image1| image:: /_static/images/en-us_image_0000002084209550.png
.. |image2| image:: /_static/images/en-us_image_0000002119776793.png
.. |image3| image:: /_static/images/en-us_image_0000002084058636.png
