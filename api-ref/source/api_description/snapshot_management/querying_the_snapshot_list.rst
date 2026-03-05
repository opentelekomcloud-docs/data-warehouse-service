:original_name: ListSnapshots.html

.. _ListSnapshots:

Querying the Snapshot List
==========================

Function
--------

This API is used to query the snapshot list.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1.0/{project_id}/snapshots

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

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 2** Response body parameters

   +-----------------------+-------------------------------------------------------------------------------------------------------------------+--------------------------------------+
   | Parameter             | Type                                                                                                              | Description                          |
   +=======================+===================================================================================================================+======================================+
   | snapshots             | Array of :ref:`Snapshots <en-us_topic_0000002532013953__en-us_topic_0000002341097128_response_snapshots>` objects | **Definition**                       |
   |                       |                                                                                                                   |                                      |
   |                       |                                                                                                                   | List of snapshot objects.            |
   |                       |                                                                                                                   |                                      |
   |                       |                                                                                                                   | **Range**                            |
   |                       |                                                                                                                   |                                      |
   |                       |                                                                                                                   | N/A                                  |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------+--------------------------------------+
   | count                 | Integer                                                                                                           | **Definition**                       |
   |                       |                                                                                                                   |                                      |
   |                       |                                                                                                                   | Total number of records in the list. |
   |                       |                                                                                                                   |                                      |
   |                       |                                                                                                                   | **Range**                            |
   |                       |                                                                                                                   |                                      |
   |                       |                                                                                                                   | N/A                                  |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------+--------------------------------------+

.. _en-us_topic_0000002532013953__en-us_topic_0000002341097128_response_snapshots:

.. table:: **Table 3** Snapshots

   +-----------------------+-----------------------+--------------------------------------------------------------------------+
   | Parameter             | Type                  | Description                                                              |
   +=======================+=======================+==========================================================================+
   | id                    | String                | **Definition**                                                           |
   |                       |                       |                                                                          |
   |                       |                       | Snapshot ID.                                                             |
   |                       |                       |                                                                          |
   |                       |                       | **Range**                                                                |
   |                       |                       |                                                                          |
   |                       |                       | N/A                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------+
   | name                  | String                | **Definition**                                                           |
   |                       |                       |                                                                          |
   |                       |                       | Snapshot name.                                                           |
   |                       |                       |                                                                          |
   |                       |                       | **Range**                                                                |
   |                       |                       |                                                                          |
   |                       |                       | N/A                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------+
   | description           | String                | **Definition**                                                           |
   |                       |                       |                                                                          |
   |                       |                       | Snapshot description.                                                    |
   |                       |                       |                                                                          |
   |                       |                       | **Range**                                                                |
   |                       |                       |                                                                          |
   |                       |                       | N/A                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------+
   | started               | String                | **Definition**                                                           |
   |                       |                       |                                                                          |
   |                       |                       | Snapshot creation time. The format is ISO8601: YYYY-MM-DDThh:mm:ssZ.     |
   |                       |                       |                                                                          |
   |                       |                       | **Range**                                                                |
   |                       |                       |                                                                          |
   |                       |                       | N/A                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------+
   | finished              | String                | **Definition**                                                           |
   |                       |                       |                                                                          |
   |                       |                       | Time when a snapshot is complete. Format: ISO8601: YYYY-MM-DDThh:mm:ssZ. |
   |                       |                       |                                                                          |
   |                       |                       | **Range**                                                                |
   |                       |                       |                                                                          |
   |                       |                       | N/A                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------+
   | size                  | Double                | **Definition**                                                           |
   |                       |                       |                                                                          |
   |                       |                       | Snapshot size, in GB.                                                    |
   |                       |                       |                                                                          |
   |                       |                       | **Range**                                                                |
   |                       |                       |                                                                          |
   |                       |                       | N/A                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------+
   | status                | String                | **Definition**                                                           |
   |                       |                       |                                                                          |
   |                       |                       | Snapshot status.                                                         |
   |                       |                       |                                                                          |
   |                       |                       | **Range**                                                                |
   |                       |                       |                                                                          |
   |                       |                       | -  **CREATING**                                                          |
   |                       |                       |                                                                          |
   |                       |                       | -  **AVAILABLE**                                                         |
   |                       |                       |                                                                          |
   |                       |                       | -  **UNAVAILABLE**                                                       |
   +-----------------------+-----------------------+--------------------------------------------------------------------------+
   | type                  | String                | **Definition**                                                           |
   |                       |                       |                                                                          |
   |                       |                       | Snapshot type.                                                           |
   |                       |                       |                                                                          |
   |                       |                       | **Range**                                                                |
   |                       |                       |                                                                          |
   |                       |                       | N/A                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------+
   | cluster_id            | String                | **Definition**                                                           |
   |                       |                       |                                                                          |
   |                       |                       | ID of the cluster for which the snapshot is created.                     |
   |                       |                       |                                                                          |
   |                       |                       | **Range**                                                                |
   |                       |                       |                                                                          |
   |                       |                       | N/A                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------+

Example Requests
----------------

.. code-block:: text

   GET https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/snapshots

Example Responses
-----------------

**Status code: 200**

Snapshot list queried.

.. code-block::

   {
     "snapshots" : [ {
       "id" : "2a4d0f86-67cd-408a-8b66-017454fb7793",
       "name" : "snapshot-1",
       "description" : "",
       "started" : "2016-08-23T03:59:23Z",
       "finished" : "2016-08-23T04:01:40Z",
       "size" : 500,
       "status" : "AVAILABLE",
       "type" : "MANUAL",
       "cluster_id" : "4f87d3c4-9e33-482f-b962-e23b30d1a18c"
     }, {
       "id" : "4af11460-06ec-48a4-b3ad-0e3bbdcd8ab1",
       "name" : "snapshot-2",
       "description" : "",
       "started" : "2016-08-23T18:20:00Z",
       "finished" : "2016-08-23T18:22:12Z",
       "size" : 500,
       "status" : "AVAILABLE",
       "type" : "MANUAL",
       "cluster_id" : "4f87d3c4-9e33-482f-b962-e23b30d1a18c"
     } ],
     "count" : 2
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Snapshot list queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
