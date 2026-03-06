:original_name: ListWorkloadQueue.html

.. _ListWorkloadQueue:

Querying Resource Pools
=======================

Function
--------

This API is used to query resource pools.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/clusters/{cluster_id}/workload/queues

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

.. table:: **Table 2** Query Parameters

   +----------------------+-----------------+-----------------+------------------------------------------------------------------------+
   | Parameter            | Mandatory       | Type            | Description                                                            |
   +======================+=================+=================+========================================================================+
   | logical_cluster_name | No              | String          | **Definition**                                                         |
   |                      |                 |                 |                                                                        |
   |                      |                 |                 | Logical cluster name. This field is mandatory in logical cluster mode. |
   |                      |                 |                 |                                                                        |
   |                      |                 |                 | **Constraints**                                                        |
   |                      |                 |                 |                                                                        |
   |                      |                 |                 | N/A                                                                    |
   |                      |                 |                 |                                                                        |
   |                      |                 |                 | **Range**                                                              |
   |                      |                 |                 |                                                                        |
   |                      |                 |                 | N/A                                                                    |
   |                      |                 |                 |                                                                        |
   |                      |                 |                 | **Default Value**                                                      |
   |                      |                 |                 |                                                                        |
   |                      |                 |                 | N/A                                                                    |
   +----------------------+-----------------+-----------------+------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +--------------------------+-----------------------+-----------------------+
   | Parameter                | Type                  | Description           |
   +==========================+=======================+=======================+
   | workload_queue_name_list | Array of strings      | **Definition**        |
   |                          |                       |                       |
   |                          |                       | Resource pool name.   |
   |                          |                       |                       |
   |                          |                       | **Range**             |
   |                          |                       |                       |
   |                          |                       | N/A                   |
   +--------------------------+-----------------------+-----------------------+
   | workload_res_code        | Integer               | **Definition**        |
   |                          |                       |                       |
   |                          |                       | Result status code.   |
   |                          |                       |                       |
   |                          |                       | **Range**             |
   |                          |                       |                       |
   |                          |                       | N/A                   |
   +--------------------------+-----------------------+-----------------------+
   | workload_res_str         | String                | **Definition**        |
   |                          |                       |                       |
   |                          |                       | Result description.   |
   |                          |                       |                       |
   |                          |                       | **Range**             |
   |                          |                       |                       |
   |                          |                       | N/A                   |
   +--------------------------+-----------------------+-----------------------+

Example Requests
----------------

Query resource pools.

.. code-block:: text

   GET https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/workload/queues

Example Responses
-----------------

**Status code: 200**

Resource pools queried.

.. code-block::

   {
     "workload_queue_name_list" : [ "test1" ]
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Resource pools queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
