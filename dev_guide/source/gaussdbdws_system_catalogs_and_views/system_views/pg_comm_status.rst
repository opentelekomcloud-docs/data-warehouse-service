:original_name: dws_04_0723.html

.. _dws_04_0723:

PG_COMM_STATUS
==============

**PG_COMM_STATUS** displays the communication library status for a single DN.

.. table:: **Table 1** PG_COMM_STATUS columns

   +----------------+---------+----------------------------------------------------------------------------+
   | Name           | Type    | Description                                                                |
   +================+=========+============================================================================+
   | node_name      | text    | Specifies the node name.                                                   |
   +----------------+---------+----------------------------------------------------------------------------+
   | rxpck/s        | integer | Receiving rate of the communication library on a node. The unit is byte/s. |
   +----------------+---------+----------------------------------------------------------------------------+
   | txpck/s        | integer | Sending rate of the communication library on a node. The unit is byte/s.   |
   +----------------+---------+----------------------------------------------------------------------------+
   | rxkB/s         | bigint  | Receiving rate of the communication library on a node. The unit is KB/s.   |
   +----------------+---------+----------------------------------------------------------------------------+
   | txkB/s         | bigint  | Sending rate of the communication library on a node. The unit is KB/s.     |
   +----------------+---------+----------------------------------------------------------------------------+
   | buffer         | bigint  | Size of the buffer of the Cmailbox.                                        |
   +----------------+---------+----------------------------------------------------------------------------+
   | memKB(libcomm) | bigint  | Communication memory size of the **libcomm** process, in KB.               |
   +----------------+---------+----------------------------------------------------------------------------+
   | memKB(libpq)   | bigint  | Communication memory size of the **libpq** process, in KB.                 |
   +----------------+---------+----------------------------------------------------------------------------+
   | %USED(PM)      | integer | Real-time usage of the postmaster thread.                                  |
   +----------------+---------+----------------------------------------------------------------------------+
   | %USED (sflow)  | integer | Real-time usage of the **gs_sender_flow_controller** thread.               |
   +----------------+---------+----------------------------------------------------------------------------+
   | %USED (rflow)  | integer | Real-time usage of the **gs_receiver_flow_controller** thread.             |
   +----------------+---------+----------------------------------------------------------------------------+
   | %USED (rloop)  | integer | Highest real-time usage among multiple **gs_receivers_loop** threads.      |
   +----------------+---------+----------------------------------------------------------------------------+
   | stream         | integer | Total number of used logical connections.                                  |
   +----------------+---------+----------------------------------------------------------------------------+
