:original_name: dws_04_0813.html

.. _dws_04_0813:

PGXC_OS_THREADS
===============

**PGXC_OS_THREADS** displays thread status information under all normal nodes in the current cluster.

.. table:: **Table 1** PGXC_OS_THREADS columns

   +---------------+--------------------------+-----------------------------------------------------------------------------------+
   | Column        | Type                     | Description                                                                       |
   +===============+==========================+===================================================================================+
   | node_name     | Text                     | Names of all normal nodes currently in the cluster.                               |
   +---------------+--------------------------+-----------------------------------------------------------------------------------+
   | pid           | Bigint                   | Thread IDs currently running in the processes of all normal nodes in the cluster. |
   +---------------+--------------------------+-----------------------------------------------------------------------------------+
   | lwpid         | Integer                  | Lightweight thread IDs corresponding to the PIDs.                                 |
   +---------------+--------------------------+-----------------------------------------------------------------------------------+
   | thread_name   | Text                     | Thread names corresponding to the PIDs.                                           |
   +---------------+--------------------------+-----------------------------------------------------------------------------------+
   | creation_time | Timestamp with time zone | Creation time of the threads corresponding to the PIDs.                           |
   +---------------+--------------------------+-----------------------------------------------------------------------------------+
