:original_name: ListAuditLog.html

.. _ListAuditLog:

Querying Log Records
====================

Function
--------

This API is used to query audit logs.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1.0/{project_id}/clusters/{cluster_id}/audit-log-records

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

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | Parameter             | Type                                                                                                                          | Description            |
   +=======================+===============================================================================================================================+========================+
   | records               | Array of :ref:`AuditDumpRecord <en-us_topic_0000002500174104__en-us_topic_0000002340937888_response_auditdumprecord>` objects | **Definition**         |
   |                       |                                                                                                                               |                        |
   |                       |                                                                                                                               | Audit log list.        |
   |                       |                                                                                                                               |                        |
   |                       |                                                                                                                               | **Range**              |
   |                       |                                                                                                                               |                        |
   |                       |                                                                                                                               | N/A                    |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | cluster_id            | String                                                                                                                        | **Definition**         |
   |                       |                                                                                                                               |                        |
   |                       |                                                                                                                               | Cluster ID.            |
   |                       |                                                                                                                               |                        |
   |                       |                                                                                                                               | **Range**              |
   |                       |                                                                                                                               |                        |
   |                       |                                                                                                                               | It is a 36-digit UUID. |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | count                 | Integer                                                                                                                       | **Definition**         |
   |                       |                                                                                                                               |                        |
   |                       |                                                                                                                               | Total number.          |
   |                       |                                                                                                                               |                        |
   |                       |                                                                                                                               | **Range**              |
   |                       |                                                                                                                               |                        |
   |                       |                                                                                                                               | N/A                    |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------+------------------------+

.. _en-us_topic_0000002500174104__en-us_topic_0000002340937888_response_auditdumprecord:

.. table:: **Table 3** AuditDumpRecord

   +-----------------------+-----------------------+------------------------+
   | Parameter             | Type                  | Description            |
   +=======================+=======================+========================+
   | cluster_id            | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Cluster ID.            |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | It is a 36-digit UUID. |
   +-----------------------+-----------------------+------------------------+
   | executor_time         | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Execution time.        |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | begin_time            | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Start time.            |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | end_time              | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | End time.              |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | bucket_name           | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Bucket name.           |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | location_prefix       | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Prefix.                |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | result                | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Results.               |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | failed_reason         | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Failure cause.         |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+

Example Requests
----------------

.. code-block:: text

   GET https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/audit-log-records

Example Responses
-----------------

**Status code: 200**

Query succeeded.

.. code-block::

   {
     "records" : [ {
       "result" : "RUNNING",
       "cluster_id" : "a07cb2f7-b17e-4d95-923b-a33d0c884d37",
       "executor_time" : "2022-10-31T09:11:31",
       "begin_time" : "2022-10-31T09:09:55",
       "end_time" : "2022-10-31T09:19:55",
       "bucket_name" : "testbucket",
       "location_prefix" : "test"
     } ],
     "count" : 1,
     "cluster_id" : "a07cb2f7-b17e-4d95-923b-a33d0c884d37"
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
