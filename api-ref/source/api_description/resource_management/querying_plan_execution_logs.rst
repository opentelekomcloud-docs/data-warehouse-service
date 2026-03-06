:original_name: ListPlanExecLogs.html

.. _ListPlanExecLogs:

Querying Plan Execution Logs
============================

Function
--------

This API is used to query plan execution logs.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/clusters/{cluster_id}/workload/plans/{plan_id}/logs

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
   | plan_id         | Yes             | String          | **Definition**                                                                                             |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | Plan ID.                                                                                                   |
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

   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                             |
   +=================+=================+=================+=========================================================+
   | limit           | No              | Integer         | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Size of a single page.                                  |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Greater than 0                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | 10                                                      |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | offset          | No              | Integer         | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Page offset, which starts from 0 (page number minus 1). |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Greater than or equal to **0**                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | 0                                                       |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+
   | Parameter             | Type                                                                                                          | Description           |
   +=======================+===============================================================================================================+=======================+
   | workload_res_code     | Integer                                                                                                       | **Definition**        |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | Result status code.   |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | **Range**             |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | N/A                   |
   +-----------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+
   | workload_res_str      | String                                                                                                        | **Definition**        |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | Result description.   |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | **Range**             |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | N/A                   |
   +-----------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+
   | plan_logs             | Array of :ref:`PlanLog <en-us_topic_0000002532013897__en-us_topic_0000002375015093_response_planlog>` objects | **Definition**        |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | Resource pool name.   |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | **Range**             |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | N/A                   |
   +-----------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+
   | count                 | Integer                                                                                                       | **Definition**        |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | Total number.         |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | **Range**             |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | N/A                   |
   +-----------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+

.. _en-us_topic_0000002532013897__en-us_topic_0000002375015093_response_planlog:

.. table:: **Table 4** PlanLog

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | exec_time             | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Execution time.       |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | stage_info            | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Plan execution stage. |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | exec_result           | Integer               | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Execution result.     |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | exec_log              | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Execution log.        |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

.. code-block:: text

   GET https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/workload/plans/0c2145ad-4d76-4abe-bd1b-cdbe9128478a/logs

Example Responses
-----------------

**Status code: 200**

Query succeeded.

.. code-block::

   {
     "plan_logs" : [ {
       "exec_time" : "2023-08-23 13:28:50",
       "stage_info" : "stage1",
       "exec_result" : 0,
       "exec_log" : "2023-08-23 13:28:47.661892+00:00 UTC |INFO| start change stage."
     } ],
     "count" : 1
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
