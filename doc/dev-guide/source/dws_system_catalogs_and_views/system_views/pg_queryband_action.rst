:original_name: dws_04_0743.html

.. _dws_04_0743:

PG_QUERYBAND_ACTION
===================

**PG_QUERYBAND_ACTION** displays information about the object associated with **query_band** and the **query_band** query order.

.. table:: **Table 1** PG_QUERYBAND_ACTION columns

   +------------+---------+----------------------------------------------------------+
   | Column     | Type    | Description                                              |
   +============+=========+==========================================================+
   | qband      | Text    | **query_band** key-value pairs                           |
   +------------+---------+----------------------------------------------------------+
   | respool_id | OID     | OID of the resource pool associated with **query_band**  |
   +------------+---------+----------------------------------------------------------+
   | respool    | Text    | Name of the resource pool associated with **query_band** |
   +------------+---------+----------------------------------------------------------+
   | priority   | Text    | Intra-queue priority associated with **query_band**      |
   +------------+---------+----------------------------------------------------------+
   | qborder    | Integer | **query_band** query order                               |
   +------------+---------+----------------------------------------------------------+
