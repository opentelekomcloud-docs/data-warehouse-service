:original_name: ShowResourceStatistics.html

.. _ShowResourceStatistics:

Querying Resource Statistics
============================

Function
--------

This API is used to query resource statistics.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1/{project_id}/resource-statistics

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

   +-----------------+-----------------+-----------------+--------------------------------+
   | Parameter       | Mandatory       | Type            | Description                    |
   +=================+=================+=================+================================+
   | namespace       | No              | String          | **Definition**                 |
   |                 |                 |                 |                                |
   |                 |                 |                 | Namespace.                     |
   |                 |                 |                 |                                |
   |                 |                 |                 | **Constraints**                |
   |                 |                 |                 |                                |
   |                 |                 |                 | N/A                            |
   |                 |                 |                 |                                |
   |                 |                 |                 | **Range**                      |
   |                 |                 |                 |                                |
   |                 |                 |                 | The value can only be **dws**. |
   |                 |                 |                 |                                |
   |                 |                 |                 | **Default Value**              |
   |                 |                 |                 |                                |
   |                 |                 |                 | **dws**                        |
   +-----------------+-----------------+-----------------+--------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------------------------+------------------------------+
   | Parameter             | Type                                                                                                                  | Description                  |
   +=======================+=======================================================================================================================+==============================+
   | cluster_statistics    | :ref:`StatusStatistics <en-us_topic_0000002531893953__en-us_topic_0000002374935101_response_statusstatistics>` object | **Definition**               |
   |                       |                                                                                                                       |                              |
   |                       |                                                                                                                       | Cluster resource statistics. |
   |                       |                                                                                                                       |                              |
   |                       |                                                                                                                       | **Range**                    |
   |                       |                                                                                                                       |                              |
   |                       |                                                                                                                       | N/A                          |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------+------------------------------+
   | node_statistics       | :ref:`StatusStatistics <en-us_topic_0000002531893953__en-us_topic_0000002374935101_response_statusstatistics>` object | **Definition**               |
   |                       |                                                                                                                       |                              |
   |                       |                                                                                                                       | Node resource statistics.    |
   |                       |                                                                                                                       |                              |
   |                       |                                                                                                                       | **Range**                    |
   |                       |                                                                                                                       |                              |
   |                       |                                                                                                                       | N/A                          |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------+------------------------------+

.. _en-us_topic_0000002531893953__en-us_topic_0000002374935101_response_statusstatistics:

.. table:: **Table 4** StatusStatistics

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | active                | Long                  | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Active resources.     |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | total                 | Long                  | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Total resources.      |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

Query resource statistics.

.. code-block:: text

   GET https://{Endpoint}/v1/0536cdee2200d5912f7cc00b877980f1/resource-statistics

Example Responses
-----------------

**Status code: 200**

Resource statistics queried.

.. code-block::

   {
     "cluster_statistics" : {
       "active" : 2,
       "total" : 2
     },
     "node_statistics" : {
       "active" : 6,
       "total" : 36
     }
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Resource statistics queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
500         Internal server error.
503         Service unavailable.
=========== =====================================
