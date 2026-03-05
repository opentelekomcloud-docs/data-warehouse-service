:original_name: ListClusterWorkload.html

.. _ListClusterWorkload:

Querying Whether Resource Management Is Enabled
===============================================

Function
--------

This API is used to query whether resource management is enabled.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/clusters/{cluster_id}/workload

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

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 2** Response body parameters

   +-----------------------+-------------------------------------------------------------------------------------------------------------------+----------------------------+
   | Parameter             | Type                                                                                                              | Description                |
   +=======================+===================================================================================================================+============================+
   | workload_res_code     | Integer                                                                                                           | **Definition**             |
   |                       |                                                                                                                   |                            |
   |                       |                                                                                                                   | Result status code.        |
   |                       |                                                                                                                   |                            |
   |                       |                                                                                                                   | **Range**                  |
   |                       |                                                                                                                   |                            |
   |                       |                                                                                                                   | N/A                        |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------+----------------------------+
   | workload_res_str      | String                                                                                                            | **Definition**             |
   |                       |                                                                                                                   |                            |
   |                       |                                                                                                                   | Result description.        |
   |                       |                                                                                                                   |                            |
   |                       |                                                                                                                   | **Range**                  |
   |                       |                                                                                                                   |                            |
   |                       |                                                                                                                   | N/A                        |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------+----------------------------+
   | workload_status       | :ref:`WorkloadStatus <en-us_topic_0000002531894001__en-us_topic_0000002340937184_response_workloadstatus>` object | **Definition**             |
   |                       |                                                                                                                   |                            |
   |                       |                                                                                                                   | Resource management status |
   |                       |                                                                                                                   |                            |
   |                       |                                                                                                                   | **Range**                  |
   |                       |                                                                                                                   |                            |
   |                       |                                                                                                                   | N/A                        |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------+----------------------------+

.. _en-us_topic_0000002531894001__en-us_topic_0000002340937184_response_workloadstatus:

.. table:: **Table 3** WorkloadStatus

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | workload_switch       | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Switch                |
   |                       |                       |                       |
   |                       |                       | **Constraints**       |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | **on**: enabled       |
   |                       |                       |                       |
   |                       |                       | **off**: disabled     |
   |                       |                       |                       |
   |                       |                       | **Default Value**     |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | max_concurrency_num   | Integer               | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Maximum concurrency   |
   |                       |                       |                       |
   |                       |                       | **Constraints**       |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   |                       |                       |                       |
   |                       |                       | **Default Value**     |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

Query whether resource management is enabled.

.. code-block:: text

   GET https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/workload

Example Responses
-----------------

**Status code: 200**

Query succeeded.

.. code-block::

   {
     "workload_res_code" : 0,
     "workload_res_str" : "Success get workload manager status.",
     "workload_status" : {
       "workload_switch" : "on",
       "max_concurrency_num" : 60
     }
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Query succeeded.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
