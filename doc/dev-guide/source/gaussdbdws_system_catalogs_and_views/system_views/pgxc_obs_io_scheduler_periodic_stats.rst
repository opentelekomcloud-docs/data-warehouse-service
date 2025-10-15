:original_name: dws_04_1236.html

.. _dws_04_1236:

PGXC_OBS_IO_SCHEDULER_PERIODIC_STATS
====================================

**PGXC_OBS_IO_SCHEDULER_PERIODIC_STATS** provides statistics on the number of requests and flow control information for different types of OBS I/O Scheduler requests, including read, write, and file operations. This system view is supported only by clusters of version 9.1.0 or later.

The first query result shows the statistics from the cluster startup to the query time, with detailed columns listed in the table below.

.. table:: **Table 1** PGXC_OBS_IO_SCHEDULER_PERIODIC_STATS columns

   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | Column                   | Type                  | Description                                                       |
   +==========================+=======================+===================================================================+
   | node_name                | Name                  | Name of a CN or DN, for example, **dn_6001_6002**.                |
   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | io_type                  | Char                  | Type of I/O operation, including:                                 |
   |                          |                       |                                                                   |
   |                          |                       | -  **R**: read                                                    |
   |                          |                       | -  **W**: write                                                   |
   |                          |                       | -  **S**: file operation                                          |
   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | recent_throttled_req_num | Int                   | Number of times flow control was applied between two query views. |
   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | total_throttled_req_num  | Int                   | Total number of times flow control was applied.                   |
   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | last_throttled_dur(s)    | INT8                  | Time interval since the last occurrence of flow control.          |
   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | waiting_req_num          | Int                   | Number of queued requests currently waiting.                      |
   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | mean_tps                 | numeric(7,2)          | Average TPS (transactions per second) between two query views.    |
   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | mean_req_size(KB)        | INT8                  | Average length of requests between two query views, in KB.        |
   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | mean_req_latency(ms)     | INT8                  | Average latency of requests between two query views, in ms.       |
   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | max_req_latency(ms)      | INT8                  | Maximum latency of requests before two query views, in ms.        |
   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | mean_bps(KB/s)           | INT8                  | Average read or write speed between two query views, in KB/s.     |
   +--------------------------+-----------------------+-------------------------------------------------------------------+
   | duration(s)              | Int                   | Time interval between two query views, in seconds.                |
   +--------------------------+-----------------------+-------------------------------------------------------------------+

Example
-------

Run the **SELECT \* FROM pgxc_obs_io_scheduler_periodic_stats** statement to query the view content. The following is an example of the query result:

.. code-block::

   SELECT * FROM pgxc_obs_io_scheduler_periodic_stats;

     node_name   | io_type | recent_throttled_req_num | total_throttled_req_num | last_throttled_dur(s) | waiting_req_num | mean_tps | mean_req_size(KB) | mean_req_latency(ms) | max_req_latency(ms) | mean_bps(KB/s) | duration(s)
   --------------+---------+--------------------------+-------------------------+-----------------------+-----------------+----------+-------------------+----------------------+---------------------+----------------+-------------
    dn_6001_6002 | S       |                        0 |                       0 |                     0 |               0 |     0.00 |                 0 |                    0 |                   0 |              0 |         155
    dn_6001_6002 | R       |                        0 |                       0 |                     0 |               0 |     0.00 |                 0 |                    0 |                   0 |              0 |         155
    dn_6001_6002 | W       |                        0 |                       0 |                     0 |               0 |     0.00 |                 0 |                    0 |                   0 |              0 |         155
    cn_5001      | S       |                        0 |                       0 |                     0 |               0 |      .03 |                 0 |                  207 |                 519 |              0 |         155
    cn_5001      | R       |                        0 |                       0 |                     0 |               0 |     0.00 |                 0 |                    0 |                   0 |              0 |         155
    cn_5001      | W       |                        0 |                       0 |                     0 |               0 |      .01 |                 0 |                  288 |                 288 |              0 |         155
   (6 rows)

To display **0** before the decimal point in the value of **mean_tps**, set the **display_leading_zero** option in the **behavior_compat_options** parameter.

Run the **select \* from pgxc_obs_io_scheduler_periodic_stats** statement. The following information is displayed:

.. code-block::

   SELECT * FROM pgxc_obs_io_scheduler_periodic_stats;

     node_name   | io_type | recent_throttled_req_num | total_throttled_req_num | last_throttled_dur(s) | waiting_req_num | mean_tps | mean_req_size(KB) | mean_req_latency(ms) | max_req_latency(ms) | mean_bps(KB/s) | duration(s)
   --------------+---------+--------------------------+-------------------------+-----------------------+-----------------+----------+-------------------+----------------------+---------------------+----------------+-------------
    dn_6001_6002 | S       |                        0 |                       0 |                     0 |               0 |     0.36 |                 0 |                  132 |                 326 |              0 |         177
    dn_6001_6002 | R       |                        0 |                       0 |                     0 |               0 |     0.00 |                 0 |                    0 |                   0 |              0 |         177
    dn_6001_6002 | W       |                        0 |                       0 |                     0 |               0 |     0.00 |                 0 |                    0 |                   0 |              0 |         177
    cn_5001      | S       |                        0 |                       0 |                     0 |               0 |     0.00 |                 0 |                    0 |                   0 |              0 |         177
    cn_5001      | R       |                        0 |                       0 |                     0 |               0 |     0.00 |                 0 |                    0 |                   0 |              0 |         177
    cn_5001      | W       |                        0 |                       0 |                     0 |               0 |     0.00 |                 0 |                    0 |                   0 |              0 |         177
