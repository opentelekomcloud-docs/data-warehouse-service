:original_name: dws_04_0760.html

.. _dws_04_0760:

PG_STAT_BGWRITER
================

**PG_STAT_BGWRITER** displays statistics about the background writer process's activity.

.. table:: **Table 1** PG_STAT_BGWRITER columns

   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
   | Column                | Type                     | Description                                                                                          |
   +=======================+==========================+======================================================================================================+
   | checkpoints_timed     | Bigint                   | Number of scheduled checkpoints that have been performed.                                            |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
   | checkpoints_req       | Bigint                   | Number of requested checkpoints that have been performed.                                            |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
   | checkpoint_write_time | Double precision         | Time spent on writing files to the disk during checkpoints, in milliseconds.                         |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
   | checkpoint_sync_time  | Double precision         | Time spent on synchronizing data to the disk during checkpoints, in milliseconds.                    |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
   | buffers_checkpoint    | Bigint                   | Number of buffers written during checkpoints.                                                        |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
   | buffers_clean         | Bigint                   | Number of buffers written by the background writer.                                                  |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
   | maxwritten_clean      | Bigint                   | Number of times the background writer stopped a cleaning scan because too many buffers were written. |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
   | buffers_backend       | Bigint                   | Number of buffers written directly by a backend                                                      |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
   | buffers_backend_fsync | Bigint                   | Number of times that the backend has to execute **fsync**.                                           |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
   | buffers_alloc         | Bigint                   | Number of buffers allocated.                                                                         |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
   | stats_reset           | Timestamp with time zone | Time at which these statistics were reset.                                                           |
   +-----------------------+--------------------------+------------------------------------------------------------------------------------------------------+
