:original_name: ListAlarmStatistic.html

.. _ListAlarmStatistic:

Querying the Alarm Statistics List
==================================

Function
--------

This API is used to query alarm statistics.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/alarm-statistic

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

   +-----------------+-----------------+-----------------+-------------------+
   | Parameter       | Mandatory       | Type            | Description       |
   +=================+=================+=================+===================+
   | time_zone       | No              | String          | **Definition**    |
   |                 |                 |                 |                   |
   |                 |                 |                 | Time zone.        |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Constraints**   |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Range**         |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Default Value** |
   |                 |                 |                 |                   |
   |                 |                 |                 | GMT+08:00         |
   +-----------------+-----------------+-----------------+-------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | Parameter             | Type                                                                                                                                        | Description            |
   +=======================+=============================================================================================================================================+========================+
   | alarm_statistics      | Array of :ref:`AlarmStatisticResponse <en-us_topic_0000002531893909__en-us_topic_0000002374935665_response_alarmstatisticresponse>` objects | **Definition**         |
   |                       |                                                                                                                                             |                        |
   |                       |                                                                                                                                             | Alarm statistics list. |
   |                       |                                                                                                                                             |                        |
   |                       |                                                                                                                                             | **Range**              |
   |                       |                                                                                                                                             |                        |
   |                       |                                                                                                                                             | N/A                    |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------+------------------------+

.. _en-us_topic_0000002531893909__en-us_topic_0000002374935665_response_alarmstatisticresponse:

.. table:: **Table 4** AlarmStatisticResponse

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | date                  | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Date.                 |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | urgent                | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Urgent.               |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | important             | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Important.            |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | minor                 | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Minor.                |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | prompt                | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Prompt.               |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

.. code-block::

   https://{Endpoint}/v2/4cf650fd46704908aa071b4df2453e1e/alarm-statistic?time_zone=GMT

Example Responses
-----------------

**Status code: 200**

Alarm statistics list queried.

.. code-block::

   {
     "alarm_statistics" : [ {
       "date" : "2022-10-21",
       "urgent" : 0,
       "important" : 0,
       "minor" : 0,
       "prompt" : 0
     }, {
       "date" : "2022-10-22",
       "urgent" : 0,
       "important" : 0,
       "minor" : 0,
       "prompt" : 0
     }, {
       "date" : "2022-10-23",
       "urgent" : 0,
       "important" : 0,
       "minor" : 0,
       "prompt" : 0
     }, {
       "date" : "2022-10-24",
       "urgent" : 0,
       "important" : 0,
       "minor" : 0,
       "prompt" : 0
     }, {
       "date" : "2022-10-25",
       "urgent" : 0,
       "important" : 0,
       "minor" : 0,
       "prompt" : 0
     }, {
       "date" : "2022-10-26",
       "urgent" : 0,
       "important" : 0,
       "minor" : 0,
       "prompt" : 0
     }, {
       "date" : "2022-10-27",
       "urgent" : 17,
       "important" : 0,
       "minor" : 0,
       "prompt" : 0
     } ]
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Alarm statistics list queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
