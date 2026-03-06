:original_name: ListSnapshotStatistics.html

.. _ListSnapshotStatistics:

Querying Snapshot Statistics
============================

Function
--------

This API is used to query snapshot statistics.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1.0/{project_id}/clusters/{cluster_id}/snapshots/statistics

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

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 2** Response body parameters

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Parameter             | Type                                                                                                                                | Description           |
   +=======================+=====================================================================================================================================+=======================+
   | statistics            | Array of :ref:`SnapshotsStatistic <en-us_topic_0000002500174068__en-us_topic_0000002341097120_response_snapshotsstatistic>` objects | **Definition**        |
   |                       |                                                                                                                                     |                       |
   |                       |                                                                                                                                     | Snapshot statistics.  |
   |                       |                                                                                                                                     |                       |
   |                       |                                                                                                                                     | **Range**             |
   |                       |                                                                                                                                     |                       |
   |                       |                                                                                                                                     | N/A                   |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

.. _en-us_topic_0000002500174068__en-us_topic_0000002341097120_response_snapshotsstatistic:

.. table:: **Table 3** SnapshotsStatistic

   +-----------------------+-----------------------+---------------------------------+
   | Parameter             | Type                  | Description                     |
   +=======================+=======================+=================================+
   | name                  | String                | **Definition**                  |
   |                       |                       |                                 |
   |                       |                       | Resource statistics name.       |
   |                       |                       |                                 |
   |                       |                       | **Range**                       |
   |                       |                       |                                 |
   |                       |                       | **storage.free**: free capacity |
   |                       |                       |                                 |
   |                       |                       | **storage.paid**: paid capacity |
   |                       |                       |                                 |
   |                       |                       | **storage.used**: used capacity |
   +-----------------------+-----------------------+---------------------------------+
   | value                 | Number                | **Definition**                  |
   |                       |                       |                                 |
   |                       |                       | Resource statistics value.      |
   |                       |                       |                                 |
   |                       |                       | **Range**                       |
   |                       |                       |                                 |
   |                       |                       | N/A                             |
   +-----------------------+-----------------------+---------------------------------+
   | unit                  | String                | **Definition**                  |
   |                       |                       |                                 |
   |                       |                       | Resource statistics unit.       |
   |                       |                       |                                 |
   |                       |                       | **Range**                       |
   |                       |                       |                                 |
   |                       |                       | N/A                             |
   +-----------------------+-----------------------+---------------------------------+

Example Requests
----------------

.. code-block:: text

   GET https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/snapshots/statistics

Example Responses
-----------------

**Status code: 200**

Snapshot statistics queried.

.. code-block::

   {
     "statistics" : [ {
       "name" : "storage.free",
       "value" : 300.0,
       "unit" : "GB"
     }, {
       "name" : "storage.paid",
       "value" : 0,
       "unit" : "GB"
     }, {
       "name" : "storage.used",
       "value" : 128.5,
       "unit" : "GB"
     } ]
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Snapshot statistics queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
