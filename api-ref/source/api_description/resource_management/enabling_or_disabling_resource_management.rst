:original_name: CreateClusterWorkload.html

.. _CreateClusterWorkload:

Enabling or Disabling Resource Management
=========================================

Function
--------

This API is used to enable or disable resource management. The function is enabled by default for new clusters.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v2/{project_id}/clusters/{cluster_id}/workload

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

   +-----------------+-----------------+------------------------------------------------------------------------------------------------------------------+----------------------------------------+
   | Parameter       | Mandatory       | Type                                                                                                             | Description                            |
   +=================+=================+==================================================================================================================+========================================+
   | workload_status | No              | :ref:`WorkloadStatus <en-us_topic_0000002531893883__en-us_topic_0000002374935293_request_workloadstatus>` object | **Definition**                         |
   |                 |                 |                                                                                                                  |                                        |
   |                 |                 |                                                                                                                  | Request for resource management status |
   |                 |                 |                                                                                                                  |                                        |
   |                 |                 |                                                                                                                  | **Constraints**                        |
   |                 |                 |                                                                                                                  |                                        |
   |                 |                 |                                                                                                                  | N/A                                    |
   |                 |                 |                                                                                                                  |                                        |
   |                 |                 |                                                                                                                  | **Range**                              |
   |                 |                 |                                                                                                                  |                                        |
   |                 |                 |                                                                                                                  | N/A                                    |
   |                 |                 |                                                                                                                  |                                        |
   |                 |                 |                                                                                                                  | **Default Value**                      |
   |                 |                 |                                                                                                                  |                                        |
   |                 |                 |                                                                                                                  | N/A                                    |
   +-----------------+-----------------+------------------------------------------------------------------------------------------------------------------+----------------------------------------+

.. _en-us_topic_0000002531893883__en-us_topic_0000002374935293_request_workloadstatus:

.. table:: **Table 3** WorkloadStatus

   +---------------------+-----------------+-----------------+---------------------+
   | Parameter           | Mandatory       | Type            | Description         |
   +=====================+=================+=================+=====================+
   | workload_switch     | Yes             | String          | **Definition**      |
   |                     |                 |                 |                     |
   |                     |                 |                 | Switch              |
   |                     |                 |                 |                     |
   |                     |                 |                 | **Constraints**     |
   |                     |                 |                 |                     |
   |                     |                 |                 | N/A                 |
   |                     |                 |                 |                     |
   |                     |                 |                 | **Range**           |
   |                     |                 |                 |                     |
   |                     |                 |                 | **on**: enabled     |
   |                     |                 |                 |                     |
   |                     |                 |                 | **off**: disabled   |
   |                     |                 |                 |                     |
   |                     |                 |                 | **Default Value**   |
   |                     |                 |                 |                     |
   |                     |                 |                 | N/A                 |
   +---------------------+-----------------+-----------------+---------------------+
   | max_concurrency_num | No              | Integer         | **Definition**      |
   |                     |                 |                 |                     |
   |                     |                 |                 | Maximum concurrency |
   |                     |                 |                 |                     |
   |                     |                 |                 | **Constraints**     |
   |                     |                 |                 |                     |
   |                     |                 |                 | N/A                 |
   |                     |                 |                 |                     |
   |                     |                 |                 | **Range**           |
   |                     |                 |                 |                     |
   |                     |                 |                 | N/A                 |
   |                     |                 |                 |                     |
   |                     |                 |                 | **Default Value**   |
   |                     |                 |                 |                     |
   |                     |                 |                 | N/A                 |
   +---------------------+-----------------+-----------------+---------------------+

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

Enable resource management and set the maximum number of concurrent tasks to 5.

.. code-block:: text

   POST https://{Endpoint} /v2/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/workload

Example Responses
-----------------

**Status code: 200**

Resource management configured.

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
200         Resource management configured.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
