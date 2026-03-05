:original_name: UpdateQueueResources.html

.. _UpdateQueueResources:

Modifying Resource Configurations of a Resource Pool
====================================================

Function
--------

This API is used to modify the resource configurations of a resource pool.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

PUT /v2/{project_id}/clusters/{cluster_id}/workload/queues/{queue_name}/resources

.. table:: **Table 1** Path Parameters

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                |
   +=================+=================+=================+============================================================================================================+
   | project_id      | Yes             | String          | **Definition**                                                                                             |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | Project ID. To obtain the value, see :ref:`Obtaining a Project ID <dws_02_0011>`.                          |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Constraints**                                                                                            |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Range**                                                                                                  |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Default Value**                                                                                          |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+
   | cluster_id      | Yes             | String          | **Definition**                                                                                             |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | Cluster ID. For details about how to obtain the value, see :ref:`Obtaining the Cluster ID <dws_02_00068>`. |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Constraints**                                                                                            |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | The value must be a valid DWS cluster ID.                                                                  |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Range**                                                                                                  |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | It is a 36-digit UUID.                                                                                     |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Default Value**                                                                                          |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+
   | queue_name      | Yes             | String          | **Definition**                                                                                             |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | Resource pool name.                                                                                        |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Constraints**                                                                                            |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Range**                                                                                                  |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Default Value**                                                                                          |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------+----------------------------+
   | Parameter       | Mandatory       | Type                                                                                                                   | Description                |
   +=================+=================+========================================================================================================================+============================+
   | workload_queue  | Yes             | :ref:`WorkloadQueueInfo <en-us_topic_0000002500014084__en-us_topic_0000002375015109_request_workloadqueueinfo>` object | **Definition**             |
   |                 |                 |                                                                                                                        |                            |
   |                 |                 |                                                                                                                        | Resource pool information. |
   |                 |                 |                                                                                                                        |                            |
   |                 |                 |                                                                                                                        | **Constraints**            |
   |                 |                 |                                                                                                                        |                            |
   |                 |                 |                                                                                                                        | N/A                        |
   |                 |                 |                                                                                                                        |                            |
   |                 |                 |                                                                                                                        | **Range**                  |
   |                 |                 |                                                                                                                        |                            |
   |                 |                 |                                                                                                                        | N/A                        |
   |                 |                 |                                                                                                                        |                            |
   |                 |                 |                                                                                                                        | **Default Value**          |
   |                 |                 |                                                                                                                        |                            |
   |                 |                 |                                                                                                                        | N/A                        |
   +-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------+----------------------------+

.. _en-us_topic_0000002500014084__en-us_topic_0000002375015109_request_workloadqueueinfo:

.. table:: **Table 3** WorkloadQueueInfo

   +----------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------+-------------------------------+
   | Parameter            | Mandatory       | Type                                                                                                                                   | Description                   |
   +======================+=================+========================================================================================================================================+===============================+
   | workload_queue_name  | Yes             | String                                                                                                                                 | **Definition**                |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | Resource pool name.           |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | **Constraints**               |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | N/A                           |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | **Range**                     |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | N/A                           |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | **Default Value**             |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | N/A                           |
   +----------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------+-------------------------------+
   | logical_cluster_name | No              | String                                                                                                                                 | **Definition**                |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | Logical cluster name.         |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | **Constraints**               |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | N/A                           |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | **Range**                     |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | N/A                           |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | **Default Value**             |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | N/A                           |
   +----------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------+-------------------------------+
   | resource_item_list   | Yes             | Array of :ref:`WorkloadResourceItem <en-us_topic_0000002500014084__en-us_topic_0000002375015109_request_workloadresourceitem>` objects | **Definition**                |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | Resource configuration queue. |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | **Constraints**               |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | N/A                           |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | **Range**                     |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | N/A                           |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | **Default Value**             |
   |                      |                 |                                                                                                                                        |                               |
   |                      |                 |                                                                                                                                        | N/A                           |
   +----------------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------+-------------------------------+

.. _en-us_topic_0000002500014084__en-us_topic_0000002375015109_request_workloadresourceitem:

