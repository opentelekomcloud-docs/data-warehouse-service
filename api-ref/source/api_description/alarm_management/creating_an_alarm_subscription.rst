:original_name: CreateAlarmSub.html

.. _CreateAlarmSub:

Creating an Alarm Subscription
==============================

Function
--------

This API is used to create an alarm subscription.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v2/{project_id}/alarm-subs

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

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +--------------------------+-----------------+-----------------+--------------------------------------------+
   | Parameter                | Mandatory       | Type            | Description                                |
   +==========================+=================+=================+============================================+
   | name                     | Yes             | String          | **Definition**                             |
   |                          |                 |                 |                                            |
   |                          |                 |                 | Subscribed alarm name.                     |
   |                          |                 |                 |                                            |
   |                          |                 |                 | **Range**                                  |
   |                          |                 |                 |                                            |
   |                          |                 |                 | N/A                                        |
   +--------------------------+-----------------+-----------------+--------------------------------------------+
   | enable                   | No              | Integer         | **Definition**                             |
   |                          |                 |                 |                                            |
   |                          |                 |                 | Whether to enable subscription.            |
   |                          |                 |                 |                                            |
   |                          |                 |                 | **Range**                                  |
   |                          |                 |                 |                                            |
   |                          |                 |                 | N/A                                        |
   +--------------------------+-----------------+-----------------+--------------------------------------------+
   | alarm_level              | No              | String          | **Definition**                             |
   |                          |                 |                 |                                            |
   |                          |                 |                 | Alarm severity.                            |
   |                          |                 |                 |                                            |
   |                          |                 |                 | **Range**                                  |
   |                          |                 |                 |                                            |
   |                          |                 |                 | N/A                                        |
   +--------------------------+-----------------+-----------------+--------------------------------------------+
   | notification_target      | Yes             | String          | **Definition**                             |
   |                          |                 |                 |                                            |
   |                          |                 |                 | Message topic address.                     |
   |                          |                 |                 |                                            |
   |                          |                 |                 | **Range**                                  |
   |                          |                 |                 |                                            |
   |                          |                 |                 | N/A                                        |
   +--------------------------+-----------------+-----------------+--------------------------------------------+
   | notification_target_name | Yes             | String          | **Definition**                             |
   |                          |                 |                 |                                            |
   |                          |                 |                 | Message topic name.                        |
   |                          |                 |                 |                                            |
   |                          |                 |                 | **Range**                                  |
   |                          |                 |                 |                                            |
   |                          |                 |                 | N/A                                        |
   +--------------------------+-----------------+-----------------+--------------------------------------------+
   | notification_target_type | Yes             | String          | **Definition**                             |
   |                          |                 |                 |                                            |
   |                          |                 |                 | Message topic type. Only SMN is supported. |
   |                          |                 |                 |                                            |
   |                          |                 |                 | **Range**                                  |
   |                          |                 |                 |                                            |
   |                          |                 |                 | N/A                                        |
   +--------------------------+-----------------+-----------------+--------------------------------------------+
   | time_zone                | Yes             | String          | **Definition**                             |
   |                          |                 |                 |                                            |
   |                          |                 |                 | Time zone.                                 |
   |                          |                 |                 |                                            |
   |                          |                 |                 | **Range**                                  |
   |                          |                 |                 |                                            |
   |                          |                 |                 | N/A                                        |
   +--------------------------+-----------------+-----------------+--------------------------------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

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

Create an alarm subscription (Subscription name: **zrf-test-12**. Alarm m severity: **critical,major,minor**. SMN topic name: **dws-test-nodelete**. Topic address: **urn:smn:eu-de-01:4cf650fd46704908aa071b4df2453e1e:dws-test-nodelete**).

.. code-block::

   https://{Endpoint}/v2/4cf650fd46704908aa071b4df2453e1e/alarm-subs

   {
     "alarm_level" : "urgent,important,minor",
     "enable" : 1,
     "name" : "zrf-test-12",
     "notification_target" : "urn:smn:eu-de-01:4cf650fd46704908aa071b4df2453e1e:dws-test-nodelete",
     "notification_target_name" : "dws-test-nodelete",
     "notification_target_type" : "SMN",
     "time_zone" : "GMT+08:00"
   }

Example Responses
-----------------

**Status code: 200**

Alarm subscription created.

.. code-block::

   {
     "id" : "273ce506-dad8-411c-92f9-be5004739b40",
     "name" : "zrf-test-12",
     "enable" : 1,
     "language" : "zh-cn",
     "alarm_level" : "urgent,important,minor",
     "project_id" : "4cf650fd46704908aa071b4df2453e1e",
     "name_space" : "dws",
     "notification_target" : "urn:smn:eu-de-01:4cf650fd46704908aa071b4df2453e1e:dws-test-nodelete",
     "notification_target_name" : "dws-test-nodelete",
     "notification_target_type" : "SMN",
     "time_zone" : "GMT+08:00"
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Alarm subscription created.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
