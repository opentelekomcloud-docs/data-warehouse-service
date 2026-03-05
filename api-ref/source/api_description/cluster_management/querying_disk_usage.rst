:original_name: ShowClusterVolume.html

.. _ShowClusterVolume:

Querying Disk Usage
===================

Function
--------

This API is used to query the disk usage of a tenant management node.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1/{project_id}/clusters/{cluster_id}/volume

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

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
   | Parameter             | Type                                                                                                                            | Description                       |
   +=======================+=================================================================================================================================+===================================+
   | duplicate             | Integer                                                                                                                         | **Definition**                    |
   |                       |                                                                                                                                 |                                   |
   |                       |                                                                                                                                 | Number of disks on a single node. |
   |                       |                                                                                                                                 |                                   |
   |                       |                                                                                                                                 | **Range**                         |
   |                       |                                                                                                                                 |                                   |
   |                       |                                                                                                                                 | N/A                               |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
   | disk_info_list        | Array of :ref:`DiskInfoResponse <en-us_topic_0000002531893949__en-us_topic_0000002340937132_response_diskinforesponse>` objects | **Definition**                    |
   |                       |                                                                                                                                 |                                   |
   |                       |                                                                                                                                 | Node capacity details.            |
   |                       |                                                                                                                                 |                                   |
   |                       |                                                                                                                                 | **Range**                         |
   |                       |                                                                                                                                 |                                   |
   |                       |                                                                                                                                 | N/A                               |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+

.. _en-us_topic_0000002531893949__en-us_topic_0000002340937132_response_diskinforesponse:

.. table:: **Table 3** DiskInfoResponse

   +-----------------------+-----------------------+------------------------+
   | Parameter             | Type                  | Description            |
   +=======================+=======================+========================+
   | percentage            | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Disk usage percentage. |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | id                    | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Node ID.               |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | name                  | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Node name.             |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | disk_capacity         | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Total disk capacity.   |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | disk_used             | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Used disk capacity.    |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+

Example Requests
----------------

Query the disk usage.

.. code-block:: text

   GET https://{Endpoint}/v1/0536cdee2200d5912f7cc00b877980f1/clusters/2c14ab37-ed8f-48fa-8c23-c9e772c7fd5d/volume

Example Responses
-----------------

**Status code: 200**

Disk information queried.

.. code-block::

   {
     "duplicate" : 2,
     "disk_info_list" : [ {
       "percentage" : "2.18",
       "id" : "7240fb6b-c8d9-43da-9177-1540117989b5",
       "name" : "dws-cluster-7-dws-cn-cn-3-1",
       "disk_capacity" : "400",
       "disk_used" : "8.74"
     }, {
       "percentage" : "2.19",
       "id" : "af4c33f3-27bd-4a07-90a4-5a0ad2f50a13",
       "name" : "dws-cluster-7-dws-cn-cn-2-1",
       "disk_capacity" : "400",
       "disk_used" : "8.78"
     }, {
       "percentage" : "2.45",
       "id" : "d197f4f7-774f-4858-b1e3-ce2319bd59bf",
       "name" : "dws-cluster-7-dws-cn-cn-1-1",
       "disk_capacity" : "400",
       "disk_used" : "9.82"
     } ]
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Disk information queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
500         Internal server error.
503         Service unavailable.
=========== =====================================
