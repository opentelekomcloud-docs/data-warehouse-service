:original_name: dws_04_0852.html

.. _dws_04_0852:

PV_SESSION_MEMORY_DETAIL
========================

**PV_SESSION_MEMORY_DETAIL** displays statistics about thread memory usage by memory context.

The memory context TempSmallContextGroup collects information about all memory contexts whose value in the **totalsize** column is less than 8192 bytes in the current thread, and the number of the collected memory contexts is recorded in the **usedsize** column. Therefore, the **totalsize** and **freesize** columns for TempSmallContextGroup in the view display the corresponding information about all the memory contexts whose value in the **totalsize** column is less than 8192 bytes in the current thread, and the **usedsize** column displays the number of these memory contexts.

You can run the **SELECT \* FROM pv_session_memctx_detail (**\ *threadid*\ **,'');** statement to record information about all memory contexts of a thread into the *threadid*\ \_\ *timestamp*\ **.log** file in the **/tmp/dumpmem** directory. *threadid* can be obtained from the following table.

.. table:: **Table 1** PV_SESSION_MEMORY_DETAIL columns

   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name        | Type     | Description                                                                                                                                       |
   +=============+==========+===================================================================================================================================================+
   | sessid      | text     | Thread start time+thread ID (string: *timestamp*.\ *threadid*)                                                                                    |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | sesstype    | text     | Thread name                                                                                                                                       |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | contextname | text     | Name of the memory context                                                                                                                        |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | level       | smallint | Hierarchy of the memory context                                                                                                                   |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | parent      | text     | Name of the parent memory context                                                                                                                 |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | totalsize   | bigint   | Total size of the memory context, in bytes                                                                                                        |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | freesize    | bigint   | Total size of released memory in the memory context, in bytes                                                                                     |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
   | usedsize    | bigint   | Size of used memory in the memory context, in bytes. For TempSmallContextGroup, this parameter specifies the number of collected memory contexts. |
   +-------------+----------+---------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Query the usage of all MemoryContexts on the current node.

Locate the thread in which the MemoryContext is created and used based on **sessid**. Check whether the memory usage meets the expectation based on **totalsize**, **freesize**, and **usedsize** to see whether memory leakage may occur.

::

   SELECT * FROM PV_SESSION_MEMORY_DETAIL order by totalsize desc;
              sessid           |        sesstype         |                 contextname                 | level |            parent            | totalsize | freesize | usedsize
   ----------------------------+-------------------------+---------------------------------------------+-------+------------------------------+-----------+----------+----------
    0.139975915622720          | postmaster              | gs_signal                                   |     1 | TopMemoryContext             |  17209904 |  8081136 |  9128768
    1667462258.139973631031040 | postgres                | SRF multi-call context                      |     5 | FunctionScan_139973631031040 |   1725504 |     3168 |  1722336
    1667461280.139973666686720 | postgres                | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   284456 |  1188088
    1667450443.139973877479168 | postgres                | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   356088 |  1116456
    1667462258.139973631031040 | postgres                | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   128216 |  1344328
    1667461250.139973915236096 | postgres                | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   226352 |  1246192
    1667450439.139974010144512 | WLMarbiter              | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   386736 |  1085808
    1667450439.139974151726848 | WDRSnapshot             | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   159720 |  1312824
    1667450439.139974026925824 | WLMmonitor              | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   297976 |  1174568
    1667451036.139973746386688 | postgres                | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   208064 |  1264480
    1667461250.139973950891776 | postgres                | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   270016 |  1202528
    1667450439.139974076212992 | WLMCalSpaceInfo         | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   393952 |  1078592
    1667450439.139974092994304 | WLMCollectWorker        | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |    94848 |  1377696
    1667461254.139973971343104 | postgres                | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   338544 |  1134000
    1667461280.139973822945024 | postgres                | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   284456 |  1188088
    1667450439.139974202070784 | JobScheduler            | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   216728 |  1255816
    1667450454.139973860697856 | postgres                | CacheMemoryContext                          |     1 | TopMemoryContext             |   1472544 |   388384 |  1084160
    0.139975915622720          | postmaster              | Postmaster                                  |     1 | TopMemoryContext             |   1004288 |    88792 |   915496
    1667450439.139974218852096 | AutoVacLauncher         | CacheMemoryContext                          |     1 | TopMemoryContext             |    948256 |   183488 |   764768
    1667461250.139973915236096 | postgres                | TempSmallContextGroup                       |     0 |                              |    584448 |   148032 |      119
    1667462258.139973631031040 | postgres                | TempSmallContextGroup                       |     0 |                              |    579712 |   162128 |      123
