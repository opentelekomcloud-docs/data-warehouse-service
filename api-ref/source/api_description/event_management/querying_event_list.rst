:original_name: ListEvents.html

.. _ListEvents:

Querying Event List
===================

Function
--------

This API is used to query event list.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/events

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

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | Parameter             | Type                                                                                                                      | Description             |
   +=======================+===========================================================================================================================+=========================+
   | events                | Array of :ref:`EventResponse <en-us_topic_0000002531893941__en-us_topic_0000002341097544_response_eventresponse>` objects | **Definition**          |
   |                       |                                                                                                                           |                         |
   |                       |                                                                                                                           | Event details list.     |
   |                       |                                                                                                                           |                         |
   |                       |                                                                                                                           | **Range**               |
   |                       |                                                                                                                           |                         |
   |                       |                                                                                                                           | N/A                     |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------+
   | count                 | Integer                                                                                                                   | **Definition**          |
   |                       |                                                                                                                           |                         |
   |                       |                                                                                                                           | Total number of events. |
   |                       |                                                                                                                           |                         |
   |                       |                                                                                                                           | **Range**               |
   |                       |                                                                                                                           |                         |
   |                       |                                                                                                                           | N/A                     |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------+

.. _en-us_topic_0000002531893941__en-us_topic_0000002341097544_response_eventresponse:

.. table:: **Table 4** EventResponse

   +-----------------------+-----------------------+-------------------------------------+
   | Parameter             | Type                  | Description                         |
   +=======================+=======================+=====================================+
   | category              | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Event category.                     |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | description           | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Incident description.               |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | event_id              | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Event ID.                           |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | name                  | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Definition name of the event.       |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | display_name          | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Display name of the event.          |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | name_space            | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Service to which the event belongs. |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | severity              | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Event severity.                     |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | source_type           | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Event source type.                  |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | occur_time            | Long                  | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Time.                               |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | project_id            | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Tenant credential ID.               |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | source_id             | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Event source ID.                    |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | source_name           | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Event source name.                  |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | status                | Integer               | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Status.                             |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | subject               | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Event subject.                      |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+
   | context               | String                | **Definition**                      |
   |                       |                       |                                     |
   |                       |                       | Event information.                  |
   |                       |                       |                                     |
   |                       |                       | **Range**                           |
   |                       |                       |                                     |
   |                       |                       | N/A                                 |
   +-----------------------+-----------------------+-------------------------------------+

Example Requests
----------------

.. code-block::

   https://{Endpoint}/v2/4cf650fd46704908aa071b4df2453e1e/events

Example Responses
-----------------

**Status code: 200**

Event list of the cluster queried.

.. code-block::

   {
     "events" : [ {
       "category" : "management",
       "description" : "Cluster %s deleted",
       "name" : "deleteClusterSuccess",
       "severity" : "normal",
       "status" : 2,
       "subject" : "DWS event notification",
       "context" : "The cluster test-ty-820-1006 is deleted",
       "event_id" : "f63ccf96-e3e0-474a-835a-fd1a779f68bd",
       "display_name" : "Cluster deleted",
       "name_space" : "dws",
       "source_type" : "cluster",
       "occur_time" : 1664331248330,
       "project_id" : "4cf650fd46704908aa071b4df2453e1e",
       "source_id" : "9defa0ce-b11c-47b2-abbc-5cad09ced772",
       "source_name" : "test-ty-820-1006"
     } ],
     "count" : 1
   }

Status Codes
------------

=========== ==================================
Status Code Description
=========== ==================================
200         Event list of the cluster queried.
400         Request error.
401         Authentication failed.
403         No operation permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ==================================
