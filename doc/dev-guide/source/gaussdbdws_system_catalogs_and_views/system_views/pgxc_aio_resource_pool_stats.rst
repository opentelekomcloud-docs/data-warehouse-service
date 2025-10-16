:original_name: dws_04_0017.html

.. _dws_04_0017:

PGXC_AIO_RESOURCE_POOL_STATS
============================

**PGXC_AIO_RESOURCE_POOL_STATS** queries the status of the asynchronous I/O resource pool usage for all nodes in the cluster. This includes the node name, the name of the asynchronous I/O resource type, the number of asynchronous I/O resources in use, and the number of idle asynchronous I/O resources. This view is supported only by 9.1.0.100 and later cluster versions.

.. table:: **Table 1** PGXC_AIO_RESOURCE_POOL_STATS columns

   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                  | Description                                                                                                                                                                            |
   +=======================+=======================+========================================================================================================================================================================================+
   | node_name             | Text                  | Node name.                                                                                                                                                                             |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource_name         | Text                  | Asynchronous I/O resource type. The options are:                                                                                                                                       |
   |                       |                       |                                                                                                                                                                                        |
   |                       |                       | -  **ASYNC_CONTEXT_TYPE**: Asynchronous context resource in the FPT (Future-Promise-Then) framework, at the thread level.                                                              |
   |                       |                       | -  **DISK_CACHE_CACHE_BLOCK_TYPE**: Instance of disk cache granularity block.                                                                                                          |
   |                       |                       | -  **DISK_CACHE_PATH_MANAGER_TYPE**: Cache path manager in the disk cache, at the thread level.                                                                                        |
   |                       |                       | -  **FUTURE_STATE_TYPE**: Shared state FutureState of Future and Promise in the FPT framework.                                                                                         |
   |                       |                       | -  **FUTURE_TYPE**: Future in the FPT framework, which provides a non-blocking way to get the result of an asynchronous task.                                                          |
   |                       |                       | -  **OBS_GET_IO_SCHEDULER_PERIODIC_STATS_SYSTEM_MESSAGE_TYPE**: System message of type **pgxc_obs_io_scheduler_periodic_stats** in the asynchronous scheduling module statistics view. |
   |                       |                       | -  **OBS_IO_REQUEST_TYPE**: I/O request in the asynchronous scheduling module.                                                                                                         |
   |                       |                       | -  **OBS_IO_STATS_SYSTEM_MESSAGE_TYPE**: System message of the **pgxc_obs_io_scheduler_stats** view in the asynchronous scheduling statistics module.                                  |
   |                       |                       | -  **OBS_MANAGE_PRIORITY_SYSTEM_MESSAGE_TYPE**: System message used to adjust priority in the asynchronous scheduling module.                                                          |
   |                       |                       | -  **OBS_SYSTEM_MESSAGE_TYPE**: System message in the asynchronous scheduling module.                                                                                                  |
   |                       |                       | -  **OBS_VFILE_TYPE**: Virtual file in OBS, OBS read/write handle.                                                                                                                     |
   |                       |                       | -  **READ_SEGMENT_TYPE**: Entity that merges multiple OBS read requests.                                                                                                               |
   |                       |                       | -  **SHARED_OBS_HANDLE_TYPE**: OBS Handler resource used to connect to the OBS service.                                                                                                |
   |                       |                       | -  **SHARED_VAR_AUTO_GUARD_TYPE**: Thread-level resource that manages OBS Handler and cached OBS file streams.                                                                         |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | busy_num              | Bigint                | Number of asynchronous I/O resources in use.                                                                                                                                           |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | idle_num              | Bigint                | Number of idle asynchronous I/O resources.                                                                                                                                             |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

