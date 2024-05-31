:original_name: dws_04_0395.html

.. _dws_04_0395:

Monitoring Memory Resources
===========================

Monitoring the Memory
---------------------

GaussDB(DWS) provides a view for monitoring the memory usage of the entire cluster.

Query the pgxc_total_memory_detail view as a user with sysadmin permissions.

::

   SELECT * FROM pgxc_total_memory_detail;

If the following error message is returned during the query, enable the memory management function.

::

   SELECT * FROM pgxc_total_memory_detail;
   ERROR:  unsupported view for memory protection feature is disabled.
   CONTEXT:  PL/pgSQL function pgxc_total_memory_detail() line 12 at FOR over EXECUTE statement

You can set **enable_memory_limit** and **max_process_memory** on the GaussDB(DWS) console to enable memory management. The procedure is as follows:

#. Log in to the GaussDB(DWS) management console.
#. In the navigation pane on the left, click **Clusters**.
#. In the cluster list, find the target cluster and click its name. The **Basic Information** page is displayed.
#. Click the **Parameter Modification** tab, change the value of **enable_memory_limit** to **on**, and click **Save** to save the file.
#. Change the value of **max_process_memory** to a proper one. For details about the modification suggestions, see :ref:`max_process_memory <en-us_topic_0000001188163786__sadc1e0e8c1c246a4a6cad3967deebaad>`. After it is done, click **Save**.
#. In the **Modification Preview** dialog box, confirm the modifications and click **Save**. After the modification, restart the cluster for the modification to take effect.

Monitoring the Shared Memory
----------------------------

You can query the context information about the shared memory on the pg_shared_memory_detail view.

::

   SELECT * FROM pg_shared_memory_detail;
              contextname           | level |             parent              | totalsize | freesize | usedsize
   ---------------------------------+-------+---------------------------------+-----------+----------+----------
    ProcessMemory                   |     0 |                                 |     24576 |     9840 |    14736
    Workload manager memory context |     1 | ProcessMemory                   |   2105400 |     7304 |  2098096
    wlm collector hash table        |     2 | Workload manager memory context |      8192 |     3736 |     4456
    Resource pool hash table        |     2 | Workload manager memory context |     24576 |    15968 |     8608
    wlm cgroup hash table           |     2 | Workload manager memory context |     24576 |    15968 |     8608
   (5 rows)

This view lists the context name of the memory, level, the upper-layer memory context, and the total size of the shared memory.

In the database, GUC parameter **memory_tracking_mode** is used to configure the memory statistics collecting mode, including the following options:

-  **none:** The memory statistics collecting function is not enabled.

-  **normal:** Only memory statistics is collected in real time and no file is generated.

-  **executor:** The statistics file is generated, containing the context information about all allocated memory used on the execution layer.

   When the parameter is set to **executor**, cvs files are generated under the **pg_log** directory of the DN process. The file names are in the format of **memory_track\_**\ *<DN name>*\ **\_query\_**\ *<queryid>*\ **.csv**. The information about the operators executed by the postgres thread of the executor and all stream threads are input in this file during task execution.

   The instance is built with a file content similar to the following:

   .. code-block::

      0, 0, ExecutorState, 0, PortalHeapMemory, 0, 40K, 602K, 23
      1, 3, CStoreScan_29360131_25, 0, ExecutorState, 1, 265K, 554K, 23
      2, 128, cstore scan per scan memory context, 1, CStoreScan_29360131_25, 2, 24K, 24K, 23
      3, 127, cstore scan memory context, 1, CStoreScan_29360131_25, 2, 264K, 264K, 23
      4, 7, InitPartitionMapTmpMemoryContext, 1, CStoreScan_29360131_25, 2, 31K, 31K, 23
      5, 2, VecPartIterator_29360131_24, 0, ExecutorState, 1, 16K, 16K, 23
      0, 0, ExecutorState, 0, PortalHeapMemory, 0, 24K, 1163K, 20
      1, 3, CStoreScan_29360131_22, 0, ExecutorState, 1, 390K, 1122K, 20
      2, 20, cstore scan per scan memory context, 1, CStoreScan_29360131_22, 2, 476K, 476K, 20
      3, 19, cstore scan memory context, 1, CStoreScan_29360131_22, 2, 264K, 264K, 20
      4, 7, InitPartitionMapTmpMemoryContext, 1, CStoreScan_29360131_22, 2, 23K, 23K, 20
      5, 2, VecPartIterator_29360131_21, 0, ExecutorState, 1, 16K, 16K, 20

   The fields include the output SN, SN of the memory allocation context within the thread, name of the current memory context, output SN of the parent memory context, name of the parent memory context, tree layer No. of the memory context, peak memory used by the current memory context, peak memory used by the current memory context and all its child memory contexts, and plan node ID of the query where the thread is executed.

   In this example, the record "1, 3, CStoreScan_29360131_22, 0, ExecutorState, 1, 390K, 1122K, 20" represents the following information about Explain Analyze:

   -  **CstoreScan_29360131_22** indicates the CstoreScan operator.
   -  **1122K** indicates the peak memory used by the CstoreScan operator.

-  **fullexec:** The generated file includes the information about all memory contexts requested by the execution layer.

   If the parameter is set to **fullexec**, the output information will be similar to that for **executor**, except that some memory context allocation information may be returned because the information about all memory applications (no matter succeeded or not) is printed. As only the memory application information is recorded, the peak memory used by the memory context is recorded as **0**.
