:original_name: dws_04_0842.html

.. _dws_04_0842:

PGXC_WLM_WORKLOAD_RECORDS
=========================

**PGXC_WLM_WORKLOAD_RECORDS** displays the status of job executed by the current user on CNs. It is accessible only to users with system administrator rights. This view is available only when **enable_dynamic_workload** is set to **on**.

.. table:: **Table 1** PGXC_WLM_WORKLOAD_RECORDS columns

   +-----------------------+-----------------------+----------------------------------------------------------------+
   | Name                  | Type                  | Description                                                    |
   +=======================+=======================+================================================================+
   | node_name             | text                  | Name of the CN where the job is executed                       |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | thread_id             | bigint                | ID of the backend thread                                       |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | processid             | integer               | lwpid of a thread                                              |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | timestamp             | bigint                | Time when a statement starts to be executed                    |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | username              | name                  | Name of the user logging in to the backend                     |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | memory                | integer               | Memory required by a statement                                 |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | active_points         | integer               | Number of resources consumed by a statement in a resource pool |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | max_points            | integer               | Maximum number of resources in a resource pool                 |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | priority              | integer               | Priority of a job                                              |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | resource_pool         | text                  | Resource pool to which a job belongs                           |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | status                | text                  | Job execution status. Its value can be:                        |
   |                       |                       |                                                                |
   |                       |                       | **pending**                                                    |
   |                       |                       |                                                                |
   |                       |                       | **running**                                                    |
   |                       |                       |                                                                |
   |                       |                       | **finished**                                                   |
   |                       |                       |                                                                |
   |                       |                       | **aborted**                                                    |
   |                       |                       |                                                                |
   |                       |                       | **unknown**                                                    |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | control_group         | text                  | Cgroups used by a job                                          |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | enqueue               | text                  | Queue that a job is in. Its value can be:                      |
   |                       |                       |                                                                |
   |                       |                       | **GLOBAL**: global queue                                       |
   |                       |                       |                                                                |
   |                       |                       | **RESPOOL**: resource pool queue                               |
   |                       |                       |                                                                |
   |                       |                       | **ACTIVE**: not in a queue                                     |
   +-----------------------+-----------------------+----------------------------------------------------------------+
   | query                 | text                  | Statement that is being executed                               |
   +-----------------------+-----------------------+----------------------------------------------------------------+
