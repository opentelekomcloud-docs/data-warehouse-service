:original_name: dws_04_0723.html

.. _dws_04_0723:

PG_COMM_STATUS
==============

**PG_COMM_STATUS** displays the communication library status for a single DN.

.. table:: **Table 1** PG_COMM_STATUS columns

   +----------------+---------+----------------------------------------------------------------------------+
   | Column         | Type    | Description                                                                |
   +================+=========+============================================================================+
   | node_name      | Text    | Node name.                                                                 |
   +----------------+---------+----------------------------------------------------------------------------+
   | rxpck/s        | Integer | Receiving rate of the communication library on a node. The unit is byte/s. |
   +----------------+---------+----------------------------------------------------------------------------+
   | txpck/s        | Integer | Sending rate of the communication library on a node. The unit is byte/s.   |
   +----------------+---------+----------------------------------------------------------------------------+
   | rxkB/s         | Bigint  | Receiving rate of the communication library on a node. The unit is KB/s.   |
   +----------------+---------+----------------------------------------------------------------------------+
   | txkB/s         | Bigint  | Sending rate of the communication library on a node. The unit is KB/s.     |
   +----------------+---------+----------------------------------------------------------------------------+
   | buffer         | Bigint  | Size of the buffer of the Cmailbox.                                        |
   +----------------+---------+----------------------------------------------------------------------------+
   | memKB(libcomm) | Bigint  | Communication memory size of the **libcomm** process, in KB.               |
   +----------------+---------+----------------------------------------------------------------------------+
   | memKB(libpq)   | Bigint  | Communication memory size of the **libpq** process, in KB.                 |
   +----------------+---------+----------------------------------------------------------------------------+
   | %USED(PM)      | Integer | Real-time usage of the postmaster thread.                                  |
   +----------------+---------+----------------------------------------------------------------------------+
   | %USED (sflow)  | Integer | Real-time usage of the **gs_sender_flow_controller** thread.               |
   +----------------+---------+----------------------------------------------------------------------------+
   | %USED (rflow)  | Integer | Real-time usage of the **gs_receiver_flow_controller** thread.             |
   +----------------+---------+----------------------------------------------------------------------------+
   | %USED (rloop)  | Integer | Highest real-time usage among multiple **gs_receivers_loop** threads.      |
   +----------------+---------+----------------------------------------------------------------------------+
   | stream         | Integer | Total number of used logical connections.                                  |
   +----------------+---------+----------------------------------------------------------------------------+
