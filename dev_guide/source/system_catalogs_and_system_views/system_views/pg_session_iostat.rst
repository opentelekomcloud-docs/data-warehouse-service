:original_name: dws_04_0750.html

.. _dws_04_0750:

PG_SESSION_IOSTAT
=================

**PG_SESSION_IOSTAT** has been discarded in version 8.1.2 and is reserved for compatibility with earlier versions. This view is invalid in the current version.

.. table:: **Table 1** PG_SESSION_IOSTAT columns

   =========== ======= ==============================================
   Name        Type    Description
   =========== ======= ==============================================
   query_id    bigint  Job ID
   mincurriops integer Minimum I/O of the current job across DNs
   maxcurriops integer Maximum I/O of the current job across DNs
   minpeakiops integer Minimum peak I/O of the current job across DNs
   maxpeakiops integer Maximum peak I/O of the current job across DNs
   io_limits   integer **io_limits** set for the job
   io_priority text    **io_priority** set for the job
   query       text    Job
   node_group  text    Logical cluster of the user running the job
   =========== ======= ==============================================
