:original_name: dws_04_0396.html

.. _dws_04_0396:

Instance Resource Monitoring
============================

GaussDB(DWS) provides system catalogs for monitoring the resource usage of CNs and DNs (including memory, CPU usage, disk I/O, process physical I/O, and process logical I/O), and system catalogs for monitoring the resource usage of the entire cluster.

For details about the system catalog **GS_WLM_INSTANCE_HISTORY**, see :ref:`GS_WLM_INSTANCE_HISTORY <dws_04_0564>`.

.. note::

   Data in the system catalog\ **GS_WLM_INSTANCE_HISTORY** is distributed in corresponding instances. CN monitoring data is stored in the CN instance, and DN monitoring data is stored in the DN instance. The DN has a standby node. When the primary DN is abnormal, the monitoring data of the DN can be restored from the standby node. However, a CN has no standby node. When a CN is abnormal and then restored, the monitoring data of the CN will be lost.

Procedure
---------

-  Query the latest resource usage of the current instance.

   ::

      SELECT * FROM GS_WLM_INSTANCE_HISTORY ORDER BY TIMESTAMP DESC;

   The query result is as follows:

   .. code-block::

      instancename |           timestamp           | used_cpu | free_mem | used_mem | io_await | io_util  | disk_read | disk_write | process_read | process_write | logical_read | logical_write | read_counts | write_counts
      --------------+-------------------------------+----------+----------+----------+----------+----------+-----------+------------+--------------+---------------+--------------+---------------+-------------+--------------
      dn_6015_6016 | 2022-01-10 17:29:17.329495+08 |        0 |    14570 |     8982 |  662.923 |  99.9601 |    697666 |    93655.5 |       183104 |         30082 |       285659 |         30079 |      357717 |        37667
      dn_6015_6016 | 2022-01-10 17:29:07.312049+08 |        0 |    14578 |     8974 |  883.102 |  99.9801 |    756228 |    81417.4 |       189722 |         30786 |       285681 |         30780 |      358103 |        38584
      dn_6015_6016 | 2022-01-10 17:28:57.284472+08 |        0 |    14583 |     8969 |  727.135 |  99.9801 |    648581 |    88799.6 |       177120 |         31176 |       252161 |         31175 |      316085 |        39079
      dn_6015_6016 | 2022-01-10 17:28:47.256613+08 |        0 |    14591 |     8961 |  679.534 |   100.08 |    655360 |     169962 |       179404 |         30424 |       242002 |         30422 |      303351 |        38136

-  Query the resource usage of the current instance during a specified period.

   ::

      SELECT * FROM GS_WLM_INSTANCE_HISTORY WHERE TIMESTAMP > '2022-01-10' AND TIMESTAMP < '2020-01-11' ORDER BY TIMESTAMP DESC;

   The query result is as follows:

   .. code-block::

      instancename |           timestamp           | used_cpu | free_mem | used_mem | io_await | io_util  | disk_read | disk_write | process_read | process_write | logical_read | logical_write | read_counts | write_counts
      --------------+-------------------------------+----------+----------+----------+----------+----------+-----------+------------+--------------+---------------+--------------+---------------+-------------+--------------
      dn_6015_6016 | 2022-01-10 17:29:17.329495+08 |        0 |    14570 |     8982 |  662.923 |  99.9601 |    697666 |    93655.5 |       183104 |         30082 |       285659 |         30079 |      357717 |        37667
      dn_6015_6016 | 2022-01-10 17:29:07.312049+08 |        0 |    14578 |     8974 |  883.102 |  99.9801 |    756228 |    81417.4 |       189722 |         30786 |       285681 |         30780 |      358103 |        38584
      dn_6015_6016 | 2022-01-10 17:28:57.284472+08 |        0 |    14583 |     8969 |  727.135 |  99.9801 |    648581 |    88799.6 |       177120 |         31176 |       252161 |         31175 |      316085 |        39079
      dn_6015_6016 | 2022-01-10 17:28:47.256613+08 |        0 |    14591 |     8961 |  679.534 |   100.08 |    655360 |     169962 |       179404 |         30424 |       242002 |         30422 |      303351 |        38136

-  To query the latest resource usage of a cluster, you can invoke the **pgxc_get_wlm_current_instance_info** stored procedure on the CN.

   ::

      SELECT * FROM pgxc_get_wlm_current_instance_info('ALL');

   The query result is as follows:

   .. code-block::

      instancename |           timestamp           | used_cpu | free_mem | used_mem | io_await | io_util | disk_read | disk_write | process_read | process_write | logical_read | logical_write | read_counts | write_counts
      --------------+-------------------------------+----------+----------+----------+----------+---------+-----------+------------+--------------+---------------+--------------+---------------+-------------+--------------
      coordinator2 | 2020-01-14 21:58:29.290894+08 |        0 |    12010 |      278 |  16.0445 | 7.19561 |   184.431 |    27959.3 |            0 |            10 |            0 |             0 |           0 |            0
      coordinator3 | 2020-01-14 21:58:27.567655+08 |        0 |    12000 |      288 |  .964557 | 3.40659 |   332.468 |    3375.02 |           26 |            13 |            0 |             0 |           0 |            0
      datanode1    | 2020-01-14 21:58:23.900321+08 |        0 |    11899 |      389 |  1.17296 |    3.25 |     329.6 |     2870.4 |           28 |             8 |           13 |             3 |          18 |            6
      datanode2    | 2020-01-14 21:58:32.832989+08 |        0 |    11904 |      384 |   17.948 | 8.52148 |   214.186 |    25894.1 |           28 |            10 |           13 |             3 |          18 |            6
      datanode3    | 2020-01-14 21:58:24.826694+08 |        0 |    11894 |      394 |  1.16088 |    3.15 |       328 |     2868.8 |           25 |            10 |           13 |             3 |          18 |            6
      coordinator1 | 2020-01-14 21:58:33.367649+08 |        0 |    11988 |      300 |  9.53286 |   10.05 |      43.2 |      55232 |            0 |             0 |            0 |             0 |           0 |            0
      coordinator1 | 2020-01-14 21:58:23.216645+08 |        0 |    11988 |      300 |  1.17085 | 3.21182 |   324.729 |    2831.13 |            8 |            13 |            0 |             0 |           0 |            0
      (7 rows)

