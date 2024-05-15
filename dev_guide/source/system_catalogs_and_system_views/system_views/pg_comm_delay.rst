:original_name: dws_04_0722.html

.. _dws_04_0722:

PG_COMM_DELAY
=============

**PG_COMM_DELAY** displays the communication library delay status for a single DN.

.. table:: **Table 1** PG_COMM_DELAY columns

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
   | Name                  | Type                  | Description                                                                                |
   +=======================+=======================+============================================================================================+
   | node_name             | text                  | Node name                                                                                  |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
   | remote_name           | text                  | Name of the peer node                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
   | remote_host           | text                  | IP address of the peer                                                                     |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
   | stream_num            | integer               | Number of logical stream connections used by the current physical connection               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
   | min_delay             | integer               | Minimum delay of the current physical connection within 1 minute. Its unit is microsecond. |
   |                       |                       |                                                                                            |
   |                       |                       | .. note::                                                                                  |
   |                       |                       |                                                                                            |
   |                       |                       |    A negative result is invalid. Wait until the delay status is updated and query again.   |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
   | average               | integer               | Average delay of the current physical connection within 1 minute. The unit is microsecond. |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
   | max_delay             | integer               | Maximum delay of the current physical connection within 1 minute. The unit is microsecond. |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
