:original_name: AddQueueUserList.html

.. _AddQueueUserList:

Associating a User to a Resource Pool
=====================================

Function
--------

This API is used to associate a user to a resource pool.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v2/{project_id}/clusters/{cluster_id}/workload/queues/{queue_name}/users/batch-create

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

   +-----------------+-----------------+------------------------------------------------------------------------------------------------------------------+------------------------------+
   | Parameter       | Mandatory       | Type                                                                                                             | Description                  |
   +=================+=================+==================================================================================================================+==============================+
   | queue_name      | Yes             | String                                                                                                           | **Definition**               |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | Resource pool name.          |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | **Constraints**              |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | N/A                          |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | **Range**                    |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | N/A                          |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | **Default Value**            |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | N/A                          |
   +-----------------+-----------------+------------------------------------------------------------------------------------------------------------------+------------------------------+
   | user_list       | Yes             | Array of :ref:`user_list <en-us_topic_0000002532013901__en-us_topic_0000002341097044_request_user_list>` objects | **Definition**               |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | List of resource pool users. |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | **Constraints**              |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | N/A                          |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | **Range**                    |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | N/A                          |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | **Default Value**            |
   |                 |                 |                                                                                                                  |                              |
   |                 |                 |                                                                                                                  | N/A                          |
   +-----------------+-----------------+------------------------------------------------------------------------------------------------------------------+------------------------------+

.. _en-us_topic_0000002532013901__en-us_topic_0000002341097044_request_user_list:

.. table:: **Table 3** user_list

   ========= ========= ====== ===========
   Parameter Mandatory Type   Description
   ========= ========= ====== ===========
   user_name No        String Username
   ========= ========= ====== ===========

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 4** Response body parameters

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

   POST https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/workload/queues/resource1/users/batch-create

   {
     "queue_name" : "test11",
     "user_list" : [ {
       "user_name" : "user_batch"
     } ]
   }

Example Responses
-----------------

**Status code: 200**

The user is associated to the resource pool.

.. code-block::

   {
     "workload_res_code" : 0,
     "workload_res_str" : ""
   }

Status Codes
------------

=========== ============================================
Status Code Description
=========== ============================================
200         The user is associated to the resource pool.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ============================================
