:original_name: dws_04_1424.html

.. _dws_04_1424:

Refreshing a Partition Materialized View
========================================

Using partition refresh reduces the data update costs after a partitioned materialized view is created.

Enabling Auto Refresh
---------------------

The system automatically identifies and refreshes the materialized view. You can run the following command to trigger a refresh:

.. code-block::

   REFRESH MATERIALIZED VIEW materialized_view_name;

This command initiates a refresh of the materialized view. If the conditions for partition refresh are met, the system refreshes the partition; otherwise, a full refresh is performed.

Manually Refreshing a Specified Partition
-----------------------------------------

Run the following command to specify the materialized view partition to be refreshed:

.. code-block::

   REFRESH MATERIALIZED VIEW materialized_view_name PARTITION (partname1,partname2);

This command refreshes the specified partitions, **partname1** and **partname2**, of the materialized view. If the current partition does not require refreshing, the system displays a message indicating that the data is up to date.

Forcibly and Manually Refreshing a Specified Partition
------------------------------------------------------

Run the following command to forcibly refresh the specified partition of the materialized view:

.. code-block::

   REFRESH MATERIALIZED VIEW materialized_view_name PARTITION (partname) RESTRICT;

Viewing a Partitioned Materialized View
---------------------------------------

Check whether the partition data is updated.

Use the **active** column of **pg_matview_partition** to check whether the current partition has been updated.

.. code-block::

   \d  pg_matview_partition;
   Table "pg_catalog.pg_matview_partition"
         Column        |           Type           | Modifiers
   --------------------+--------------------------+-----------
   matpartid           | oid                      | not null
   active              | boolean                  | not null
   refresh_start_time  | timestamp with time zone | not null
   refresh_finish_time | timestamp with time zone | not null

Where,

-  **matpartid** indicates the OID of the materialized view partition.
-  **active** indicates whether the current partition has been updated. **t** indicates updated and **f** indicates invalid.
-  **refresh_start_time** indicates the start time of the last partition refresh.
-  **refresh_finish_time** indicates the end time of the last partition refresh.

Examples
--------

#. Create the materialized view **level_month** and check the view information.

   .. code-block::

      CREATE MATERIALIZED VIEW level_month enable query rewrite
      DISTRIBUTE BY HASH(start_time)
      PARTITION BY date_trunc('month', start_time)
      AS SELECT fact_table.start_time, dimen_table.id FROM fact_table JOIN dimen_table ON fact_table.id = dimen_table.id;

   ::

      SELECT * FROM pg_matview_partition;
      matpartid | active |      refresh_start_time       |      refresh_finish_time
      ----------+--------+-------------------------------+-------------------------------
         17092  | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
         17093  | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
         17094  | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
         17095  | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
         17096  | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08

#. Insert data into the base table.

   ::

      INSERT INTO fact_table (start_time, id) VALUES ('2023-05-01',1);

#. Check the partition invalidation information.

   ::

      SELECT * FROM pg_matview_partition;
      matpartid | active |      refresh_start_time       |      refresh_finish_time
      ----------+--------+-------------------------------+-------------------------------
         17092  | f      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
         17093  | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
         17094  | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
         17095  | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
         17096  | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08

#. Refresh a materialized view.

   ::

      REFRESH MATERIALIZED VIEW level_month;

   Partition refresh is triggered because the problem is partition invalidity.

#. Check the status after the refresh.

   ::

      SELECT * FROM pg_matview_partition;
      matpartid | active |      refresh_start_time       |      refresh_finish_time
      ----------+--------+-------------------------------+-------------------------------
        17093   | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
        17094   | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
        17095   | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
        17096   | t      | 2024-11-18 15:15:32.177697+08 | 2024-11-18 15:15:32.204009+08
        17092   | t      | 2024-11-18 15:16:28.902902+08 | 2024-11-18 15:16:28.940159+08
