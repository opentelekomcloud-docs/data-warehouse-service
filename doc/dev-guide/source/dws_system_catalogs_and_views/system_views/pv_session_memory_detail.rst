:original_name: dws_04_0852.html

.. _dws_04_0852:

PV_SESSION_MEMORY_DETAIL
========================

**PV_SESSION_MEMORY_DETAIL** displays statistics about thread memory usage by memory context.

The memory context TempSmallContextGroup collects information about all memory contexts whose value in the **totalsize** column is less than 8192 bytes in the current thread, and the number of the collected memory contexts is recorded in the **usedsize** column. Therefore, the **totalsize** and **freesize** columns for TempSmallContextGroup in the view display the corresponding information about all the memory contexts whose value in the **totalsize** column is less than 8192 bytes in the current thread, and the **usedsize** column displays the number of these memory contexts.

You can run the **SELECT \* FROM pv_session_memctx_detail (**\ *threadid*\ **,'');** statement to record information about all memory contexts of a thread into the *threadid*\ \_\ *timestamp*\ **.log** file in the **/tmp/dumpmem** directory. *threadid* can be obtained from the following table.

.. table:: **Table 1** PV_SESSION_MEMORY_DETAIL columns

   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column      | Type     | Description                                                                                                                                       |
   +=============+==========+===================================================================================================================================================+
   | sessid      | Text     | Thread start time + Thread ID + Node name (string format: *timestamp*.\ *threadid*.\ *nodename*)                                                  |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | sesstype    | Text     | Thread name                                                                                                                                       |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | contextname | Text     | Name of the memory context                                                                                                                        |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | level       | Smallint | Hierarchy of the memory context                                                                                                                   |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | parent      | Text     | Name of the parent memory context                                                                                                                 |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | totalsize   | Bigint   | Total size of the memory context, in bytes                                                                                                        |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | freesize    | Bigint   | Total size of released memory in the memory context, in bytes                                                                                     |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | usedsize    | Bigint   | Size of used memory in the memory context, in bytes. For TempSmallContextGroup, this parameter specifies the number of collected memory contexts. |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Query the usage of all MemoryContexts on the current node.

Locate the thread in which the MemoryContext is created and used based on **sessid**. Check whether the memory usage meets the expectation based on **totalsize**, **freesize**, and **usedsize** to see whether memory leakage may occur.

::

   SELECT * FROM pv_session_memory_detail;
                    sessid                  |        sesstype        |                  contextname                   | level |            parent            | totalsize | freesize | usedsize
   -----------------------------------------+------------------------+------------------------------------------------+-------+------------------------------+-----------+----------+----------
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | TempSmallContextGroup                          |     0 |                              |    646520 |   125184 |      149
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | TopMemoryContext                               |     0 |                              |    372672 |    19488 |   353184
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | Record information cache                       |     1 | TopMemoryContext             |     24576 |    13632 |    10944
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | Operator class cache                           |     1 | TopMemoryContext             |     24576 |    15168 |     9408
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | smgr relation table                            |     1 | TopMemoryContext             |     24576 |    11040 |    13536
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | TransactionAbortContext                        |     1 | TopMemoryContext             |     32768 |    32704 |       64
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | Portal hash                                    |     1 | TopMemoryContext             |     24576 |    16192 |     8384
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | CacheMemoryContext                             |     1 | TopMemoryContext             |   1858848 |    10640 |  1848208
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | InvalidCacheMemoryContext                      |     2 | CacheMemoryContext           |   2088960 |   828928 |  1260032
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | WAL record construction                        |     1 | TopMemoryContext             |     49792 |     6528 |    43264
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | LOCAL LOCK hash                                |     1 | TopMemoryContext             |     24576 |    15936 |     8640
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | Timezones                                      |     1 | TopMemoryContext             |     24576 |    15808 |     8768
    1760444600.140326303563096.coordinator1 | GlobalDeadLockDetector | SELF MEMORY CONTEXT                            |     1 | TopMemoryContext             |     66600 |    17824 |    48776
    1760444600.140326303562480.coordinator1 | file_recyclebin        | TempSmallContextGroup                          |     0 |                              |    642688 |   129040 |      147
    1760444600.140326303562480.coordinator1 | file_recyclebin        | TopMemoryContext                               |     0 |                              |   1437632 |    14256 |  1423376
    1760444600.140326303562480.coordinator1 | file_recyclebin        | Operator class cache                           |     1 | TopMemoryContext             |     24576 |    15168 |     9408
    1760444600.140326303562480.coordinator1 | file_recyclebin        | smgr relation table                            |     1 | TopMemoryContext             |     24576 |    11040 |    13536
    1760444600.140326303562480.coordinator1 | file_recyclebin        | TransactionAbortContext                        |     1 | TopMemoryContext             |     32768 |    32704 |       64
    1760444600.140326303562480.coordinator1 | file_recyclebin        | Portal hash                                    |     1 | TopMemoryContext             |     24576 |    16192 |     8384
    1760444600.140326303562480.coordinator1 | file_recyclebin        | CacheMemoryContext                             |     1 | TopMemoryContext             |   1858848 |    12096 |  1846752
