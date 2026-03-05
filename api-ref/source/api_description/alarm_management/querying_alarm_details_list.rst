:original_name: ListAlarmDetail.html

.. _ListAlarmDetail:

Querying Alarm Details List
===========================

Function
--------

This API is used to query the alarm details list.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/alarms

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

.. table:: **Table 2** Query Parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                             |
   +=================+=================+=================+=========================================================+
   | time_zone       | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Time zone.                                              |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | GMT+08:00                                               |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | offset          | No              | String          | **Definition**                                          |
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
   | limit           | No              | String          | **Definition**                                          |
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
   |                 |                 |                 | **10**                                                  |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Parameter             | Type                                                                                                                                  | Description                    |
   +=======================+=======================================================================================================================================+================================+
   | count                 | Integer                                                                                                                               | **Definition**                 |
   |                       |                                                                                                                                       |                                |
   |                       |                                                                                                                                       | Total number of alarm details. |
   |                       |                                                                                                                                       |                                |
   |                       |                                                                                                                                       | **Range**                      |
   |                       |                                                                                                                                       |                                |
   |                       |                                                                                                                                       | N/A                            |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | alarm_details         | Array of :ref:`AlarmDetailResponse <en-us_topic_0000002532013993__en-us_topic_0000002375015517_response_alarmdetailresponse>` objects | **Definition**                 |
   |                       |                                                                                                                                       |                                |
   |                       |                                                                                                                                       | Alarm list.                    |
   |                       |                                                                                                                                       |                                |
   |                       |                                                                                                                                       | **Range**                      |
   |                       |                                                                                                                                       |                                |
   |                       |                                                                                                                                       | N/A                            |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+--------------------------------+

.. _en-us_topic_0000002532013993__en-us_topic_0000002375015517_response_alarmdetailresponse:

.. table:: **Table 4** AlarmDetailResponse

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | alarm_id              | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Alarm definition ID.  |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | alarm_name            | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Alarm name.           |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | alarm_level           | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Alarm severity.       |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | alarm_source          | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Alarm source.         |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | alarm_message         | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Alarm message.        |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | alarm_location        | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Alarm location.       |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | resource_id           | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Alarm source ID.      |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | resource_id_name      | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Alarm source name.    |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | alarm_generate_date   | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Alarm date.           |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | alarm_status          | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Alarm status.         |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

.. code-block::

   https://{Endpoint}/v2/4cf650fd46704908aa071b4df2453e1e/alarms?time_zone=GMT

Example Responses
-----------------

**Status code: 200**

Alarm details list queried.

.. code-block::

   {
     "alarm_details" : [ {
       "alarm_id" : "DWS_01010",
       "alarm_name" : "Cluster Status Fault",
       "alarm_level" : "1",
       "alarm_source" : "DWS",
       "alarm_message" : "CloudService=DWS, resourceId: 5e76e8e2-d0cf-4b64-9d9a-aadbb04b54f7, resourceIdName: evs-09, domain_name=, domain_id=0676610f3a0a4c2c80c50bea7ddf18c1, res_domain_name=op_svc_dws_0676610f3a0a4c2c80c50bea7ddf18c1",
       "alarm_location" : "cluster_id: 5e76e8e2-d0cf-4b64-9d9a-aadbb04b54f7,cluster_name: evs-09,cluster type: dws,domain_name: ,domain_id: 0676610f3a0a4c2c80c50bea7ddf18c1",
       "resource_id" : "5e76e8e2-d0cf-4b64-9d9a-aadbb04b54f7",
       "resource_id_name" : "evs-09",
       "alarm_generate_date" : "2022-10-27 08:11:29",
       "alarm_status" : "0"
     } ],
     "count" : 1
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Alarm details list queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
