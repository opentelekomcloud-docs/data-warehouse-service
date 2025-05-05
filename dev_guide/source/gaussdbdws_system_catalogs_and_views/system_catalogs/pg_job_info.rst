:original_name: dws_04_1104.html

.. _dws_04_1104:

PG_JOB_INFO
===========

**PG_JOB_INFO** records the execution results of scheduled tasks. The schema of the system catalog is **dbms_om**.

.. table:: **Table 1** dbms_om.pg_job_info columns

   ========== =================== =====================================
   Column     Type                Description
   ========== =================== =====================================
   job_id     Integer             Job ID
   job_db     OID                 OID of the database where the task is
   start_time Timestamp with zone Task execution start time
   status     character(8)        Task execution status
   end_time   Timestamp with zone Task execution end time
   err_msg    Text                Task execution error information
   ========== =================== =====================================
