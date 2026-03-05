:original_name: ListLogicalClusterVolumes.html

.. _ListLogicalClusterVolumes:

Querying Disk Information of a Logical Cluster
==============================================

Function
--------

This API is used to query the disk information of a logical cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/clusters/{cluster_id}/logical-clusters/volumes

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
   |                 |                 |                 | 10                                                      |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+
   | Parameter             | Type                                                                                                                                    | Description                                   |
   +=======================+=========================================================================================================================================+===============================================+
   | volumes               | Array of :ref:`LogicalClusterVolume <en-us_topic_0000002500014094__en-us_topic_0000002341096936_response_logicalclustervolume>` objects | **Definition**                                |
   |                       |                                                                                                                                         |                                               |
   |                       |                                                                                                                                         | Disk information list of the logical cluster. |
   |                       |                                                                                                                                         |                                               |
   |                       |                                                                                                                                         | **Range**                                     |
   |                       |                                                                                                                                         |                                               |
   |                       |                                                                                                                                         | N/A                                           |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+
   | count                 | Long                                                                                                                                    | **Definition**                                |
   |                       |                                                                                                                                         |                                               |
   |                       |                                                                                                                                         | Total number of disks in the logical cluster. |
   |                       |                                                                                                                                         |                                               |
   |                       |                                                                                                                                         | **Range**                                     |
   |                       |                                                                                                                                         |                                               |
   |                       |                                                                                                                                         | N/A                                           |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------+

.. _en-us_topic_0000002500014094__en-us_topic_0000002341096936_response_logicalclustervolume:

.. table:: **Table 4** LogicalClusterVolume

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | logical_cluster_name  | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Logical cluster name. |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | usage                 | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Usage disk space.     |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | total                 | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Total disk space.     |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | percent               | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Disk usage.           |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

Query the disk information of a logical cluster.

.. code-block:: text

   GET https://{Endpoint}/v2/9b06d044ea4f49f1a58b2bed2b0084bd/clusters/9b7ff56b-47b3-4d00-a1fd-4c023d34404b/logical-clusters/volumes

Example Responses
-----------------

**Status code: 200**

Cluster disk information queried.

.. code-block::

   {
     "volumes" : [ {
       "logical_cluster_name" : "v3_logical",
       "usage" : "1.0G",
       "total" : "10.0G",
       "percent" : 0.1
     } ],
     "count" : 1
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Cluster disk information queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
