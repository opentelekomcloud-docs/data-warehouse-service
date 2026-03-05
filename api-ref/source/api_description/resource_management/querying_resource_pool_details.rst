:original_name: ShowWorkloadQueue.html

.. _ShowWorkloadQueue:

Querying Resource Pool Details
==============================

Function
--------

This API is used to query details about a resource pool.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/clusters/{cluster_id}/workload/queues/{queue_name}

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

.. table:: **Table 2** Query Parameters

   +----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter            | Mandatory       | Type            | Description                                                                                                                                                         |
   +======================+=================+=================+=====================================================================================================================================================================+
   | logical_cluster_name | No              | String          | **Definition**                                                                                                                                                      |
   |                      |                 |                 |                                                                                                                                                                     |
   |                      |                 |                 | Logical cluster name. In non-logical cluster mode, this parameter is left blank. In logical cluster mode, you need to set this parameter to a logical cluster name. |
   |                      |                 |                 |                                                                                                                                                                     |
   |                      |                 |                 | **Constraints**                                                                                                                                                     |
   |                      |                 |                 |                                                                                                                                                                     |
   |                      |                 |                 | N/A                                                                                                                                                                 |
   |                      |                 |                 |                                                                                                                                                                     |
   |                      |                 |                 | **Range**                                                                                                                                                           |
   |                      |                 |                 |                                                                                                                                                                     |
   |                      |                 |                 | N/A                                                                                                                                                                 |
   |                      |                 |                 |                                                                                                                                                                     |
   |                      |                 |                 | **Default Value**                                                                                                                                                   |
   |                      |                 |                 |                                                                                                                                                                     |
   |                      |                 |                 | N/A                                                                                                                                                                 |
   +----------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | Parameter             | Type                                                                                                                    | Description             |
   +=======================+=========================================================================================================================+=========================+
   | workload_res_code     | Integer                                                                                                                 | **Definition**          |
   |                       |                                                                                                                         |                         |
   |                       |                                                                                                                         | Result status code.     |
   |                       |                                                                                                                         |                         |
   |                       |                                                                                                                         | **Range**               |
   |                       |                                                                                                                         |                         |
   |                       |                                                                                                                         | N/A                     |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | workload_res_str      | String                                                                                                                  | **Definition**          |
   |                       |                                                                                                                         |                         |
   |                       |                                                                                                                         | Result description.     |
   |                       |                                                                                                                         |                         |
   |                       |                                                                                                                         | **Range**               |
   |                       |                                                                                                                         |                         |
   |                       |                                                                                                                         | N/A                     |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | workload_queue        | :ref:`WorkloadQueueItem <en-us_topic_0000002500014062__en-us_topic_0000002375015073_response_workloadqueueitem>` object | **Definition**          |
   |                       |                                                                                                                         |                         |
   |                       |                                                                                                                         | Resource queue details. |
   |                       |                                                                                                                         |                         |
   |                       |                                                                                                                         | **Range**               |
   |                       |                                                                                                                         |                         |
   |                       |                                                                                                                         | N/A                     |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+-------------------------+

.. _en-us_topic_0000002500014062__en-us_topic_0000002375015073_response_workloadqueueitem:

.. table:: **Table 4** WorkloadQueueItem

   +-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------+
   | Parameter                   | Type                                                                                                                                    | Description                                                      |
   +=============================+=========================================================================================================================================+==================================================================+
   | queue_name                  | String                                                                                                                                  | **Definition**                                                   |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | Resource pool name.                                              |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | **Range**                                                        |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | N/A                                                              |
   +-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------+
   | logical_cluster_name        | String                                                                                                                                  | **Definition**                                                   |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | Logical cluster name.                                            |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | **Range**                                                        |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | N/A                                                              |
   +-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------+
   | short_query_optimize        | String                                                                                                                                  | **Definition**                                                   |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | Whether to enable short query acceleration for the resource pool |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | **Range**                                                        |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | N/A                                                              |
   +-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------+
   | short_query_concurrency_num | Integer                                                                                                                                 | **Definition**                                                   |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | Number of concurrent short queries in the resource pool          |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | **Range**                                                        |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | N/A                                                              |
   +-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------+
   | resource_item_list          | Array of :ref:`WorkloadResourceItem <en-us_topic_0000002500014062__en-us_topic_0000002375015073_response_workloadresourceitem>` objects | **Definition**                                                   |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | Resource configuration queue.                                    |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | **Range**                                                        |
   |                             |                                                                                                                                         |                                                                  |
   |                             |                                                                                                                                         | N/A                                                              |
   +-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------+

