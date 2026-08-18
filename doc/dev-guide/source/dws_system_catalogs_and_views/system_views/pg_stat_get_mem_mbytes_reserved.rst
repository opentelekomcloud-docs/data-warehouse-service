:original_name: dws_04_0763.html

.. _dws_04_0763:

PG_STAT_GET_MEM_MBYTES_RESERVED
===============================

**PG_STAT_GET_MEM_MBYTES_RESERVED** displays the current activity information of a thread stored in memory. You need to specify the thread ID (pid in :ref:`PG_STAT_ACTIVITY <dws_04_0755>`) for query. If the thread ID is set to **0**, the current thread ID is used. For example:

::

   SELECT pg_stat_get_mem_mbytes_reserved(0);

.. table:: **Table 1** PG_STAT_GET_MEM_MBYTES_RESERVED columns

   ==================== ===================================
   Column               Description
   ==================== ===================================
   ConnectInfo          Connection information.
   ParctlManager        Concurrency management information.
   GeneralParams        Basic parameter information.
   GeneralParams RPDATA Basic resource pool information.
   ExceptionManager     Exception management information.
   CollectInfo          Collection information.
   GeneralInfo          Basic information.
   ParctlState          Concurrency status information.
   CPU INFO             CPU information.
   ControlGroup         Cgroup information.
   IOSTATE              I/O status information.
   ==================== ===================================
