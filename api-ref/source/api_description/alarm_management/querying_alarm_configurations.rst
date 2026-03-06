:original_name: ListAlarmConfigs.html

.. _ListAlarmConfigs:

Querying Alarm Configurations
=============================

Function
--------

This API is used to query alarm configurations.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/alarm-configs

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
   |                 |                 |                 | No limit.                                               |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | Parameter             | Type                                                                                                                                  | Description                           |
   +=======================+=======================================================================================================================================+=======================================+
   | count                 | Integer                                                                                                                               | **Definition**                        |
   |                       |                                                                                                                                       |                                       |
   |                       |                                                                                                                                       | Total number of alarm configurations. |
   |                       |                                                                                                                                       |                                       |
   |                       |                                                                                                                                       | **Range**                             |
   |                       |                                                                                                                                       |                                       |
   |                       |                                                                                                                                       | N/A                                   |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | alarm_configs         | Array of :ref:`AlarmConfigResponse <en-us_topic_0000002500014036__en-us_topic_0000002341097432_response_alarmconfigresponse>` objects | **Definition**                        |
   |                       |                                                                                                                                       |                                       |
   |                       |                                                                                                                                       | Alarm configuration list.             |
   |                       |                                                                                                                                       |                                       |
   |                       |                                                                                                                                       | **Range**                             |
   |                       |                                                                                                                                       |                                       |
   |                       |                                                                                                                                       | N/A                                   |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+

.. _en-us_topic_0000002500014036__en-us_topic_0000002341097432_response_alarmconfigresponse:

.. table:: **Table 4** AlarmConfigResponse

   +-----------------------+-----------------------+------------------------------------------------+
   | Parameter             | Type                  | Description                                    |
   +=======================+=======================+================================================+
   | id                    | String                | **Definition**                                 |
   |                       |                       |                                                |
   |                       |                       | Alarm configuration ID.                        |
   |                       |                       |                                                |
   |                       |                       | **Range**                                      |
   |                       |                       |                                                |
   |                       |                       | N/A                                            |
   +-----------------------+-----------------------+------------------------------------------------+
   | alarm_id              | String                | **Definition**                                 |
   |                       |                       |                                                |
   |                       |                       | Alarm ID.                                      |
   |                       |                       |                                                |
   |                       |                       | **Range**                                      |
   |                       |                       |                                                |
   |                       |                       | N/A                                            |
   +-----------------------+-----------------------+------------------------------------------------+
   | alarm_name            | String                | **Definition**                                 |
   |                       |                       |                                                |
   |                       |                       | Alarm name.                                    |
   |                       |                       |                                                |
   |                       |                       | **Range**                                      |
   |                       |                       |                                                |
   |                       |                       | N/A                                            |
   +-----------------------+-----------------------+------------------------------------------------+
   | name_space            | String                | **Definition**                                 |
   |                       |                       |                                                |
   |                       |                       | Service to which the alarm belongs.            |
   |                       |                       |                                                |
   |                       |                       | **Range**                                      |
   |                       |                       |                                                |
   |                       |                       | N/A                                            |
   +-----------------------+-----------------------+------------------------------------------------+
   | alarm_level           | String                | **Definition**                                 |
   |                       |                       |                                                |
   |                       |                       | Alarm severity.                                |
   |                       |                       |                                                |
   |                       |                       | **Range**                                      |
   |                       |                       |                                                |
   |                       |                       | N/A                                            |
   +-----------------------+-----------------------+------------------------------------------------+
   | is_user_visible       | String                | **Definition**                                 |
   |                       |                       |                                                |
   |                       |                       | Whether the alarm is visible to users.         |
   |                       |                       |                                                |
   |                       |                       | **Range**                                      |
   |                       |                       |                                                |
   |                       |                       | N/A                                            |
   +-----------------------+-----------------------+------------------------------------------------+
   | is_converge           | String                | **Definition**                                 |
   |                       |                       |                                                |
   |                       |                       | Whether to cover the prior alarm               |
   |                       |                       |                                                |
   |                       |                       | **Range**                                      |
   |                       |                       |                                                |
   |                       |                       | N/A                                            |
   +-----------------------+-----------------------+------------------------------------------------+
   | converge_time         | Integer               | **Definition**                                 |
   |                       |                       |                                                |
   |                       |                       | Coverage time.                                 |
   |                       |                       |                                                |
   |                       |                       | **Range**                                      |
   |                       |                       |                                                |
   |                       |                       | N/A                                            |
   +-----------------------+-----------------------+------------------------------------------------+
   | is_maintain_visible   | String                | **Definition**                                 |
   |                       |                       |                                                |
   |                       |                       | Whether the alarm is visible to O&M personnel. |
   |                       |                       |                                                |
   |                       |                       | **Range**                                      |
   |                       |                       |                                                |
   |                       |                       | N/A                                            |
   +-----------------------+-----------------------+------------------------------------------------+

Example Requests
----------------

.. code-block::

   https://{Endpoint}/v2/{project_id}/alarm-configs

Example Responses
-----------------

**Status code: 200**

Alarm configurations queried.

.. code-block::

   {
     "alarm_configs" : [ {
       "id" : "fd02e440-b4e2-4d2c-8d98-4d80224cf848",
       "alarm_id" : "DWS_2000000021_1",
       "alarm_name" : "File Handle Usage Exceeds Threshold",
       "name_space" : "dws",
       "alarm_level" : "urgent",
       "is_user_visible" : "1",
       "is_converge" : "0",
       "converge_time" : 0,
       "is_maintain_visible" : "0"
     } ],
     "count" : 1
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Alarm configurations queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
