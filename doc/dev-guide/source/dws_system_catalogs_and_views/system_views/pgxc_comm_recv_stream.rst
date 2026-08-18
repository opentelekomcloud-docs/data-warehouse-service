:original_name: dws_04_0799.html

.. _dws_04_0799:

PGXC_COMM_RECV_STREAM
=====================

**PG_COMM_RECV_STREAM** displays the receiving stream status of the communication libraries for all the DNs.

.. table:: **Table 1** PGXC_COMM_RECV_STREAM columns

   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | Column                | Type                  | Description                                                        |
   +=======================+=======================+====================================================================+
   | node_name             | Text                  | Node name                                                          |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | local_tid             | Bigint                | ID of the thread using this stream                                 |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | remote_name           | Text                  | Name of the peer node                                              |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | remote_tid            | Bigint                | Peer thread ID                                                     |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | idx                   | Integer               | Peer DN ID in the local DN                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | sid                   | Integer               | Stream ID in the physical connection                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | tcp_sock              | Integer               | TCP socket used in the stream                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | state                 | Text                  | Current status of the stream                                       |
   |                       |                       |                                                                    |
   |                       |                       | -  **UNKNOWN**: The logical connection is unknown.                 |
   |                       |                       | -  **READY**: The logical connection is ready.                     |
   |                       |                       | -  **RUN**: The logical connection receives packets normally.      |
   |                       |                       | -  **HOLD**: The logical connection is waiting to receive packets. |
   |                       |                       | -  **CLOSED**: The logical connection is closed.                   |
   |                       |                       | -  **TO_CLOSED**: The logical connection is to be closed.          |
   |                       |                       | -  **WRITING**: Data is being written.                             |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | query_id              | Bigint                | **debug_query_id** corresponding to the stream                     |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | pn_id                 | Integer               | **plan_node_id** of the query executed by the stream               |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | send_smp              | Integer               | **smpid** of the sender of the query executed by the stream        |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | recv_smp              | Integer               | **smpid** of the receiver of the query executed by the stream      |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | recv_bytes            | Bigint                | Total data volume received from the stream. The unit is byte.      |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | time                  | Bigint                | Current life cycle service duration of the stream. The unit is ms. |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | speed                 | Bigint                | Average receiving rate of the stream. The unit is byte/s.          |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | quota                 | Bigint                | Current communication quota value of the stream. The unit is Byte. |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
   | buff_usize            | Bigint                | Current size of the data cache of the stream. The unit is byte.    |
   +-----------------------+-----------------------+--------------------------------------------------------------------+
