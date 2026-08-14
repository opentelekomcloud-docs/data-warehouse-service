:original_name: dws_04_1235.html

.. _dws_04_1235:

PGXC_OBS_IO_SCHEDULER_STATS
===========================

PGXC_OBS_IO_SCHEDULER_STATS displays the latest real-time statistics about read/write requests of the OBS I/O Scheduler. This system view is supported only by clusters of version 9.1.0 or later.

.. table:: **Table 1** PGXC_OBS_IO_SCHEDULER_STATS columns

   +-----------------------+-----------------------+-------------------------------------------------------+
   | Column                | Type                  | Description                                           |
   +=======================+=======================+=======================================================+
   | node_name             | Text                  | Node name.                                            |
   +-----------------------+-----------------------+-------------------------------------------------------+
   | io_type               | Char                  | Type of I/O operation, including:                     |
   |                       |                       |                                                       |
   |                       |                       | -  **r**: read.                                       |
   |                       |                       | -  **w**: write.                                      |
   |                       |                       | -  **s**: file operation.                             |
   +-----------------------+-----------------------+-------------------------------------------------------+
   | current_bps           | INT8                  | Current bandwidth rate, in KB/s.                      |
   +-----------------------+-----------------------+-------------------------------------------------------+
   | best_bps              | INT8                  | Best bandwidth rate achieved recently, in KB/s.       |
   +-----------------------+-----------------------+-------------------------------------------------------+
   | waiting_request_num   | Int                   | Number of queued requests currently waiting.          |
   +-----------------------+-----------------------+-------------------------------------------------------+
   | mean_request_size     | INT8                  | Average length of requests processed recently, in KB. |
   +-----------------------+-----------------------+-------------------------------------------------------+
   | total_token_num       | Int                   | Total number of I/O tokens.                           |
   +-----------------------+-----------------------+-------------------------------------------------------+
   | available_token_num   | Int                   | Number of available I/O tokens.                       |
   +-----------------------+-----------------------+-------------------------------------------------------+
   | total_worker_num      | Int                   | Total number of working threads.                      |
   +-----------------------+-----------------------+-------------------------------------------------------+
   | idle_worker_num       | Int                   | Number of idle working threads.                       |
   +-----------------------+-----------------------+-------------------------------------------------------+

Example
-------

#. Query statistics about read requests of OBS I/O Scheduler on each node:

   .. code-block::

      SELECT * FROM pgxc_obs_io_scheduler_stats WHERE io_type = 'r' ORDER BY node_name;

        node_name   | io_type | current_bps | best_bps | waiting_request_num | mean_request_size | total_token_num | available_token_num | total_worker_num | idle_worker_num
      --------------+---------+-------------+----------+---------------------+-------------------+-----------------+---------------------+------------------+-----------------
       dn_6001_6002 | r       |       26990 |    26990 |                   0 |               215 |              18 |                  16 |               12 |              10
       dn_6003_6004 | r       |       21475 |    21475 |                  10 |               190 |              30 |                  30 |               20 |              20
       dn_6005_6006 | r       |       12384 |    12384 |                  36 |               133 |              30 |                  27 |               20 |              17

   According to the result, this is a snapshot of the statistics at a certain time point when the current I/O scheduler reads I/Os. At this time, the bandwidth is increasing, and **current_bps** is equal to **best_bps**. Take **dn_6003_6004** as an example. You can see that there are queuing requests on the current DN. The value of **total_token_num** is the same as that of **available_token_num**, indicating that the I/O scheduler has not started to process these requests when the view is queried.

#. Wait for a while and initiate the query again.

   .. code-block::

      SELECT * FROM pgxc_obs_io_scheduler_stats WHERE io_type = 'r' ORDER BY node_name;

        node_name   | io_type | current_bps | best_bps | waiting_request_num | mean_request_size | total_token_num | available_token_num | total_worker_num | idle_worker_num
      --------------+---------+-------------+----------+---------------------+-------------------+-----------------+---------------------+------------------+-----------------
       dn_6001_6002 | r       |       13228 |    26990 |                   0 |               609 |              18 |                  18 |               12 |              12
       dn_6003_6004 | r       |       15717 |    21475 |                   0 |               622 |              30 |                  30 |               20 |              20
       dn_6005_6006 | r       |       18041 |    21767 |                   0 |               609 |              30 |                  30 |               20 |              20

   When the queue is empty and the value of **available_token_num** is equal to that of **total_token_num**, it indicates that the I/O scheduler has finished processing all requests and there are no new requests in line. The **current_bps** value is not **0** because it represents the average bandwidth (in bit/s) over a three-second period. Therefore, the displayed value reflects the data from three seconds ago.

3. After a short period of time, the query result is as follows. The value of **current_bps** changes to **0**.

   .. code-block::

      SELECT * FROM pgxc_obs_io_scheduler_stats WHERE io_type = 'r' ORDER BY node_name;

        node_name   | io_type | current_bps | best_bps | waiting_request_num | mean_request_size | total_token_num | available_token_num | total_worker_num | idle_worker_num
      --------------+---------+-------------+----------+---------------------+-------------------+-----------------+---------------------+------------------+-----------------
       dn_6001_6002 | r       |           0 |    26990 |                   0 |               609 |              18 |                  18 |               12 |              12
       dn_6003_6004 | r       |           0 |    21475 |                   0 |               622 |              30 |                  30 |               20 |              20
       dn_6005_6006 | r       |           0 |    21767 |                   0 |               609 |              30 |                  30 |               20 |              20
