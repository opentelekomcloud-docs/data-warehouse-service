:original_name: dws_04_0739.html

.. _dws_04_0739:

PG_OS_THREADS
=============

**PG_OS_THREADS** displays the status information about all the threads under the current node.

.. table:: **Table 1** PG_OS_THREADS columns

   +---------------+--------------------------+------------------------------------------------------+
   | Name          | Type                     | Description                                          |
   +===============+==========================+======================================================+
   | node_name     | text                     | Name of the current node                             |
   +---------------+--------------------------+------------------------------------------------------+
   | pid           | bigint                   | Thread number running under the current node process |
   +---------------+--------------------------+------------------------------------------------------+
   | lwpid         | integer                  | Lightweight thread ID corresponding to the PID       |
   +---------------+--------------------------+------------------------------------------------------+
   | thread_name   | text                     | Thread name corresponding to the PID                 |
   +---------------+--------------------------+------------------------------------------------------+
   | creation_time | timestamp with time zone | Thread creation time corresponding to the PID        |
   +---------------+--------------------------+------------------------------------------------------+
