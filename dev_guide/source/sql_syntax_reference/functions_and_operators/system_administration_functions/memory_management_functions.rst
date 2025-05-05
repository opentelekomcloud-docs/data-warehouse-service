:original_name: dws_06_0379.html

.. _dws_06_0379:

Memory Management Functions
===========================

This function is supported only by cluster versions 9.1.0 and later.

pg_shared_chunk_detail(contextname char(64))
--------------------------------------------

Description: This function queries information about all chunks requested by the memory context in a specified shared memory.

The **contextname** parameter indicates the name of the memory context.

Before using this function, use the **pg_shared_chunk_dump(contextname char(64))** function to print related information to a file.

Return type: record

The following is an example:

::

   SELECT * FROM pg_shared_chunk_detail('pgstat file hash table');
          parent        | level |  file_name   | line_number | chunk_size | requested_number | total_size
   ---------------------+-------+--------------+-------------+------------+------------------+------------
    pgstat file context |     2 | dynahash.cpp |         158 |       2048 |                1 |       2048
    pgstat file context |     2 | dynahash.cpp |         158 |        256 |                1 |        256
    pgstat file context |     2 | dynahash.cpp |         158 |       4096 |                9 |      36864
    pgstat file context |     2 | dynahash.cpp |         158 |       8192 |                4 |      32768
   (4 rows)

pv_session_chunk_detail(tid int, contextname char(64))
------------------------------------------------------

Description: This function queries information about all chunks applied for by a memory context created by a specified thread.

The **tid** parameter indicates the thread ID, while the **contextname** parameter indicates the memory context name.

To use this function, you need to use the **pv_session_chunk_dump(tid int, contextname char(64))** function to print related information to a file.

Return type: record

The following is an example:

::

   SELECT * FROM pv_session_chunk_detail(140178810990936, 'Timezones');
         parent      | level |   file_name   | line_number | chunk_size | requested_number | total_size
   ------------------+-------+---------------+-------------+------------+------------------+------------
    TopMemoryContext |     1 | dynahash.cpp  |         158 |       1280 |                2 |       2560
    TopMemoryContext |     1 | dynahash.cpp  |         158 |        160 |                1 |        160
    TopMemoryContext |     1 | dynahash.cpp  |         158 |       2560 |                2 |       5120
    TopMemoryContext |     1 | localtime.cpp |        1965 |        128 |                1 |        128
    TopMemoryContext |     1 | localtime.cpp |        1965 |        448 |                1 |        448
   (5 rows)

pg_shared_chunk_dump(contextname char(64))
------------------------------------------

Description: This function prints information about all chunks applied for by the memory context in the specified shared memory as a file.

The **contextname** parameter indicates the name of the memory context.

Return type: Boolean

The following is an example:

::

   SELECT * FROM pg_shared_chunk_dump('pgstat file hash table');
    pg_shared_chunk_dump
   ----------------------
            t
   (1 row)

pv_session_chunk_dump(tid int, contextname char(64))
----------------------------------------------------

Description: This function prints information about all chunks applied for by a memory context created by a specified thread into a file.

The **tid** parameter indicates the thread ID, while the **contextname** parameter indicates the memory context name.

Return type: Boolean

The following is an example:

::

   SELECT * FROM pv_session_chunk_dump(140472797325280, 'Timezones');
    pv_session_chunk_dump
   -----------------------
    t
   (1 row)
