:original_name: ListJobDetails.html

.. _ListJobDetails:

Querying the Task Progress
==========================

Function
--------

This API is used to query the task progress.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1.0/{project_id}/job/{job_id}

.. table:: **Table 1** Path Parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                       |
   +=================+=================+=================+===================================================================================+
   | project_id      | Yes             | String          | **Definition**                                                                    |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | Project ID. To obtain the value, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Constraints**                                                                   |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Range**                                                                         |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Default Value**                                                                 |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------+
   | job_id          | Yes             | String          | **Definition**                                                                    |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | Task ID.                                                                          |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Constraints**                                                                   |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Range**                                                                         |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Default Value**                                                                 |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 2** Response body parameters

   +-----------------------+-----------------------+-------------------------------+
   | Parameter             | Type                  | Description                   |
   +=======================+=======================+===============================+
   | job_id                | String                | **Definition**                |
   |                       |                       |                               |
   |                       |                       | Task ID.                      |
   |                       |                       |                               |
   |                       |                       | **Range**                     |
   |                       |                       |                               |
   |                       |                       | N/A                           |
   +-----------------------+-----------------------+-------------------------------+
   | job_name              | String                | **Definition**                |
   |                       |                       |                               |
   |                       |                       | Task name.                    |
   |                       |                       |                               |
   |                       |                       | **Range**                     |
   |                       |                       |                               |
   |                       |                       | N/A                           |
   +-----------------------+-----------------------+-------------------------------+
   | begin_time            | String                | **Definition**                |
   |                       |                       |                               |
   |                       |                       | Task start time.              |
   |                       |                       |                               |
   |                       |                       | **Range**                     |
   |                       |                       |                               |
   |                       |                       | N/A                           |
   +-----------------------+-----------------------+-------------------------------+
   | end_time              | String                | **Definition**                |
   |                       |                       |                               |
   |                       |                       | Task end time.                |
   |                       |                       |                               |
   |                       |                       | **Range**                     |
   |                       |                       |                               |
   |                       |                       | N/A                           |
   +-----------------------+-----------------------+-------------------------------+
   | status                | String                | **Definition**                |
   |                       |                       |                               |
   |                       |                       | Current task status.          |
   |                       |                       |                               |
   |                       |                       | **Range**                     |
   |                       |                       |                               |
   |                       |                       | N/A                           |
   +-----------------------+-----------------------+-------------------------------+
   | failed_code           | String                | **Definition**                |
   |                       |                       |                               |
   |                       |                       | Error code of a task failure. |
   |                       |                       |                               |
   |                       |                       | **Range**                     |
   |                       |                       |                               |
   |                       |                       | N/A                           |
   +-----------------------+-----------------------+-------------------------------+
   | failed_detail         | String                | **Definition**                |
   |                       |                       |                               |
   |                       |                       | Task failure details.         |
   |                       |                       |                               |
   |                       |                       | **Range**                     |
   |                       |                       |                               |
   |                       |                       | N/A                           |
   +-----------------------+-----------------------+-------------------------------+
   | progress              | String                | **Definition**                |
   |                       |                       |                               |
   |                       |                       | Task progress.                |
   |                       |                       |                               |
   |                       |                       | **Range**                     |
   |                       |                       |                               |
   |                       |                       | N/A                           |
   +-----------------------+-----------------------+-------------------------------+

Example Requests
----------------

.. code-block::

   https://{Endpoint}/v1.0/05f2cff45100d5112f4bc00b794ea08e/job/2c9080e8845b207101845b245e1e0001

Example Responses
-----------------

**Status code: 200**

Task progress queried.

.. code-block::

   {
     "status" : "FAIL",
     "progress" : "9%",
     "job_id" : "2c9080e88459fa44018459fbeb600001",
     "job_name" : "ecfClusterElbCreateJob",
     "begin_time" : "2022-11-09T20:25:00",
     "end_time" : "2022-11-09T20:30:00",
     "failed_code" : "CreateELBTask-fail:DWS.0114",
     "failed_detail" : "DWS.0114:ELB private IP is not configured."
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Task progress queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
