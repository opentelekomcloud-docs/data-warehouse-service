:original_name: dws_04_0394.html

.. _dws_04_0394:

User Resource Monitoring
========================

In the multi-tenant management framework, you can query the real-time usage of all user resources (including the memory, number of CPU cores, storage space, temporary space, operator spilling space, and I/Os) in real time through the system views :ref:`PG_TOTAL_USER_RESOURCE_INFO <dws_04_0790>` and :ref:`PGXC_TOTAL_USER_RESOURCE_INFO <dws_04_1020>` and the function **GS_WLM_USER_RESOURCE_INFO**. You can also query the system catalog :ref:`GS_WLM_USER_RESOURCE_HISTORY <dws_04_0567>` and system view :ref:`PGXC_WLM_USER_RESOURCE_HISTORY <dws_04_1021>` for the historical usage of user resources.

Precautions
-----------

-  The CPU, I/O, and memory usage of all jobs on fast and slow lanes (simple jobs on fast lanes and complex jobs on slow lanes) can be monitored.
-  Currently, fast lane jobs have no memory or CPU limits. They may use too many resources and go over the resource limit.
-  In the DN monitoring view, I/O, memory, and CPU display the resource usage and limits of resource pools.
-  In the CN monitoring view, I/O, memory, and CPU display the total resource usage and limit of all DN resource pools in the cluster.
-  The DN monitoring information is updated every 5 seconds. CNs collect monitoring information from DNs every 5 seconds. Because each instance updates or collects user monitoring information independently, the monitoring information update time on each instance may be different.
-  The auxiliary thread automatically invokes the persistence function every 30 seconds to make user monitoring data persistent. So, normally, you don't have to do this.
-  When there are a large number of users and a large cluster, querying such real-time views will cause network latency due to the real-time communication overhead between CNs and DNs.
-  Resources are not monitored for an initial administrator.

Procedure
---------

-  Query all users' resource quotas and real-time resource usage.

   ::

      SELECT * FROM PG_TOTAL_USER_RESOURCE_INFO;

   The result view is as follows:

   ::

      username        | used_memory | total_memory | used_cpu | total_cpu | used_space | total_space | used_temp_space | total_temp_space | used_spill_space | total_spill_space | read_kbytes | write_kbytes | read_counts | write_counts | read_speed | write_speed | send_speed | recv_speed
      -----------------------+-------------+--------------+----------+-----------+------------+-------------+-----------------+------------------+------------------+-------------------+-------------+--------------+-------------+--------------+------------+-------------+------------+------------
      perfadm               |           0 |            0 |        0 |         0 |          0 |          -1 |               0 |               -1 |                0 |                -1 |           0 |            0 |           0 |            0 |          0 |           0 |          0 |          0
      usern                 |           0 |        17250 |        0 |        48 |          0 |          -1 |               0 |               -1 |                0 |                -1 |           0 |            0 |           0 |            0 |          0 |           0 |          0 |          0
      (2 rows)

   The I/O resource monitoring fields (**read_kbytes**, **write_kbytes**, **read_counts**, **write_counts**, **read_speed**, and **write_speed**) can be available only when the GUC parameter described in :ref:`enable_user_metric_persistent <en-us_topic_0000001510522653__section827402723813>` is enabled.

   For details about each column, see :ref:`PG_TOTAL_USER_RESOURCE_INFO <dws_04_0790>`.

-  Query a user's resource quota and real-time resource usage.

   ::

      SELECT * FROM GS_WLM_USER_RESOURCE_INFO('username');

   The query result is as follows:

   ::

      userid | used_memory | total_memory | used_cpu | total_cpu | used_space | total_space | used_temp_space | total_temp_space | used_spill_space | total_spill_space | read_kbytes | write_kbytes | read_counts | write_counts | read_speed | write_speed | send_speed | recv_speed
      --------+-------------+--------------+----------+-----------+------------+-------------+-----------------+------------------+------------------+-------------------+-------------+--------------+-------------+--------------+------------+-------------+------------+------------
      16407 |           18 |        1655 |        6 |         19 |          13787176 |          -1 |               0 |               -1 |                0 |                -1 |           0 |            0 |           0 |            0 |          0 |           0 |          0 |          0
      (1 row)

-  Query all users' resource quotas and historical resource usage.

   ::

      SELECT * FROM GS_WLM_USER_RESOURCE_HISTORY;

   The query result is as follows:

   ::

      username        |           timestamp           | used_memory | total_memory | used_cpu | total_cpu | used_space | total_space | used_temp_space | total_temp_space | used_spill_space | total_spill_space | read_kbytes | write_kbytes | read_counts | write_counts | read_speed  | write_speed | send_speed | recv_speed
      -----------------------+-------------------------------+-------------+--------------+----------+-----------+------------+-------------+-----------------+------------------+------------------+-------------------+-------------+--------------+-------------+--------------+-------------+-------------+------------+------------
      usern                 | 2020-01-08 22:56:06.456855+08 |           0 |        17250 |        0 |        48 |          0 |          -1 |               0 |               -1 |         88349078 |                -1 |       45680 |           34 |        5710 |            8 |         320 |           0 |          0 |          0
      userg                 | 2020-01-08 22:56:06.458659+08 |           0 |        15525 |    33.48 |        48 |          0 |          -1 |               0 |               -1 |        110169581 |                -1 |       17648 |           23 |        2206 |            5 |         123 |           0 |          0 |          0
      userg1                | 2020-01-08 22:56:06.460252+08 |           0 |        13972 |    33.48 |        48 |          0 |          -1 |               0 |               -1 |        136106277 |                -1 |       17648 |           23 |        2206 |            5 |         123 |           0 |          0 |          0

   For the system catalog in :ref:`GS_WLM_USER_RESOURCE_HISTORY <dws_04_0567>`, data in the :ref:`PG_TOTAL_USER_RESOURCE_INFO <dws_04_0790>` view is periodically saved to historical tables only when the GUC parameter :ref:`enable_user_metric_persistent <en-us_topic_0000001510522653__section827402723813>` is enabled.

   For details about each column, see :ref:`GS_WLM_USER_RESOURCE_HISTORY <dws_04_0567>`.