-  To query historical resource usage of a cluster, you can invoke the **pgxc_get_wlm_current_instance_info** stored procedure on the CN.

   ::

      SELECT * FROM pgxc_get_wlm_history_instance_info('ALL', '2020-01-14 21:00:00', '2020-01-14 22:00:00', 3);

   The query result is as follows:

   .. code-block::

      instancename |           timestamp           | used_cpu | free_mem | used_mem | io_await |  io_util  | disk_read | disk_write | process_read | process_write | logical_read | logical_write | read_counts | write_counts
      --------------+-------------------------------+----------+----------+----------+----------+-----------+-----------+------------+--------------+---------------+--------------+---------------+-------------+--------------
      coordinator2 | 2020-01-14 21:50:49.778902+08 |        0 |    12020 |      268 |  .127371 |   .789211 |    15.984 |    3994.41 |            0 |             0 |            0 |             0 |           0 |            0
      coordinator2 | 2020-01-14 21:53:49.043646+08 |        0 |    12018 |      270 |  30.2902 |   8.65404 |    276.77 |    16741.8 |            3 |             1 |            0 |             0 |           0 |            0
      coordinator2 | 2020-01-14 21:57:09.202654+08 |        0 |    12018 |      270 |   .16051 |   .979021 |   59.9401 |       5596 |            0 |             0 |            0 |             0 |           0 |            0
      coordinator3 | 2020-01-14 21:38:48.948646+08 |        0 |    12012 |      276 | .0769231 | .00999001 |         0 |    35.1648 |            0 |             1 |            0 |             0 |           0 |            0
      coordinator3 | 2020-01-14 21:40:29.061178+08 |        0 |    12012 |      276 |  .118421 |  .0199601 |         0 |    970.858 |            0 |             0 |            0 |             0 |           0 |            0
      coordinator3 | 2020-01-14 21:50:19.612777+08 |        0 |    12010 |      278 |   24.411 |   11.7665 |   8.78244 |    44641.1 |            0 |             0 |            0 |             0 |           0 |            0
      datanode1    | 2020-01-14 21:49:42.758649+08 |        0 |    11909 |      379 |  .798776 |      8.02 |      51.2 |    20924.8 |            0 |             0 |            0 |             0 |           0 |            0
      datanode1    | 2020-01-14 21:49:52.760188+08 |        0 |    11909 |      379 |  23.8972 |      14.1 |         0 |      74760 |            0 |             0 |            0 |             0 |           0 |            0
      datanode1    | 2020-01-14 21:50:22.769226+08 |        0 |    11909 |      379 |  39.5868 |       7.4 |         0 |    19760.8 |            0 |             0 |            0 |             0 |           0 |            0
      datanode2    | 2020-01-14 21:58:02.826185+08 |        0 |    11905 |      383 |  .351648 |       .32 |      20.8 |      504.8 |            0 |             0 |            0 |             0 |           0 |            0
      datanode2    | 2020-01-14 21:56:42.80793+08  |        0 |    11906 |      382 |  .559748 |       .04 |         0 |      326.4 |            0 |             0 |            0 |             0 |           0 |            0
      datanode2    | 2020-01-14 21:45:21.632407+08 |        0 |    11901 |      387 |  12.1313 |   4.55544 |    3.1968 |    45177.2 |            0 |             0 |            0 |             0 |           0 |            0
      datanode3    | 2020-01-14 21:58:14.823317+08 |        0 |    11898 |      390 |  .378205 |       .99 |        48 |    23353.6 |            0 |             0 |            0 |             0 |           0 |            0
      datanode3    | 2020-01-14 21:47:50.665028+08 |        0 |    11901 |      387 |  1.07494 |      1.19 |         0 |    15506.4 |            0 |             0 |            0 |             0 |           0 |            0
      datanode3    | 2020-01-14 21:51:21.720117+08 |        0 |    11903 |      385 |  10.2795 |      3.11 |         0 |    11031.2 |            0 |             0 |            0 |             0 |           0 |            0
      coordinator1 | 2020-01-14 21:42:59.121945+08 |        0 |    12020 |      268 | .0857143 |  .0699301 |         0 |    6579.02 |            0 |             0 |            0 |             0 |           0 |            0
      coordinator1 | 2020-01-14 21:41:49.042646+08 |        0 |    12020 |      268 |  20.9039 |   11.3786 |   6042.76 |    57903.7 |            0 |             0 |            0 |             0 |           0 |            0
      coordinator1 | 2020-01-14 21:41:09.007652+08 |        0 |    12020 |      268 | .0446429 |    .03996 |         0 |    1109.29 |            0 |             0 |            0 |             0 |           0 |            0
      (18 rows)
