:original_name: DeleteWorkloadQueue.html

.. _DeleteWorkloadQueue:

Deleting a Resource Pool
========================

Function
--------

This API is used to delete a resource pool.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

DELETE /v2/{project_id}/clusters/{cluster_id}/workload/queues

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
   | workload_queue_name  | Yes             | String          | **Definition**                                                         |
   |                      |                 |                 |                                                                        |
   |                      |                 |                 | Resource pool name.                                                    |
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

   DELETE https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/workload/queues

Example Responses
-----------------

**Status code: 200**

Resource pool deleted.

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
200         Resource pool deleted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
