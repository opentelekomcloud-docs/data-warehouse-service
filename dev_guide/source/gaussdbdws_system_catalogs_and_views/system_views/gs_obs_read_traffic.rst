:original_name: dws_04_1011.html

.. _dws_04_1011:

GS_OBS_READ_TRAFFIC
===================

Collects statistics on the OBS read traffic and average read bandwidth. The statistical results are aggregated every 10 minutes. This view is supported only by clusters of version 8.2.0 or later.

+--------------------+--------------------------+----------------------------------------------------------------------+
| Column             | Type                     | Description                                                          |
+====================+==========================+======================================================================+
| nodename           | TEXT                     | Cluster node                                                         |
+--------------------+--------------------------+----------------------------------------------------------------------+
| hostname           | TEXT                     | Server node                                                          |
+--------------------+--------------------------+----------------------------------------------------------------------+
| traffic_mb         | float8                   | OBS read traffic statistics during the 10 minutes before **logtime** |
+--------------------+--------------------------+----------------------------------------------------------------------+
| bandwidth_mb_per_s | float8                   | Average bandwidth, in MB/s                                           |
+--------------------+--------------------------+----------------------------------------------------------------------+
| reqcount           | Bigint                   | Number of OBS reads during the 10 minutes before **logtime**         |
+--------------------+--------------------------+----------------------------------------------------------------------+
| logtime            | Timestamp with time zone | Time when statistics are recorded                                    |
+--------------------+--------------------------+----------------------------------------------------------------------+

Examples
--------

Query statistics on the OBS read traffic and average read bandwidth. The statistical results are aggregated every 10 minutes.

::

   select * from gs_obs_read_traffic;
    nodename |     hostname     |    traffic_mb    | bandwidth_mb_per_s | reqcount |        logtime
   ----------+------------------+------------------+--------------------+----------+------------------------
    dn_1     | rhel_10_90_45_56 | 101.959338188171 |   5.14830159670447 |       23 | 2022-11-26 09:50:00+08
   (1 row)
