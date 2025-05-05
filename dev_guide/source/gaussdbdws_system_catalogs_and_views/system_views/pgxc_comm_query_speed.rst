:original_name: dws_04_0979.html

.. _dws_04_0979:

PGXC_COMM_QUERY_SPEED
=====================

**PGXC_COMM_QUERY_SPEED** displays traffic information about all queries on all nodes.

.. table:: **Table 1** PGXC_COMM_QUERY_SPEED columns

   ========= ====== ====================================================
   Column    Type   Description
   ========= ====== ====================================================
   node_name Text   Node name
   query_id  Bigint **debug_query_id** corresponding to the stream
   rxkB/s    Bigint Receiving rate of the query stream (unit: byte/s)
   txkB/s    Bigint Sending rate of the query stream (unit: byte/s)
   rxkB      Bigint Total received data of the query stream (unit: byte)
   txkB      Bigint Total sent data of the query stream (unit: byte)
   rxpck/s   Bigint Packet receiving rate of the query (unit: packets/s)
   txpck/s   Bigint Packet sending rate of the query (Unit: packets/s)
   rxpck     Bigint Total number of received packets of the query
   txpck     Bigint Total number of sent packets of the query
   ========= ====== ====================================================
