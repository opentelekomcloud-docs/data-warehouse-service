:original_name: dws_04_1124.html

.. _dws_04_1124:

PG_MATVIEW_PARTITION
====================

**PG_MATVIEW_PARTITION** records data information about each partition in partitioned materialized views. This system catalog is supported only by clusters of version 9.1.0.200 or later.

.. table:: **Table 1** PG_MATVIEW_PARTITION columns

   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------+
   | Column                | Type                     | Description                                                                                    |
   +=======================+==========================+================================================================================================+
   | matpartid             | OID                      | OID of the materialized view partition.                                                        |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------+
   | active                | Boolean                  | Whether the partition data is the most up-to-date data.                                        |
   |                       |                          |                                                                                                |
   |                       |                          | **t** indicates that the data in the partitioned materialized view is in the latest state.     |
   |                       |                          |                                                                                                |
   |                       |                          | **f** indicates that the data in the partitioned materialized view is not in the latest state. |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------+
   | refresh_start_time    | Timestamp with time zone | Start time of the last refresh.                                                                |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------+
   | refresh_finish_time   | Timestamp with time zone | End time of the last refresh.                                                                  |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------+
