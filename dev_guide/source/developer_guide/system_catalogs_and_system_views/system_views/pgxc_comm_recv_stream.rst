:original_name: dws_04_0799.html

.. _dws_04_0799:

PGXC_COMM_RECV_STREAM
=====================

**PG_COMM_RECV_STREAM** displays the receiving stream status of the communication libraries for all the DNs.

.. table:: **Table 1** PGXC_COMM_RECV_STREAM columns

   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | Name                  | Type                  | Description                                                        |
   +=======================+=======================+====================================================================+
   | node_name             | text                  | Node name                                                          |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | local_tid             | bigint                | ID of the thread using this stream                                 |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | remote_name           | text                  | Name of the peer node                                              |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | remote_tid            | bigint                | Peer thread ID                                                     |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | idx                   | integer               | Peer DN ID in the local DN                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | sid                   | integer               | Stream ID in the physical connection                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | tcp_sock              | integer               | TCP socket used in the stream                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | state                 | text                  | Current status of the stream                                       |
   |                       |                       |                                                                    |
   |                       |                       | -  **UNKNOWN**: The logical connection is unknown.                 |
   |                       |                       | -  **READY**: The logical connection is ready.                     |
   |                       |                       | -  **RUN**: The logical connection receives packets normally.      |
   |                       |                       | -  **HOLD**: The logical connection is waiting to receive packets. |
   |                       |                       | -  **CLOSED**: The logical connection is closed.                   |
   |                       |                       | -  **TO_CLOSED**: The logical connection is to be closed.          |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | query_id              | bigint                | **debug_query_id** corresponding to the stream                     |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | pn_id                 | integer               | **plan_node_id** of the query executed by the stream               |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | send_smp              | integer               | **smpid** of the sender of the query executed by the stream        |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | recv_smp              | integer               | **smpid** of the receiver of the query executed by the stream      |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | recv_bytes            | bigint                | Total data volume received from the stream. The unit is byte.      |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | time                  | bigint                | Current life cycle service duration of the stream. The unit is ms. |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | speed                 | bigint                | Average receiving rate of the stream. The unit is byte/s.          |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | quota                 | bigint                | Current communication quota value of the stream. The unit is Byte. |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | buff_usize            | bigint                | Current size of the data cache of the stream. The unit is byte.    |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
