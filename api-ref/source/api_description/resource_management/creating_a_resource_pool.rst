:original_name: AddWorkloadQueue.html

.. _AddWorkloadQueue:

Creating a Resource Pool
========================

Function
--------

This API is used to create a resource pool.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

PUT /v2/{project_id}/clusters/{cluster_id}/workload/queues

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

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+----------------------------------------------------------------------------------------------------------------+----------------------------+
   | Parameter       | Mandatory       | Type                                                                                                           | Description                |
   +=================+=================+================================================================================================================+============================+
   | workload_queue  | Yes             | :ref:`WorkloadQueue <en-us_topic_0000002500013998__en-us_topic_0000002340937204_request_workloadqueue>` object | **Definition**             |
   |                 |                 |                                                                                                                |                            |
   |                 |                 |                                                                                                                | Resource pool information. |
   |                 |                 |                                                                                                                |                            |
   |                 |                 |                                                                                                                | **Constraints**            |
   |                 |                 |                                                                                                                |                            |
   |                 |                 |                                                                                                                | N/A                        |
   |                 |                 |                                                                                                                |                            |
   |                 |                 |                                                                                                                | **Range**                  |
   |                 |                 |                                                                                                                |                            |
   |                 |                 |                                                                                                                | N/A                        |
   |                 |                 |                                                                                                                |                            |
   |                 |                 |                                                                                                                | **Default Value**          |
   |                 |                 |                                                                                                                |                            |
   |                 |                 |                                                                                                                | N/A                        |
   +-----------------+-----------------+----------------------------------------------------------------------------------------------------------------+----------------------------+

.. _en-us_topic_0000002500013998__en-us_topic_0000002340937204_request_workloadqueue:

.. table:: **Table 3** WorkloadQueue

   +-----------------------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------+-------------------------------+
   | Parameter                   | Mandatory       | Type                                                                                                                           | Description                   |
   +=============================+=================+================================================================================================================================+===============================+
   | workload_queue_name         | Yes             | String                                                                                                                         | **Definition**                |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | Resource pool name.           |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | **Constraints**               |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | N/A                           |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | **Range**                     |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | N/A                           |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | **Default Value**             |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | N/A                           |
   +-----------------------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------+-------------------------------+
   | logical_cluster_name        | No              | String                                                                                                                         | **Definition**                |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | Logical cluster name.         |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | **Constraints**               |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | N/A                           |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | **Range**                     |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | N/A                           |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | **Default Value**             |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | N/A                           |
   +-----------------------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------+-------------------------------+
   | workload_resource_item_list | Yes             | Array of :ref:`WorkloadResource <en-us_topic_0000002500013998__en-us_topic_0000002340937204_request_workloadresource>` objects | **Definition**                |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | Resource configuration queue. |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | **Constraints**               |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | N/A                           |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | **Range**                     |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | N/A                           |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | **Default Value**             |
   |                             |                 |                                                                                                                                |                               |
   |                             |                 |                                                                                                                                | N/A                           |
   +-----------------------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------+-------------------------------+

.. _en-us_topic_0000002500013998__en-us_topic_0000002340937204_request_workloadresource:

.. table:: **Table 4** WorkloadResource

   +-----------------+-----------------+-----------------+---------------------------+
   | Parameter       | Mandatory       | Type            | Description               |
   +=================+=================+=================+===========================+
   | resource_name   | Yes             | String          | **Definition**            |
   |                 |                 |                 |                           |
   |                 |                 |                 | Resource name.            |
   |                 |                 |                 |                           |
   |                 |                 |                 | **Constraints**           |
   |                 |                 |                 |                           |
   |                 |                 |                 | N/A                       |
   |                 |                 |                 |                           |
   |                 |                 |                 | **Range**                 |
   |                 |                 |                 |                           |
   |                 |                 |                 | N/A                       |
   |                 |                 |                 |                           |
   |                 |                 |                 | **Default Value**         |
   |                 |                 |                 |                           |
   |                 |                 |                 | N/A                       |
   +-----------------+-----------------+-----------------+---------------------------+
   | resource_value  | Yes             | Integer         | **Definition**            |
   |                 |                 |                 |                           |
   |                 |                 |                 | Resource attribute value. |
   |                 |                 |                 |                           |
   |                 |                 |                 | **Constraints**           |
   |                 |                 |                 |                           |
   |                 |                 |                 | N/A                       |
   |                 |                 |                 |                           |
   |                 |                 |                 | **Range**                 |
   |                 |                 |                 |                           |
   |                 |                 |                 | N/A                       |
   |                 |                 |                 |                           |
   |                 |                 |                 | **Default Value**         |
   |                 |                 |                 |                           |
   |                 |                 |                 | N/A                       |
   +-----------------+-----------------+-----------------+---------------------------+

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

Create a resource pool (Name: **test11**. CPU share: **12%**. Memory resource: **0** (no limit). Storage resource: **-1** (no limit). Concurrent: **10**).

.. code-block:: text

   PUT https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/workload/queues

Example Responses
-----------------

**Status code: 200**

Operation succeeded.

.. code-block::

   {
     "workload_res_code" : 0,
     "workload_res_str" : ""
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Operation succeeded.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
