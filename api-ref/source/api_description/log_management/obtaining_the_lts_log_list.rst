:original_name: ListLtsLogs.html

.. _ListLtsLogs:

Obtaining the LTS Log List
==========================

Function
--------

This API is used to query the LTS log list.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1/{project_id}/clusters/{cluster_id}/lts-logs

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

.. table:: **Table 2** Query Parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                             |
   +=================+=================+=================+=========================================================+
   | limit           | No              | Integer         | **Definition**                                          |
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
   | offset          | No              | Integer         | **Definition**                                          |
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

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------------+--------------------------------------+
   | Parameter             | Type                                                                                                                | Description                          |
   +=======================+=====================================================================================================================+======================================+
   | access_status         | String                                                                                                              | **Definition**                       |
   |                       |                                                                                                                     |                                      |
   |                       |                                                                                                                     | Whether the log function is enabled. |
   |                       |                                                                                                                     |                                      |
   |                       |                                                                                                                     | **Range**                            |
   |                       |                                                                                                                     |                                      |
   |                       |                                                                                                                     | N/A                                  |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------+--------------------------------------+
   | lts_access_list       | Array of :ref:`LtslogInfo <en-us_topic_0000002500014040__en-us_topic_0000002375015733_response_ltsloginfo>` objects | **Definition**                       |
   |                       |                                                                                                                     |                                      |
   |                       |                                                                                                                     | LTS log list.                        |
   |                       |                                                                                                                     |                                      |
   |                       |                                                                                                                     | **Range**                            |
   |                       |                                                                                                                     |                                      |
   |                       |                                                                                                                     | N/A                                  |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------+--------------------------------------+
   | count                 | Integer                                                                                                             | **Definition**                       |
   |                       |                                                                                                                     |                                      |
   |                       |                                                                                                                     | Total number.                        |
   |                       |                                                                                                                     |                                      |
   |                       |                                                                                                                     | **Range**                            |
   |                       |                                                                                                                     |                                      |
   |                       |                                                                                                                     | Greater than or equal to **0**       |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------+--------------------------------------+

.. _en-us_topic_0000002500014040__en-us_topic_0000002375015733_response_ltsloginfo:

.. table:: **Table 4** LtslogInfo

   +-----------------------+-----------------------+-----------------------------+
   | Parameter             | Type                  | Description                 |
   +=======================+=======================+=============================+
   | status                | String                | **Definition**              |
   |                       |                       |                             |
   |                       |                       | Configuration status.       |
   |                       |                       |                             |
   |                       |                       | **Range**                   |
   |                       |                       |                             |
   |                       |                       | N/A                         |
   +-----------------------+-----------------------+-----------------------------+
   | id                    | String                | **Definition**              |
   |                       |                       |                             |
   |                       |                       | Log ID.                     |
   |                       |                       |                             |
   |                       |                       | **Range**                   |
   |                       |                       |                             |
   |                       |                       | N/A                         |
   +-----------------------+-----------------------+-----------------------------+
   | log_type              | String                | **Definition**              |
   |                       |                       |                             |
   |                       |                       | Log type.                   |
   |                       |                       |                             |
   |                       |                       | **Range**                   |
   |                       |                       |                             |
   |                       |                       | N/A                         |
   +-----------------------+-----------------------+-----------------------------+
   | log_desc              | String                | **Definition**              |
   |                       |                       |                             |
   |                       |                       | Log description.            |
   |                       |                       |                             |
   |                       |                       | **Range**                   |
   |                       |                       |                             |
   |                       |                       | N/A                         |
   +-----------------------+-----------------------+-----------------------------+
   | access_url            | String                | **Definition**              |
   |                       |                       |                             |
   |                       |                       | URL for accessing LTS logs. |
   |                       |                       |                             |
   |                       |                       | **Range**                   |
   |                       |                       |                             |
   |                       |                       | N/A                         |
   +-----------------------+-----------------------+-----------------------------+

Example Requests
----------------

.. code-block:: text

   GET https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/lts-logs

Example Responses
-----------------

**Status code: 200**

LTS log list queried.

.. code-block::

   {
     "access_status" : "OPEN",
     "lts_access_list" : [ {
       "status" : "OPEN",
       "id" : "c0c4e5f2-9b2a-4b47-a649-baf40b33e2e0",
       "log_type" : "messages",
       "log_desc" : "operating system messages log",
       "access_url" : "/lts/?region=eu-de-01&locale=#/cts/logEventsLeftMenu/events?groupId=b6680a92-e14f-4a7d-b669-4f702db806f7&groupName=s-5&topicId=1a9fe6d0-d383-4d58-adb6-2c26d229944e&topicName=messages&epsId=0"
     } ],
     "count" : 2
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         LTS log list queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
