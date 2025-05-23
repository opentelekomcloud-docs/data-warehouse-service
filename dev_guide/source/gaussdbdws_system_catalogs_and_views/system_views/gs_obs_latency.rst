:original_name: dws_04_1203.html

.. _dws_04_1203:

GS_OBS_LATENCY
==============

**GS_OBS_LATENCY** records the average latency of OBS during the 10 minutes before **logtime**. The latency is estimated based on OBS operations. This view is supported only by clusters of version 8.2.0 or later.

.. table:: **Table 1** GS_OBS_LATENCY columns

   +------------+--------------------------+--------------------------------------------------------------------------------+
   | Column     | Type                     | Description                                                                    |
   +============+==========================+================================================================================+
   | nodename   | Text                     | Node                                                                           |
   +------------+--------------------------+--------------------------------------------------------------------------------+
   | hostname   | Text                     | Server node.                                                                   |
   +------------+--------------------------+--------------------------------------------------------------------------------+
   | latency_ms | Double precision         | Average delay of OBS during the 10 minutes before **logtime**. The unit is ms. |
   +------------+--------------------------+--------------------------------------------------------------------------------+
   | reqcount   | Bigint                   | Number of OBS requests during the 10 minutes before **logtime**.               |
   +------------+--------------------------+--------------------------------------------------------------------------------+
   | logtime    | Timestamp with time zone | Time when the delay information is recorded.                                   |
   +------------+--------------------------+--------------------------------------------------------------------------------+
