:original_name: dws_04_0739.html

.. _dws_04_0739:

PG_OS_THREADS
=============

**PG_OS_THREADS** displays the status information about all the threads under the current node.

.. table:: **Table 1** PG_OS_THREADS columns

   +---------------+--------------------------+--------------------------------------------------------+
   | Column        | Type                     | Description                                            |
   +===============+==========================+========================================================+
   | node_name     | Text                     | Name of the node                                       |
   +---------------+--------------------------+--------------------------------------------------------+
   | pid           | Bigint                   | Thread number running under the current node process   |
   +---------------+--------------------------+--------------------------------------------------------+
   | lwpid         | Integer                  | Lightweight thread IDs corresponding to the PIDs       |
   +---------------+--------------------------+--------------------------------------------------------+
   | thread_name   | Text                     | Thread names corresponding to the PIDs                 |
   +---------------+--------------------------+--------------------------------------------------------+
   | creation_time | Timestamp with time zone | Creation time of the threads corresponding to the PIDs |
   +---------------+--------------------------+--------------------------------------------------------+
