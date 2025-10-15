:original_name: dws_04_0750.html

.. _dws_04_0750:

PG_SESSION_IOSTAT
=================

**PG_SESSION_IOSTAT** has been discarded in version 8.1.2 and is reserved for compatibility with earlier versions. This view is invalid in the current version. You can use :ref:`PGXC_WLM_SESSION_STATISTICS <dws_04_0841>` to view load management information about jobs being executed on all CNs.

.. table:: **Table 1** PG_SESSION_IOSTAT columns

   =========== ======= ==============================================
   Column      Type    Description
   =========== ======= ==============================================
   query_id    Bigint  Job ID
   mincurriops Integer Minimum I/O of the current job across DNs
   maxcurriops Integer Maximum I/O of the current job across DNs
   minpeakiops Integer Minimum peak I/O of the current job across DNs
   maxpeakiops Integer Maximum peak I/O of the current job across DNs
   io_limits   Integer **io_limits** set for the job
   io_priority Text    **io_priority** set for the job
   query       Text    Job
   node_group  Text    Logical cluster of the user running the job
   =========== ======= ==============================================