.. table:: **Table 4** WorkloadResourceItem

   +----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter            | Mandatory       | Type            | Description                                                                                                                                                |
   +======================+=================+=================+============================================================================================================================================================+
   | resource_name        | Yes             | String          | **Definition**                                                                                                                                             |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | Resource name.                                                                                                                                             |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Constraints**                                                                                                                                            |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | N/A                                                                                                                                                        |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Range**                                                                                                                                                  |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **cpu**: percentage of CPU time                                                                                                                            |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **cpu_limit**: percentage of CPU cores                                                                                                                     |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **memory**: percentage of available memory resources on each data node                                                                                     |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **concurrency**: number of concurrent queries                                                                                                              |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **shortQueryConcurrencyNum**: number of concurrent simple statements                                                                                       |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **weight**: weight for network scheduling                                                                                                                  |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Default Value**                                                                                                                                          |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | N/A                                                                                                                                                        |
   +----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource_value       | Yes             | Integer         | **Definition**                                                                                                                                             |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | Resource attribute value.                                                                                                                                  |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Constraints**                                                                                                                                            |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | N/A                                                                                                                                                        |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Range**                                                                                                                                                  |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | The value range varies according to the value of **resource_name**.                                                                                        |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | If **resource_name** is **cpu**, the value is an integer from **1** to **99**.                                                                             |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | If **resource_name** is **cpu_limit**, the value is an integer from **0** to **100**. The value **0** indicates no limit.                                  |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | If **resource_name** is **memory**, the value is an integer from **0** to **100**. The value **0** indicates that no limit.                                |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | If **resource_name** is **concurrency**, the value is an integer from **1** to **2147483647**. The value **-1** or **0** indicates no limit.               |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | If **resource_name** is **shortQueryConcurrencyNum**, the value is an integer from **-1** to **2147483647**. The value **-1** or **0** indicates no limit. |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | If **resource_name** is **weight**, the value is an integer from **1** to **2147483647**. The default value is **-1**.                                     |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Default Value**                                                                                                                                          |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | N/A                                                                                                                                                        |
   +----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value_unit           | No              | String          | **Definition**                                                                                                                                             |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | Resource attribute unit.                                                                                                                                   |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Constraints**                                                                                                                                            |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | N/A                                                                                                                                                        |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Range**                                                                                                                                                  |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | N/A                                                                                                                                                        |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Default Value**                                                                                                                                          |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | N/A                                                                                                                                                        |
   +----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource_description | No              | String          | **Definition**                                                                                                                                             |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | Additional resource description.                                                                                                                           |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Constraints**                                                                                                                                            |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | N/A                                                                                                                                                        |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Range**                                                                                                                                                  |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | N/A                                                                                                                                                        |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | **Default Value**                                                                                                                                          |
   |                      |                 |                 |                                                                                                                                                            |
   |                      |                 |                 | N/A                                                                                                                                                        |
   +----------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 5** Response body parameters

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | workload_res_code     | Integer               | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Response code         |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | workload_res_str      | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Response information. |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

.. code-block:: text

   PUT https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/workload/queues/{queue_name}/resources

   {
     "workload_queue" : {
       "workload_queue_name" : "test11",
       "resource_item_list" : [ {
         "resource_name" : "memory",
         "resource_value" : "0"
       }, {
         "resource_name" : "tablespace",
         "resource_value" : "-1"
       }, {
         "resource_name" : "activestatements",
         "resource_value" : "10"
       }, {
         "resource_name" : "cpu_limit",
         "resource_value" : 0
       }, {
         "resource_name" : "cpu_share",
         "resource_value" : 12
       } ],
       "logical_cluster_name" : ""
     }
   }

Example Responses
-----------------

**Status code: 200**

Resource configurations of the resource pool modified.

.. code-block::

   {
     "workload_res_code" : 0,
     "workload_res_str" : ""
   }

Status Codes
------------

=========== ======================================================
Status Code Description
=========== ======================================================
200         Resource configurations of the resource pool modified.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ======================================================
