:original_name: dws_04_0798.html

.. _dws_04_0798:

PGXC_COMM_DELAY
===============

**PGXC_COMM_STATUS** displays the communication library delay status for all the DNs.

.. table:: **Table 1** PGXC_COMM_DELAY columns

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
   | average               | integer               | Average delay of the current physical connection within 1 minute. Its unit is microsecond. |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
   | max_delay             | integer               | Maximum delay of the current physical connection within 1 minute. Its unit is microsecond. |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
