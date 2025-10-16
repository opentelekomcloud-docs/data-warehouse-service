:original_name: dws_04_0978.html

.. _dws_04_0978:

PG_COMM_QUERY_SPEED
===================

**PG_COMM_QUERY_SPEED** displays traffic information about all queries on a single node.

.. table:: **Table 1** PG_COMM_QUERY_SPEED columns

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
   txpck/s   Bigint Packet sending rate of the query (unit: packets/s)
   rxpck     Bigint Total number of received packets of the query
   txpck     Bigint Total number of sent packets of the query
   ========= ====== ====================================================