::

   postgres=# select * from PGXC_AIO_RESOURCE_POOL_STATS;
     node_name   |                      resource_name                      | busy_num | idle_num
   --------------+---------------------------------------------------------+----------+----------
    cn_5001      | ASYNC_CONTEXT_TYPE                                      |        0 |        1
    cn_5001      | DISK_CACHE_CACHE_BLOCK_TYPE                             |        0 |        0
    cn_5001      | DISK_CACHE_PATH_MANAGER_TYPE                            |       24 |        0
    cn_5001      | FUTURE_STATE_TYPE                                       |        0 |        2
    cn_5001      | FUTURE_TYPE                                             |        0 |        1
    cn_5001      | OBS_GET_IO_SCHEDULER_PERIODIC_STATS_SYSTEM_MESSAGE_TYPE |        0 |        0
    cn_5001      | OBS_IO_REQUEST_TYPE                                     |        0 |        1
    cn_5001      | OBS_IO_STATS_SYSTEM_MESSAGE_TYPE                        |        0 |        0
    cn_5001      | OBS_MANAGE_PRIORITY_SYSTEM_MESSAGE_TYPE                 |        0 |        0
    cn_5001      | OBS_SYSTEM_MESSAGE_TYPE                                 |        0 |        0
    cn_5001      | OBS_VFILE_TYPE                                          |        0 |        0
    cn_5001      | READ_SEGMENT_TYPE                                       |        0 |        0
    cn_5001      | SHARED_OBS_HANDLE_TYPE                                  |        0 |        0
    cn_5001      | SHARED_VAR_AUTO_GUARD_TYPE                              |        0 |        0
    dn_6001_6002 | ASYNC_CONTEXT_TYPE                                      |        0 |        1
    dn_6001_6002 | DISK_CACHE_CACHE_BLOCK_TYPE                             |      719 |        0
    dn_6001_6002 | DISK_CACHE_PATH_MANAGER_TYPE                            |       25 |        1
    dn_6001_6002 | FUTURE_STATE_TYPE                                       |      719 |        2
    dn_6001_6002 | FUTURE_TYPE                                             |        0 |       39
    dn_6001_6002 | OBS_GET_IO_SCHEDULER_PERIODIC_STATS_SYSTEM_MESSAGE_TYPE |        0 |        0
    dn_6001_6002 | OBS_IO_REQUEST_TYPE                                     |        0 |        3
    dn_6001_6002 | OBS_IO_STATS_SYSTEM_MESSAGE_TYPE                        |        0 |        0
    dn_6001_6002 | OBS_MANAGE_PRIORITY_SYSTEM_MESSAGE_TYPE                 |        0 |        0
    dn_6001_6002 | OBS_SYSTEM_MESSAGE_TYPE                                 |        0 |        0
    dn_6001_6002 | OBS_VFILE_TYPE                                          |        0 |       16
    dn_6001_6002 | READ_SEGMENT_TYPE                                       |        0 |        2
    dn_6001_6002 | SHARED_OBS_HANDLE_TYPE                                  |        0 |        1
    dn_6001_6002 | SHARED_VAR_AUTO_GUARD_TYPE                              |        0 |        1
    dn_6003_6004 | ASYNC_CONTEXT_TYPE                                      |        0 |        1
    dn_6003_6004 | DISK_CACHE_CACHE_BLOCK_TYPE                             |      715 |        0
    dn_6003_6004 | DISK_CACHE_PATH_MANAGER_TYPE                            |       25 |        1
    dn_6003_6004 | FUTURE_STATE_TYPE                                       |      715 |        2
    dn_6003_6004 | FUTURE_TYPE                                             |        0 |       39
    dn_6003_6004 | OBS_GET_IO_SCHEDULER_PERIODIC_STATS_SYSTEM_MESSAGE_TYPE |        0 |        0
    dn_6003_6004 | OBS_IO_REQUEST_TYPE                                     |        0 |        3
    dn_6003_6004 | OBS_IO_STATS_SYSTEM_MESSAGE_TYPE                        |        0 |        0
    dn_6003_6004 | OBS_MANAGE_PRIORITY_SYSTEM_MESSAGE_TYPE                 |        0 |        0
    dn_6003_6004 | OBS_SYSTEM_MESSAGE_TYPE                                 |        0 |        0
    dn_6003_6004 | OBS_VFILE_TYPE                                          |        0 |       16
    dn_6003_6004 | READ_SEGMENT_TYPE                                       |        0 |        2
    dn_6003_6004 | SHARED_OBS_HANDLE_TYPE                                  |        0 |        1
    dn_6003_6004 | SHARED_VAR_AUTO_GUARD_TYPE                              |        0 |        1
   (42 rows)
