:original_name: dws_04_0813.html

.. _dws_04_0813:

PGXC_OS_THREADS
===============

**PGXC_OS_THREADS** displays thread status information under all normal nodes in the current cluster.

.. table:: **Table 1** PGXC_OS_THREADS columns

   +---------------+--------------------------+-----------------------------------------------------------------------------------+
   | Name          | Type                     | Description                                                                       |
   +===============+==========================+===================================================================================+
   | node_name     | text                     | All normal node names in the cluster                                              |
   +---------------+--------------------------+-----------------------------------------------------------------------------------+
   | pid           | bigint                   | IDs of running threads among all the normal node processes in the current cluster |
   +---------------+--------------------------+-----------------------------------------------------------------------------------+
   | lwpid         | integer                  | Lightweight thread ID corresponding to the PID                                    |
   +---------------+--------------------------+-----------------------------------------------------------------------------------+
   | thread_name   | text                     | Thread name corresponding to the PID                                              |
   +---------------+--------------------------+-----------------------------------------------------------------------------------+
   | creation_time | timestamp with time zone | Thread creation time corresponding to the PID                                     |
   +---------------+--------------------------+-----------------------------------------------------------------------------------+
