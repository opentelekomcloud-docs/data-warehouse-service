:original_name: ShrinkLogicalCluster.html

.. _ShrinkLogicalCluster:

Scaling In a Logical Cluster
============================

Function
--------

This API is used to scale in a logical cluster in an elastic pool.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v1/{project_id}/clusters/{cluster_id}/logical-clusters/{logical_cluster_id}/shrink

.. table:: **Table 1** Path Parameters

   +--------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+
   | Parameter          | Mandatory       | Type            | Description                                                                                                |
   +====================+=================+=================+============================================================================================================+
   | project_id         | Yes             | String          | **Definition**                                                                                             |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | Project ID. To obtain the value, see :ref:`Obtaining a Project ID <dws_02_0011>`.                          |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | **Constraints**                                                                                            |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | N/A                                                                                                        |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | **Range**                                                                                                  |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | N/A                                                                                                        |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | **Default Value**                                                                                          |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | N/A                                                                                                        |
   +--------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+
   | cluster_id         | Yes             | String          | **Definition**                                                                                             |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | Cluster ID. For details about how to obtain the value, see :ref:`Obtaining the Cluster ID <dws_02_00068>`. |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | **Constraints**                                                                                            |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | The value must be a valid DWS cluster ID.                                                                  |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | **Range**                                                                                                  |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | It is a 36-digit UUID.                                                                                     |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | **Default Value**                                                                                          |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | N/A                                                                                                        |
   +--------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+
   | logical_cluster_id | Yes             | String          | **Definition**                                                                                             |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | Logical cluster ID.                                                                                        |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | **Constraints**                                                                                            |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | The value must be a valid DWS logical cluster ID.                                                          |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | **Range**                                                                                                  |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | It is a 36-digit UUID.                                                                                     |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | **Default Value**                                                                                          |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | N/A                                                                                                        |
   +--------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------+--------------------------------------------+
   | Parameter       | Mandatory       | Type                                                                                                                 | Description                                |
   +=================+=================+======================================================================================================================+============================================+
   | cluster_rings   | Yes             | Array of :ref:`ClusterRing <en-us_topic_0000002500174034__en-us_topic_0000002340937160_request_clusterring>` objects | **Definition**                             |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | Host ring scale-in information.            |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **Constraints**                            |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | N/A                                        |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **Range**                                  |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | N/A                                        |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **Default Value**                          |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | N/A                                        |
   +-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------+--------------------------------------------+
   | parallel_jobs   | No              | Integer                                                                                                              | **Definition**                             |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | Number of concurrent redistribution tasks. |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **Constraints**                            |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | N/A                                        |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **Range**                                  |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **1** to **200**                           |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **Default Value**                          |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **4**.                                     |
   +-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------+--------------------------------------------+
   | mode            | No              | String                                                                                                               | **Definition**                             |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | Scale-in mode.                             |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **Constraints**                            |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | N/A                                        |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **Range**                                  |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **read-only**: offline mode                |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **insert**: online mode                    |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | **Default Value**                          |
   |                 |                 |                                                                                                                      |                                            |
   |                 |                 |                                                                                                                      | insert                                     |
   +-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------+--------------------------------------------+

.. _en-us_topic_0000002500174034__en-us_topic_0000002340937160_request_clusterring:

.. table:: **Table 3** ClusterRing

   +----------------------------+-----------------+----------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Parameter                  | Mandatory       | Type                                                                                                           | Description                    |
   +============================+=================+================================================================================================================+================================+
   | ring_hosts                 | Yes             | Array of :ref:`RingHost <en-us_topic_0000002500174034__en-us_topic_0000002340937160_request_ringhost>` objects | **Definition**                 |
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

.. _en-us_topic_0000002500174034__en-us_topic_0000002340937160_request_ringhost:

.. table:: **Table 4** RingHost

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

.. table:: **Table 5** Response body parameters

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

Scale in a logical cluster.

.. code-block:: text

   POST https://{Endpoint}/v1/9b06d044ea4f49f1a58b2bed2b0084bd/clusters/d29fd79b-d445-4e90-8fea-2f7d8b216bfflogical-clusters/7dc7a0a9-1f43-4f3c-974e-4a5ee2070836/shrink

   {
     "cluster_rings" : [ {
       "ring_hosts" : [ {
         "back_ip" : "192.168.88.41",
         "cpu_cores" : 4,
         "disk_size" : 110,
         "host_name" : "host-192-168-37-200",
         "memory" : 16
       }, {
         "back_ip" : "192.168.72.33",
         "cpu_cores" : 4,
         "disk_size" : 110,
         "host_name" : "host-192-168-6-50",
         "memory" : 16
       }, {
         "back_ip" : "192.168.90.60",
         "cpu_cores" : 4,
         "disk_size" : 110,
         "host_name" : "host-192-168-8-183",
         "memory" : 16
       } ]
     } ]
   }

Example Responses
-----------------

**Status code: 200**

The logical cluster scale-in request submitted.

.. code-block::

   {
     "error_code" : "DWS.0000",
     "error_msg" : null
   }

Status Codes
------------

=========== ===============================================
Status Code Description
=========== ===============================================
200         The logical cluster scale-in request submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
500         Internal server error.
503         Service unavailable.
=========== ===============================================
