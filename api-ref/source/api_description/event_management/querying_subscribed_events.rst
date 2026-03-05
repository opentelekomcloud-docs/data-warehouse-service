:original_name: ListEventSubs.html

.. _ListEventSubs:

Querying Subscribed Events
==========================

Function
--------

This API is used to query subscribed events.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/event-subs

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
   |                       |                                                                                                                                                   | Total number of event subscriptions. |
   |                       |                                                                                                                                                   |                                      |
   |                       |                                                                                                                                                   | **Range**                            |
   |                       |                                                                                                                                                   |                                      |
   |                       |                                                                                                                                                   | N/A                                  |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------+
   | event_subscriptions   | Array of :ref:`EventSubscriptionResponse <en-us_topic_0000002500173984__en-us_topic_0000002374935757_response_eventsubscriptionresponse>` objects | **Definition**                       |
   |                       |                                                                                                                                                   |                                      |
   |                       |                                                                                                                                                   | Event subscription list.             |
   |                       |                                                                                                                                                   |                                      |
   |                       |                                                                                                                                                   | **Range**                            |
   |                       |                                                                                                                                                   |                                      |
   |                       |                                                                                                                                                   | N/A                                  |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------+

.. _en-us_topic_0000002500173984__en-us_topic_0000002374935757_response_eventsubscriptionresponse:

.. table:: **Table 4** EventSubscriptionResponse

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

.. code-block::

   https://{Endpoint}/v2/4cf650fd46704908aa071b4df2453e1e/event-subs

Example Responses
-----------------

**Status code: 200**

Subscribed events queried.

.. code-block::

   {
     "event_subscriptions" : [ {
       "id" : "4d62f33b-b9ee-41d3-b1bc-67e54b2239f9",
       "name" : "00",
       "category" : "",
       "severity" : "",
       "tag" : "",
       "enable" : 1,
       "language" : "zh-cn",
       "source_type" : "",
       "source_id" : "",
       "project_id" : "4cf650fd46704908aa071b4df2453e1e",
       "name_space" : "DWS",
       "notification_target" : "urn:smn:eu-de-01:4cf650fd46704908aa071b4df2453e1e:CGS",
       "notification_target_name" : "CGS",
       "notification_target_type" : "SMN",
       "time_zone" : "GMT+08:00"
     } ],
     "count" : 1
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Subscribed events queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
