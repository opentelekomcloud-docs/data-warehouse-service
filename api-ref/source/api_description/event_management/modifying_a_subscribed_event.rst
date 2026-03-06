:original_name: UpdateEventSub.html

.. _UpdateEventSub:

Modifying a Subscribed Event
============================

Function
--------

This API is used to modify a subscribed event.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

PUT /v2/{project_id}/event-subs/{event_sub_id}

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
   | event_sub_id    | Yes             | String          | **Definition**                                                                    |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | Event subscription ID.                                                            |
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

   +--------------------------+-----------------+-----------------+-----------------------------------------------------------------+
   | Parameter                | Mandatory       | Type            | Description                                                     |
   +==========================+=================+=================+=================================================================+
   | name                     | Yes             | String          | **Definition**                                                  |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | Event subscription name.                                        |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **Range**                                                       |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | N/A                                                             |
   +--------------------------+-----------------+-----------------+-----------------------------------------------------------------+
   | source_type              | No              | String          | **Definition**                                                  |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | Event source type.                                              |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **Range**                                                       |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **cluster**, **backup**, or **disaster-recovery**               |
   +--------------------------+-----------------+-----------------+-----------------------------------------------------------------+
   | source_id                | No              | String          | **Definition**                                                  |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | Event source ID.                                                |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **Range**                                                       |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | N/A                                                             |
   +--------------------------+-----------------+-----------------+-----------------------------------------------------------------+
   | category                 | No              | String          | **Definition**                                                  |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | Event category.                                                 |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **Range**                                                       |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **management**, **monitor**, **security**, or **system alarm**. |
   +--------------------------+-----------------+-----------------+-----------------------------------------------------------------+
   | severity                 | No              | String          | **Definition**                                                  |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | Event severity.                                                 |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **Range**                                                       |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | The value can be **normal** or **warning**.                     |
   +--------------------------+-----------------+-----------------+-----------------------------------------------------------------+
   | tag                      | No              | String          | **Definition**                                                  |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | Event tag.                                                      |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **Range**                                                       |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | N/A                                                             |
   +--------------------------+-----------------+-----------------+-----------------------------------------------------------------+
   | enable                   | No              | Integer         | **Definition**                                                  |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | Whether to enable subscription.                                 |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **Range**                                                       |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **1**: enabled; **0**: disabled.                                |
   +--------------------------+-----------------+-----------------+-----------------------------------------------------------------+
   | notification_target      | Yes             | String          | **Definition**                                                  |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | Message notification address.                                   |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **Range**                                                       |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | N/A                                                             |
   +--------------------------+-----------------+-----------------+-----------------------------------------------------------------+
   | notification_target_name | Yes             | String          | **Definition**                                                  |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | Message topic name.                                             |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **Range**                                                       |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | N/A                                                             |
   +--------------------------+-----------------+-----------------+-----------------------------------------------------------------+
   | notification_target_type | Yes             | String          | **Definition**                                                  |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | Message notification type. The value can only be **SMN**.       |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **Range**                                                       |
   |                          |                 |                 |                                                                 |
   |                          |                 |                 | **SMN**                                                         |
   +--------------------------+-----------------+-----------------+-----------------------------------------------------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +--------------------------+-----------------------+---------------------------------------------+
   | Parameter                | Type                  | Description                                 |
   +==========================+=======================+=============================================+
   | id                       | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Subscription ID.                            |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | name                     | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Subscription name.                          |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | source_type              | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Event source type.                          |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | source_id                | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Event source ID.                            |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | category                 | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Event category.                             |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | severity                 | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Event severity.                             |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | tag                      | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Event tag.                                  |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | enable                   | Integer               | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Whether to enable subscription.             |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | **1**: enabled; **0**: disabled.            |
   +--------------------------+-----------------------+---------------------------------------------+
   | project_id               | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Project ID.                                 |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | name_space               | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Service.                                    |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | notification_target      | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Address for the message notification topic. |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | notification_target_name | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Message notification topic.                 |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | notification_target_type | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Message notification type.                  |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | language                 | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Language.                                   |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+
   | time_zone                | String                | **Definition**                              |
   |                          |                       |                                             |
   |                          |                       | Time zone.                                  |
   |                          |                       |                                             |
   |                          |                       | **Range**                                   |
   |                          |                       |                                             |
   |                          |                       | N/A                                         |
   +--------------------------+-----------------------+---------------------------------------------+

Example Requests
----------------

Modify the **zrf-test-66** event subscription (Change the event type to **normal** and **warning**, and change the address of the SMN message topic **dws-test-nodelete** to **urn:smn:eu-de-01:4cf650fd46704908aa071b4df2453e1e:dws-test-nodelete**).

.. code-block::

   https://{Endpoint}/v2/4cf650fd46704908aa071b4df2453e1e/event-subs/41eb162b-cd3b-4c66-88d0-0c2c17fdfc2b

   {
     "severity" : "normal,warning",
     "source_id" : "",
     "source_type" : "",
     "tag" : "",
     "category" : "",
     "enable" : 1,
     "name" : "zrf-test-66",
     "notification_target" : "urn:smn:eu-de-01:4cf650fd46704908aa071b4df2453e1e:dws-test-nodelete",
     "notification_target_name" : "dws-test-nodelete",
     "notification_target_type" : "SMN"
   }

Example Responses
-----------------

**Status code: 200**

Event subscription modified.

.. code-block::

   {
     "id" : "41eb162b-cd3b-4c66-88d0-0c2c17fdfc2b",
     "name" : "zrf-test-66",
     "category" : "",
     "severity" : "normal,warning",
     "tag" : "",
     "enable" : 1,
     "language" : "zh-cn",
     "source_type" : "",
     "source_id" : "",
     "project_id" : "4cf650fd46704908aa071b4df2453e1e",
     "name_space" : "DWS",
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
200         Event subscription modified.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
