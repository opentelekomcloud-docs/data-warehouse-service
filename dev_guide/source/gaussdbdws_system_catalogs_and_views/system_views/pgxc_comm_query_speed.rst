:original_name: dws_04_0979.html

.. _dws_04_0979:

PGXC_COMM_QUERY_SPEED
=====================

**PGXC_COMM_QUERY_SPEED** displays traffic information about all queries on all nodes.

.. table:: **Table 1** PGXC_COMM_QUERY_SPEED columns

   ========= ====== ====================================================
   Name      Type   Description
   ========= ====== ====================================================
   node_name text   Node name
   query_id  bigint **debug_query_id** corresponding to the stream
   rxkB/s    bigint Receiving rate of the query stream (unit: byte/s)
   txkB/s    bigint Sending rate of the query stream (unit: byte/s)
   rxkB      bigint Total received data of the query stream (unit: byte)
   txkB      bigint Total sent data of the query stream (unit: byte)
   rxpck/s   bigint Packet receiving rate of the query (unit: packets/s)
   txpck/s   bigint Packet sending rate of the query (Unit: packets/s)
   rxpck     bigint Total number of received packets of the query
   txpck     bigint Total number of sent packets of the query
   ========= ====== ====================================================
