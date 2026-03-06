:original_name: CreateLogicalCluster.html

.. _CreateLogicalCluster:

Creating a Logical Cluster
==========================

Function
--------

This API is used to create a logical cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v2/{project_id}/clusters/{cluster_id}/logical-clusters

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

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------+
   | Parameter       | Mandatory       | Type                                                                                                                                 | Description                                          |
   +=================+=================+======================================================================================================================================+======================================================+
   | logical_cluster | Yes             | :ref:`CreateLogicalClusterInfo <en-us_topic_0000002532014011__en-us_topic_0000002374935129_request_createlogicalclusterinfo>` object | **Definition**                                       |
   |                 |                 |                                                                                                                                      |                                                      |
   |                 |                 |                                                                                                                                      | Information required for creating a logical cluster. |
   |                 |                 |                                                                                                                                      |                                                      |
   |                 |                 |                                                                                                                                      | **Constraints**                                      |
   |                 |                 |                                                                                                                                      |                                                      |
   |                 |                 |                                                                                                                                      | N/A                                                  |
   |                 |                 |                                                                                                                                      |                                                      |
   |                 |                 |                                                                                                                                      | **Range**                                            |
   |                 |                 |                                                                                                                                      |                                                      |
   |                 |                 |                                                                                                                                      | N/A                                                  |
   |                 |                 |                                                                                                                                      |                                                      |
   |                 |                 |                                                                                                                                      | **Default Value**                                    |
   |                 |                 |                                                                                                                                      |                                                      |
   |                 |                 |                                                                                                                                      | N/A                                                  |
   +-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------+

.. _en-us_topic_0000002532014011__en-us_topic_0000002374935129_request_createlogicalclusterinfo:

.. table:: **Table 3** CreateLogicalClusterInfo

   +----------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------------+
   | Parameter            | Mandatory       | Type                                                                                                                 | Description                              |
   +======================+=================+======================================================================================================================+==========================================+
   | logical_cluster_name | Yes             | String                                                                                                               | **Definition**                           |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | Logical cluster name.                    |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | **Constraints**                          |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | N/A                                      |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | **Range**                                |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | N/A                                      |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | **Default Value**                        |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | N/A                                      |
   +----------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------------+
   | cluster_rings        | Yes             | Array of :ref:`ClusterRing <en-us_topic_0000002532014011__en-us_topic_0000002374935129_request_clusterring>` objects | **Definition**                           |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | Information of the logical cluster ring. |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | **Constraints**                          |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | N/A                                      |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | **Range**                                |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | N/A                                      |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | **Default Value**                        |
   |                      |                 |                                                                                                                      |                                          |
   |                      |                 |                                                                                                                      | N/A                                      |
   +----------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------------+

.. _en-us_topic_0000002532014011__en-us_topic_0000002374935129_request_clusterring:

.. table:: **Table 4** ClusterRing

   +----------------------------+-----------------+----------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Parameter                  | Mandatory       | Type                                                                                                           | Description                    |
   +============================+=================+================================================================================================================+================================+
   | ring_hosts                 | Yes             | Array of :ref:`RingHost <en-us_topic_0000002532014011__en-us_topic_0000002374935129_request_ringhost>` objects | **Definition**                 |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | Cluster host information.      |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | **Constraints**                |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | N/A                            |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | **Range**                      |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | N/A                            |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | **Default Value**              |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | N/A                            |
   +----------------------------+-----------------+----------------------------------------------------------------------------------------------------------------+--------------------------------+
   | un_shrinkable_cluster_ring | No              | Boolean                                                                                                        | **Definition**                 |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | Whether scale-in is supported. |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | **Constraints**                |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | N/A                            |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | **Range**                      |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | **false** or **true**          |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | **Default Value**              |
   |                            |                 |                                                                                                                |                                |
   |                            |                 |                                                                                                                | N/A                            |
   +----------------------------+-----------------+----------------------------------------------------------------------------------------------------------------+--------------------------------+

.. _en-us_topic_0000002532014011__en-us_topic_0000002374935129_request_ringhost:

.. table:: **Table 5** RingHost

   +-----------------+-----------------+-----------------+------------------------+
   | Parameter       | Mandatory       | Type            | Description            |
   +=================+=================+=================+========================+
   | host_name       | Yes             | String          | **Definition**         |
   |                 |                 |                 |                        |
   |                 |                 |                 | Host name.             |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Constraints**        |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Range**              |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Default Value**      |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   +-----------------+-----------------+-----------------+------------------------+
   | back_ip         | Yes             | String          | **Definition**         |
   |                 |                 |                 |                        |
   |                 |                 |                 | Backend IP address.    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Constraints**        |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Range**              |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Default Value**      |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   +-----------------+-----------------+-----------------+------------------------+
   | cpu_cores       | Yes             | Integer         | **Definition**         |
   |                 |                 |                 |                        |
   |                 |                 |                 | Number of host CPUs.   |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Constraints**        |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Range**              |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Default Value**      |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   +-----------------+-----------------+-----------------+------------------------+
   | memory          | Yes             | Double          | **Definition**         |
   |                 |                 |                 |                        |
   |                 |                 |                 | Host memory.           |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Constraints**        |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Range**              |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Default Value**      |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   +-----------------+-----------------+-----------------+------------------------+
   | disk_size       | Yes             | Double          | **Definition**         |
   |                 |                 |                 |                        |
   |                 |                 |                 | Disk size of the host. |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Constraints**        |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Range**              |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Default Value**      |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   +-----------------+-----------------+-----------------+------------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 6** Response body parameters

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | error_code            | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Error code.           |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | error_msg             | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Error message.        |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

Create a logical cluster.

.. code-block:: text

   POST https://{Endpoint}/v2/9b06d044ea4f49f1a58b2bed2b0084bd/clusters/9b7ff56b-47b3-4d00-a1fd-4c023d34404b/logical-clusters

   {
     "logical_cluster" : {
       "logical_cluster_name" : "v3_logical",
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
           "host_name" : "host-172-16-4-26",
           "back_ip" : "172.16.123.5",
           "cpu_cores" : 8,
           "memory" : 32.0,
           "disk_size" : 800.0
         } ]
       } ]
     }
   }

Example Responses
-----------------

**Status code: 200**

Request for creating a logical cluster submitted.

.. code-block::

   {
     "error_code" : "DWS.0000",
     "error_msg" : null
   }

Status Codes
------------

=========== =================================================
Status Code Description
=========== =================================================
200         Request for creating a logical cluster submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
500         Internal server error.
503         Service unavailable.
=========== =================================================
