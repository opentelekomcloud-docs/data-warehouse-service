:original_name: dws_04_0394.html

.. _dws_04_0394:

User Resource Query
===================

Context
-------

In the multi-tenant management framework, you can query the real-time or historical usage of all user resources (including memory, CPU cores, storage space, temporary space, and I/Os).

.. note::

   -  User real-time resource views/functions: :ref:`PG_TOTAL_USER_RESOURCE_INFO <dws_04_0790>` and GS_WLM_USER_RESOURCE_INFO; user historical resource table: :ref:`GS_WLM_USER_RESOURCE_HISTORY <dws_04_0567>`.
   -  In the preceding views and tables, the value of **used_cpu** indicates the CPU usage of a user's resource pool. The resource pool records only the CPU usage of long queries.
   -  In the preceding views and tables, I/O-related resource statistics only record I/O read and write data of long queries executed by users.
   -  When there are a large number of users and a large cluster, querying such real-time views will cause network latency due to the real-time communication overhead between CNs and DNs.
   -  User memory and CPU monitoring does not apply to short queries or administrator jobs.

Procedure
---------

-  Query all users' resource quotas and real-time resource usage.

   ::

      SELECT * FROM PG_TOTAL_USER_RESOURCE_INFO;

   The result view is as follows:

   .. code-block::

      username        | used_memory | total_memory | used_cpu | total_cpu | used_space | total_space | used_temp_space | total_temp_space | used_spill_space | total_spill_space | read_kbytes | write_kbytes | read_counts | write_counts | read_speed | write_speed
      -----------------------+-------------+--------------+----------+-----------+------------+-------------+-----------------+------------------+------------------+-------------------+-------------+--------------+-------------+--------------+------------+-------------
      perfadm               |           0 |        17250 |        0 |         0 |          0 |          -1 |               0 |               -1 |                0 |                -1 |           0 |            0 |           0 |            0 |          0 |           0
      usern                 |           0 |        17250 |        0 |        48 |          0 |          -1 |               0 |               -1 |                0 |                -1 |           0 |            0 |           0 |            0 |          0 |           0
      userg                 |          34 |        15525 |    23.53 |        48 |          0 |          -1 |               0 |               -1 |        814955731 |                -1 |     6111952 |      1145864 |      763994 |       143233 |      42678 |        8001
      userg1                |          34 |        13972 |    23.53 |        48 |          0 |          -1 |               0 |               -1 |        814972419 |                -1 |     6111952 |      1145864 |      763994 |       143233 |      42710 |        8007
      (4 rows)

   The I/O resource monitoring fields (**read_kbytes**, **write_kbytes**, **read_counts**, **write_counts**, **read_speed**, and **write_speed**) can be available only when the GUC parameter enable_user_metric_persistent is enabled.

   For details about each column, see :ref:`PG_TOTAL_USER_RESOURCE_INFO <dws_04_0790>`.

-  Query a user's resource quota and real-time resource usage.

   ::

      SELECT * FROM GS_WLM_USER_RESOURCE_INFO('username');

   The query result is as follows:

   .. code-block::

      userid | used_memory | total_memory | used_cpu | total_cpu | used_space | total_space | used_temp_space | total_temp_space | used_spill_space | total_spill_space | read_kbytes | write_kbytes | read_counts | write_counts | read_speed | write_speed
      --------+-------------+--------------+----------+-----------+------------+-------------+-----------------+------------------+------------------+-------------------+-------------+--------------+-------------+--------------+------------+-------------
      16407 |           18 |        1655 |        6 |         19 |          13787176 |          -1 |               0 |               -1 |                0 |                -1 |           0 |            0 |           0 |            0 |          0 |           0
      (1 row)

-  Query all users' resource quotas and historical resource usage.

   ::

      SELECT * FROM GS_WLM_USER_RESOURCE_HISTORY;

   The query result is as follows:

   .. code-block::

      username        |           timestamp           | used_memory | total_memory | used_cpu | total_cpu | used_space | total_space | used_temp_space | total_temp_space | used_spill_space | total_spill_space | read_kbytes | write_kbytes | read_counts | write_counts | read_speed  | write_speed
      -----------------------+-------------------------------+-------------+--------------+----------+-----------+------------+-------------+-----------------+------------------+------------------+-------------------+-------------+--------------+-------------+--------------+-------------+-------------
      usern                 | 2020-01-08 22:56:06.456855+08 |           0 |        17250 |        0 |        48 |          0 |          -1 |               0 |               -1 |         88349078 |                -1 |       45680 |           34 |        5710 |            8 |         320 |           0
      userg                 | 2020-01-08 22:56:06.458659+08 |           0 |        15525 |    33.48 |        48 |          0 |          -1 |               0 |               -1 |        110169581 |                -1 |       17648 |           23 |        2206 |            5 |         123 |           0
      userg1                | 2020-01-08 22:56:06.460252+08 |           0 |        13972 |    33.48 |        48 |          0 |          -1 |               0 |               -1 |        136106277 |                -1 |       17648 |           23 |        2206 |            5 |         123 |           0

   For the system catalog in :ref:`GS_WLM_USER_RESOURCE_HISTORY <dws_04_0567>`, data in the :ref:`PG_TOTAL_USER_RESOURCE_INFO <dws_04_0790>` view is periodically saved to historical tables only when the GUC parameter enable_user_metric_persistent is enabled.

   For details about each column, see :ref:`GS_WLM_USER_RESOURCE_HISTORY <dws_04_0567>`.

-  Query the real-time I/O control resource usage of a specific user. (The value in the view indicates the I/O control resource usage instead of the actual I/O read/write data.)

   ::

      SELECT * FROM pg_user_iostat('username');

   The query result is as follows:

   .. code-block::

       userid | min_curr_iops | max_curr_iops | min_peak_iops | max_peak_iops | io_limits | io_priority
       -------+---------------+---------------+---------------+---------------+-----------+-------------
           10 |             0 |             0 |             0 |             0 |         0 | None
      (1 row)
