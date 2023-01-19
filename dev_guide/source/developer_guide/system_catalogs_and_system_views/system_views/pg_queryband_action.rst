:original_name: dws_04_0743.html

.. _dws_04_0743:

PG_QUERYBAND_ACTION
===================

**PG_QUERYBAND_ACTION** displays information about the object associated with **query_band** and the **query_band** query order.

.. table:: **Table 1** PG_QUERYBAND_ACTION columns

   +------------+---------+----------------------------------------------------------+
   | Name       | Type    | Description                                              |
   +============+=========+==========================================================+
   | qband      | text    | **query_band** key-value pairs                           |
   +------------+---------+----------------------------------------------------------+
   | respool_id | oid     | OID of the resource pool associated with **query_band**  |
   +------------+---------+----------------------------------------------------------+
   | respool    | text    | Name of the resource pool associated with **query_band** |
   +------------+---------+----------------------------------------------------------+
   | priority   | text    | Intra-queue priority associated with **query_band**      |
   +------------+---------+----------------------------------------------------------+
   | qborder    | integer | **query_band** query order                               |
   +------------+---------+----------------------------------------------------------+
