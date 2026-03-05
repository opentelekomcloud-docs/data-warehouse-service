:original_name: ListAlarmSubs.html

.. _ListAlarmSubs:

Querying the Alarm Subscription List
====================================

Function
--------

This API is used to query subscribed alarms.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/alarm-subs

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
   |                 |                 |                 | 1000                                                    |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------+
   | Parameter             | Type                                                                                                                                              | Description                          |
   +=======================+===================================================================================================================================================+======================================+
   | count                 | Integer                                                                                                                                           | **Definition**                       |
   |                       |                                                                                                                                                   |                                      |
   |                       |                                                                                                                                                   | Total number of alarm subscriptions. |
   |                       |                                                                                                                                                   |                                      |
   |                       |                                                                                                                                                   | **Range**                            |
   |                       |                                                                                                                                                   |                                      |
   |                       |                                                                                                                                                   | N/A                                  |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------+
   | alarm_subscriptions   | Array of :ref:`AlarmSubscriptionResponse <en-us_topic_0000002532013999__en-us_topic_0000002340937708_response_alarmsubscriptionresponse>` objects | **Definition**                       |
   |                       |                                                                                                                                                   |                                      |
   |                       |                                                                                                                                                   | Alarm subscription list.             |
   |                       |                                                                                                                                                   |                                      |
   |                       |                                                                                                                                                   | **Range**                            |
   |                       |                                                                                                                                                   |                                      |
   |                       |                                                                                                                                                   | N/A                                  |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------+

.. _en-us_topic_0000002532013999__en-us_topic_0000002340937708_response_alarmsubscriptionresponse:

.. table:: **Table 4** AlarmSubscriptionResponse

   +--------------------------+-----------------------+-------------------------------------+
   | Parameter                | Type                  | Description                         |
   +==========================+=======================+=====================================+
   | id                       | String                | **Definition**                      |
   |                          |                       |                                     |
   |                          |                       | Alarm subscription ID.              |
   |                          |                       |                                     |
   |                          |                       | **Range**                           |
   |                          |                       |                                     |
   |                          |                       | N/A                                 |
   +--------------------------+-----------------------+-------------------------------------+
   | name                     | String                | **Definition**                      |
   |                          |                       |                                     |
   |                          |                       | Subscribed alarm name.              |
   |                          |                       |                                     |
   |                          |                       | **Range**                           |
   |                          |                       |                                     |
   |                          |                       | N/A                                 |
   +--------------------------+-----------------------+-------------------------------------+
   | enable                   | Integer               | **Definition**                      |
   |                          |                       |                                     |
   |                          |                       | Whether to enable subscription.     |
   |                          |                       |                                     |
   |                          |                       | **Range**                           |
   |                          |                       |                                     |
   |                          |                       | N/A                                 |
   +--------------------------+-----------------------+-------------------------------------+
   | alarm_level              | String                | **Definition**                      |
   |                          |                       |                                     |
   |                          |                       | Alarm severity.                     |
   |                          |                       |                                     |
   |                          |                       | **Range**                           |
   |                          |                       |                                     |
   |                          |                       | N/A                                 |
   +--------------------------+-----------------------+-------------------------------------+
   | project_id               | String                | **Definition**                      |
   |                          |                       |                                     |
   |                          |                       | Project ID.                         |
   |                          |                       |                                     |
   |                          |                       | **Range**                           |
   |                          |                       |                                     |
   |                          |                       | N/A                                 |
   +--------------------------+-----------------------+-------------------------------------+
   | name_space               | String                | **Definition**                      |
   |                          |                       |                                     |
   |                          |                       | Service to which the alarm belongs. |
   |                          |                       |                                     |
   |                          |                       | **Range**                           |
   |                          |                       |                                     |
   |                          |                       | N/A                                 |
   +--------------------------+-----------------------+-------------------------------------+
   | notification_target      | String                | **Definition**                      |
   |                          |                       |                                     |
   |                          |                       | Message topic address.              |
   |                          |                       |                                     |
   |                          |                       | **Range**                           |
   |                          |                       |                                     |
   |                          |                       | N/A                                 |
   +--------------------------+-----------------------+-------------------------------------+
   | notification_target_name | String                | **Definition**                      |
   |                          |                       |                                     |
   |                          |                       | Message topic name.                 |
   |                          |                       |                                     |
   |                          |                       | **Range**                           |
   |                          |                       |                                     |
   |                          |                       | N/A                                 |
   +--------------------------+-----------------------+-------------------------------------+
   | notification_target_type | String                | **Definition**                      |
   |                          |                       |                                     |
   |                          |                       | Message topic type.                 |
   |                          |                       |                                     |
   |                          |                       | **Range**                           |
   |                          |                       |                                     |
   |                          |                       | N/A                                 |
   +--------------------------+-----------------------+-------------------------------------+
   | language                 | String                | **Definition**                      |
   |                          |                       |                                     |
   |                          |                       | Language.                           |
   |                          |                       |                                     |
   |                          |                       | **Range**                           |
   |                          |                       |                                     |
   |                          |                       | N/A                                 |
   +--------------------------+-----------------------+-------------------------------------+
   | time_zone                | String                | **Definition**                      |
   |                          |                       |                                     |
   |                          |                       | Time zone.                          |
   |                          |                       |                                     |
   |                          |                       | **Range**                           |
   |                          |                       |                                     |
   |                          |                       | N/A                                 |
   +--------------------------+-----------------------+-------------------------------------+

Example Requests
----------------

.. code-block::

   https://{Endpoint}/v2/4cf650fd46704908aa071b4df2453e1e/alarm-subs

Example Responses
-----------------

**Status code: 200**

Alarm subscription list queried.

.. code-block::

   {
     "count" : 1,
     "alarm_subscriptions" : [ {
       "id" : "e8d8359f-b8bd-4b80-bc4d-32c86c7c725e",
       "name" : "00",
       "enable" : 1,
       "language" : "zh-cn",
       "alarm_level" : "urgent,important,minor,prompt",
       "project_id" : "4cf650fd46704908aa071b4df2453e1e",
       "name_space" : "DWS",
       "notification_target" : "urn:smn:eu-de-01:4cf650fd46704908aa071b4df2453e1e:CGS",
       "notification_target_name" : "CGS",
       "notification_target_type" : "SMN",
       "time_zone" : "GMT+08:00"
     } ]
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Alarm subscription list queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
