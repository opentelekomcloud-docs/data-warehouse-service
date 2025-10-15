:original_name: dws_04_1012.html

.. _dws_04_1012:

GS_OBS_WRITE_TRAFFIC
====================

Collects statistics on the OBS write traffic and average write bandwidth. The statistical results are aggregated every 10 minutes. This view is supported only by clusters of version 8.2.0 or later.

+--------------------+--------------------------+-----------------------------------------------------------------------+
| Column             | Type                     | Description                                                           |
+====================+==========================+=======================================================================+
| nodename           | TEXT                     | Cluster node                                                          |
+--------------------+--------------------------+-----------------------------------------------------------------------+
| hostname           | TEXT                     | Server node                                                           |
+--------------------+--------------------------+-----------------------------------------------------------------------+
| traffic_mb         | float8                   | OBS write traffic statistics during the 10 minutes before **logtime** |
+--------------------+--------------------------+-----------------------------------------------------------------------+
| bandwidth_mb_per_s | float8                   | Average bandwidth, in MB/s                                            |
+--------------------+--------------------------+-----------------------------------------------------------------------+
| reqcount           | Bigint                   | Number of OBS writes during the 10 minutes before **logtime**         |
+--------------------+--------------------------+-----------------------------------------------------------------------+
| logtime            | Timestamp with time zone | Time when statistics are recorded                                     |
+--------------------+--------------------------+-----------------------------------------------------------------------+

Examples
--------

Query statistics on the OBS write traffic and average write bandwidth. The statistical results are aggregated every 10 minutes.

::

   select * from gs_obs_write_traffic;
      nodename   |     hostname     |      traffic_mb      | bandwidth_mb_per_s  | reqcount |        logtime
   --------------+------------------+----------------------+---------------------+----------+------------------------
    dn_1         | rhel_10_90_45_56 |  .000738143920898438 | .000289970820362525 |       12 | 2022-10-24 16:10:00+08
    dn_1         | rhel_10_90_45_56 |  .000354766845703125 | .000386063466694153 |        7 | 2022-10-24 18:50:00+08
    dn_1         | rhel_10_90_45_56 | 9.34600830078125e-05 | .000143659648687162 |        2 | 2022-11-07 09:20:00+08
    dn_1         | rhel_10_90_45_56 | 4.10079956054688e-05 | .000186667253592502 |        1 | 2022-11-07 09:30:00+08
    dn_1         | rhel_10_90_45_56 |     2048.17834663391 |    27.2766632219637 |        2 | 2022-11-22 16:10:00+08
    dn_1         | rhel_10_90_45_56 |     3747.23722648621 |    28.0842938534546 |        4 | 2022-11-22 16:20:00+08
   (6 row)
