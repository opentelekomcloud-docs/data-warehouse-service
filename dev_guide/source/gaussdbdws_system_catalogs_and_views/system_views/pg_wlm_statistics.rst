:original_name: dws_04_0794.html

.. _dws_04_0794:

PG_WLM_STATISTICS
=================

**PG_WLM_STATISTICS** displays information about workload management after the task is complete or the exception has been handled. This view has been discarded in 8.1.2. You can use :ref:`PGXC_WLM_SESSION_INFO <dws_04_0839>` to view load management records of completed jobs executed on all CNs.

.. table:: **Table 1** PG_WLM_STATISTICS columns

   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                  | Description                                                                                                                 |
   +=======================+=======================+=============================================================================================================================+
   | statement             | Text                  | Statement executed for exception handling                                                                                   |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | block_time            | Bigint                | Block time before the statement is executed                                                                                 |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | elapsed_time          | Bigint                | Elapsed time when the statement is executed                                                                                 |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | total_cpu_time        | Bigint                | Total time used by the CPU on the DN when the statement is executed for exception handling                                  |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | qualification_time    | Bigint                | Period when the statement checks the inclination ratio                                                                      |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | cpu_skew_percent      | Integer               | CPU usage skew on the DN when the statement is executed for exception handling                                              |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | control_group         | Text                  | Cgroup used when the statement is executed for exception handling                                                           |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | status                | Text                  | Statement status after it is executed for exception handling                                                                |
   |                       |                       |                                                                                                                             |
   |                       |                       | -  **pending**: The statement is waiting to be executed.                                                                    |
   |                       |                       | -  **running**: The statement is being executed.                                                                            |
   |                       |                       | -  **finished**: The execution is finished normally.                                                                        |
   |                       |                       | -  **abort**: The execution is unexpectedly terminated.                                                                     |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | action                | Text                  | Actions when statements are executed for exception handling                                                                 |
   |                       |                       |                                                                                                                             |
   |                       |                       | -  **abort** indicates terminating the operation.                                                                           |
   |                       |                       | -  **adjust** indicates executing the Cgroup adjustment operations. Currently, you can only perform the demotion operation. |
   |                       |                       | -  **finish** indicates that the operation is normally finished.                                                            |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | queryid               | Bigint                | Internal query ID used for statement execution                                                                              |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | threadid              | Bigint                | ID of the backend thread                                                                                                    |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------+