.. _en-us_topic_0000002500014062__en-us_topic_0000002375015073_response_workloadresourceitem:

.. table:: **Table 5** WorkloadResourceItem

   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Type                  | Description                                                                                                                                                |
   +=======================+=======================+============================================================================================================================================================+
   | resource_name         | String                | **Definition**                                                                                                                                             |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | Resource name.                                                                                                                                             |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Constraints**                                                                                                                                            |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | N/A                                                                                                                                                        |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Range**                                                                                                                                                  |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **cpu**: percentage of CPU time                                                                                                                            |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **cpu_limit**: percentage of CPU cores                                                                                                                     |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **memory**: percentage of available memory resources on each data node                                                                                     |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **concurrency**: number of concurrent queries                                                                                                              |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **shortQueryConcurrencyNum**: number of concurrent simple statements                                                                                       |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **weight**: weight for network scheduling                                                                                                                  |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Default Value**                                                                                                                                          |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | N/A                                                                                                                                                        |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource_value        | Integer               | **Definition**                                                                                                                                             |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | Resource attribute value.                                                                                                                                  |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Constraints**                                                                                                                                            |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | N/A                                                                                                                                                        |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Range**                                                                                                                                                  |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | The value range varies according to the value of **resource_name**.                                                                                        |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | If **resource_name** is **cpu**, the value is an integer from **1** to **99**.                                                                             |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | If **resource_name** is **cpu_limit**, the value is an integer from **0** to **100**. The value **0** indicates no limit.                                  |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | If **resource_name** is **memory**, the value is an integer from **0** to **100**. The value **0** indicates that no limit.                                |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | If **resource_name** is **concurrency**, the value is an integer from **1** to **2147483647**. The value **-1** or **0** indicates no limit.               |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | If **resource_name** is **shortQueryConcurrencyNum**, the value is an integer from **-1** to **2147483647**. The value **-1** or **0** indicates no limit. |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | If **resource_name** is **weight**, the value is an integer from **1** to **2147483647**. The default value is **-1**.                                     |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Default Value**                                                                                                                                          |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | N/A                                                                                                                                                        |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value_unit            | String                | **Definition**                                                                                                                                             |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | Resource attribute unit.                                                                                                                                   |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Constraints**                                                                                                                                            |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | N/A                                                                                                                                                        |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Range**                                                                                                                                                  |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | N/A                                                                                                                                                        |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Default Value**                                                                                                                                          |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | N/A                                                                                                                                                        |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resource_description  | String                | **Definition**                                                                                                                                             |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | Additional resource description.                                                                                                                           |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Constraints**                                                                                                                                            |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | N/A                                                                                                                                                        |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Range**                                                                                                                                                  |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | N/A                                                                                                                                                        |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | **Default Value**                                                                                                                                          |
   |                       |                       |                                                                                                                                                            |
   |                       |                       | N/A                                                                                                                                                        |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Requests
----------------

.. code-block:: text

   GET https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/workload/queues/resource1

Example Responses
-----------------

**Status code: 200**

Resource pool details queried.

.. code-block::

   {
     "workload_queue" : {
       "queue_name" : "resource1",
       "logical_cluster_name" : "",
       "short_query_optimize" : "t",
       "short_query_concurrency_num" : -1,
       "resource_item_list" : [ {
         "resource_name" : "cpu",
         "resource_value" : 1,
         "value_unit" : null,
         "resource_description" : null
       } ]
     }
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Resource pool details queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
