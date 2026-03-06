:original_name: ListLogicalClusterRings.html

.. _ListLogicalClusterRings:

Querying Information About Available Ring Nodes in a Logical Cluster
====================================================================

Function
--------

This API is used to query information about available ring nodes in a logical cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/clusters/{cluster_id}/logical-clusters/rings

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

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------+--------------------------+
   | Parameter             | Type                                                                                                                                        | Description              |
   +=======================+=============================================================================================================================================+==========================+
   | cluster_rings         | Array of :ref:`LogicalClusterRingInfo <en-us_topic_0000002531893983__en-us_topic_0000002340937164_response_logicalclusterringinfo>` objects | **Definition**           |
   |                       |                                                                                                                                             |                          |
   |                       |                                                                                                                                             | Cluster ring list.       |
   |                       |                                                                                                                                             |                          |
   |                       |                                                                                                                                             | **Range**                |
   |                       |                                                                                                                                             |                          |
   |                       |                                                                                                                                             | N/A                      |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------+--------------------------+
   | count                 | Integer                                                                                                                                     | **Definition**           |
   |                       |                                                                                                                                             |                          |
   |                       |                                                                                                                                             | Number of cluster rings. |
   |                       |                                                                                                                                             |                          |
   |                       |                                                                                                                                             | **Range**                |
   |                       |                                                                                                                                             |                          |
   |                       |                                                                                                                                             | N/A                      |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------+--------------------------+

.. _en-us_topic_0000002531893983__en-us_topic_0000002340937164_response_logicalclusterringinfo:

.. table:: **Table 4** LogicalClusterRingInfo

   +-----------------------+-----------------------------------------------------------------------------------------------------------------+------------------------------------+
   | Parameter             | Type                                                                                                            | Description                        |
   +=======================+=================================================================================================================+====================================+
   | ring_hosts            | Array of :ref:`RingHost <en-us_topic_0000002531893983__en-us_topic_0000002340937164_response_ringhost>` objects | **Definition**                     |
   |                       |                                                                                                                 |                                    |
   |                       |                                                                                                                 | Cluster instance ring information. |
   |                       |                                                                                                                 |                                    |
   |                       |                                                                                                                 | **Range**                          |
   |                       |                                                                                                                 |                                    |
   |                       |                                                                                                                 | N/A                                |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------+------------------------------------+

.. _en-us_topic_0000002531893983__en-us_topic_0000002340937164_response_ringhost:

.. table:: **Table 5** RingHost

   +-----------------------+-----------------------+------------------------+
   | Parameter             | Type                  | Description            |
   +=======================+=======================+========================+
   | host_name             | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Host name.             |
   |                       |                       |                        |
   |                       |                       | **Constraints**        |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   |                       |                       |                        |
   |                       |                       | **Default Value**      |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | back_ip               | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Backend IP address.    |
   |                       |                       |                        |
   |                       |                       | **Constraints**        |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   |                       |                       |                        |
   |                       |                       | **Default Value**      |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | cpu_cores             | Integer               | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Number of host CPUs.   |
   |                       |                       |                        |
   |                       |                       | **Constraints**        |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   |                       |                       |                        |
   |                       |                       | **Default Value**      |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | memory                | Double                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Host memory.           |
   |                       |                       |                        |
   |                       |                       | **Constraints**        |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   |                       |                       |                        |
   |                       |                       | **Default Value**      |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | disk_size             | Double                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Disk size of the host. |
   |                       |                       |                        |
   |                       |                       | **Constraints**        |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   |                       |                       |                        |
   |                       |                       | **Default Value**      |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+

Example Requests
----------------

Query information about available ring nodes in a logical cluster.

.. code-block:: text

   GET https://{Endpoint}/v2/9b06d044ea4f49f1a58b2bed2b0084bd/clusters/9b7ff56b-47b3-4d00-a1fd-4c023d34404b/logical-clusters/rings

Example Responses
-----------------

**Status code: 200**

Information about available ring nodes in a logical cluster queried.

.. code-block::

   {
     "cluster_rings" : [ {
       "ring_hosts" : [ {
         "host_name" : "host-172-16-20-246",
         "back_ip" : "172.16.73.90",
         "cpu_cores" : 8,
         "memory" : 32.0,
         "disk_size" : 800.0
       }, {
         "host_name" : "host-172-16-4-26",
         "back_ip" : "172.16.123.5",
         "cpu_cores" : 8,
         "memory" : 32.0,
         "disk_size" : 800.0
       }, {
         "host_name" : "host-172-16-43-90",
         "back_ip" : "172.16.92.175",
         "cpu_cores" : 8,
         "memory" : 32.0,
         "disk_size" : 800.0
       } ]
     } ],
     "count" : 1
   }

Status Codes
------------

+-------------+----------------------------------------------------------------------+
| Status Code | Description                                                          |
+=============+======================================================================+
| 200         | Information about available ring nodes in a logical cluster queried. |
+-------------+----------------------------------------------------------------------+
| 400         | Request error.                                                       |
+-------------+----------------------------------------------------------------------+
| 401         | Authentication failed.                                               |
+-------------+----------------------------------------------------------------------+
| 403         | You do not have required permissions.                                |
+-------------+----------------------------------------------------------------------+
| 404         | No resources found.                                                  |
+-------------+----------------------------------------------------------------------+
| 500         | Internal server error.                                               |
+-------------+----------------------------------------------------------------------+
| 503         | Service unavailable.                                                 |
+-------------+----------------------------------------------------------------------+
