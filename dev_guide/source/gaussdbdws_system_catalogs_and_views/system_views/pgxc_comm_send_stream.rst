:original_name: dws_04_0800.html

.. _dws_04_0800:

PGXC_COMM_SEND_STREAM
=====================

**PGXC_COMM_SEND_STREAM** displays the sending stream status of the communication libraries for all the DNs.

.. table:: **Table 1** PGXC_COMM_SEND_STREAM columns

   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | Column                | Type                  | Description                                                                 |
   +=======================+=======================+=============================================================================+
   | node_name             | Text                  | Node name                                                                   |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | local_tid             | Bigint                | ID of the thread using this stream                                          |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | remote_name           | Text                  | Name of the peer node                                                       |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | remote_tid            | Bigint                | Peer thread ID                                                              |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | idx                   | Integer               | Peer DN ID in the local DN                                                  |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | sid                   | Integer               | Stream ID in the physical connection                                        |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | tcp_sock              | Integer               | TCP socket used in the stream                                               |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | state                 | Text                  | Current status of the stream.                                               |
   |                       |                       |                                                                             |
   |                       |                       | -  **UNKNOWN**: The logical connection is unknown.                          |
   |                       |                       | -  **READY**: The logical connection is ready.                              |
   |                       |                       | -  **RUN**: The logical connection sends packets normally.                  |
   |                       |                       | -  **HOLD**: The logical connection is waiting to send packets.             |
   |                       |                       | -  **CLOSED**: The logical connection is closed.                            |
   |                       |                       | -  **TO_CLOSED**: The logical connection is to be closed.                   |
   |                       |                       | -  **WRITING**: Data is being written.                                      |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | query_id              | Bigint                | **debug_query_id** corresponding to the stream                              |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | pn_id                 | Integer               | **plan_node_id** of the query executed by the stream                        |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | send_smp              | Integer               | **smpid** of the sender of the query executed by the stream                 |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | recv_smp              | Integer               | **smpid** of the receiver of the query executed by the stream               |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | send_bytes            | Bigint                | Total data volume sent by the stream. The unit is Byte.                     |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | time                  | Bigint                | Current life cycle service duration of the stream. The unit is ms.          |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | speed                 | Bigint                | Average sending rate of the stream. The unit is Byte/s.                     |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | quota                 | Bigint                | Current communication quota value of the stream. The unit is Byte.          |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
   | wait_quota            | Bigint                | Extra time generated when the stream waits the quota value. The unit is ms. |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------+
