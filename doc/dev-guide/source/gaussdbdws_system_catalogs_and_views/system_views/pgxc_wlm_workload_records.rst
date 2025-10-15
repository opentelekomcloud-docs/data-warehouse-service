:original_name: dws_04_0842.html

.. _dws_04_0842:

PGXC_WLM_WORKLOAD_RECORDS
=========================

**PGXC_WLM_WORKLOAD_RECORDS** displays the status of job executed by the current user on CNs. Only the system administrator or the preset role **gs_role_read_all_stats** can access this view. This view is available only when **enable_dynamic_workload** is set to **on**.

.. table:: **Table 1** PGXC_WLM_WORKLOAD_RECORDS columns

   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | Column                | Type                  | Description                                                         |
   +=======================+=======================+=====================================================================+
   | node_name             | Text                  | Name of the CN where the job is executed.                           |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | thread_id             | Bigint                | ID of the backend thread.                                           |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | processid             | Integer               | lwpid of the thread.                                                |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | Timestamp             | Bigint                | Start time of statement execution.                                  |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | username              | Name                  | Username logged in to the backend.                                  |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | memory                | Integer               | Memory required for the statement.                                  |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | active_points         | Integer               | Number of resources consumed by the statement on the resource pool. |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | max_points            | Integer               | Maximum number of resources in the resource pool.                   |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | priority              | Integer               | Priority of a job.                                                  |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | resource_pool         | Text                  | Resource pool where a job is.                                       |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | status                | Text                  | Job execution status. The options are:                              |
   |                       |                       |                                                                     |
   |                       |                       | **pending**                                                         |
   |                       |                       |                                                                     |
   |                       |                       | **running**                                                         |
   |                       |                       |                                                                     |
   |                       |                       | **finished**                                                        |
   |                       |                       |                                                                     |
   |                       |                       | **aborted**                                                         |
   |                       |                       |                                                                     |
   |                       |                       | **unknown**                                                         |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | control_group         | Text                  | Cgroups used by a job.                                              |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | enqueue               | Text                  | Queue for the job, including:                                       |
   |                       |                       |                                                                     |
   |                       |                       | **GLOBAL**: global queue.                                           |
   |                       |                       |                                                                     |
   |                       |                       | **RESPOOL**: resource pool queue.                                   |
   |                       |                       |                                                                     |
   |                       |                       | **ACTIVE**: not queued.                                             |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
   | query                 | Text                  | Statement currently being executed.                                 |
   +-----------------------+-----------------------+---------------------------------------------------------------------+
