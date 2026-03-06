:original_name: UpdateLogicalCluster.html

.. _UpdateLogicalCluster:

Editing a Logical Cluster
=========================

Function
--------

This API is used to edit a logical cluster. The API determines whether to scale out or scale in a logical cluster based on the submitted request body.

Scenario 1: The original logical cluster has six nodes (two rings). If the request body submitted contains only one ring, the logical cluster is scaled in.

Scenario 2: The original logical cluster has six nodes (two rings). If the request body submitted contains three rings, the logical cluster is scaled out.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

PUT /v2/{project_id}/clusters/{cluster_id}/logical-clusters/{logical_cluster_id}

.. table:: **Table 1** Path Parameters

   +--------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+
   | Parameter          | Mandatory       | Type            | Description                                                                                                |
   +====================+=================+=================+============================================================================================================+
   | cluster_id         | Yes             | String          | **Definition**                                                                                             |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | Cluster ID. For details about how to obtain the value, see :ref:`Obtaining the Cluster ID <dws_02_00068>`. |
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
   | logical_cluster_id | Yes             | String          | **Definition**                                                                                             |
   |                    |                 |                 |                                                                                                            |
   |                    |                 |                 | ID of the logical cluster to be edited.                                                                    |
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

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +---------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+
   | Parameter           | Mandatory       | Type                                                                                                                 | Description                                                   |
   +=====================+=================+======================================================================================================================+===============================================================+
   | cluster_rings       | Yes             | Array of :ref:`ClusterRing <en-us_topic_0000002532013959__en-us_topic_0000002374935137_request_clusterring>` objects | **Definition**                                                |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | Information for editing the ring list of the logical cluster. |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | **Constraints**                                               |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | N/A                                                           |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | **Range**                                                     |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | N/A                                                           |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | **Default Value**                                             |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | N/A                                                           |
   +---------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+
   | mode                | No              | String                                                                                                               | **Definition**                                                |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | Redistribution mode.                                          |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | **Constraints**                                               |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | N/A                                                           |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | **Range**                                                     |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | N/A                                                           |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | **Default Value**                                             |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | N/A                                                           |
   +---------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+
   | waiting_for_killing | No              | Integer                                                                                                              | **Definition**                                                |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | Time before job termination.                                  |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | **Constraints**                                               |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | N/A                                                           |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | **Range**                                                     |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | N/A                                                           |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | **Default Value**                                             |
   |                     |                 |                                                                                                                      |                                                               |
   |                     |                 |                                                                                                                      | N/A                                                           |
   +---------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+

.. _en-us_topic_0000002532013959__en-us_topic_0000002374935137_request_clusterring:

.. table:: **Table 3** ClusterRing

   +----------------------------+-----------------+----------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Parameter                  | Mandatory       | Type                                                                                                           | Description                    |
   +============================+=================+================================================================================================================+================================+
   | ring_hosts                 | Yes             | Array of :ref:`RingHost <en-us_topic_0000002532013959__en-us_topic_0000002374935137_request_ringhost>` objects | **Definition**                 |
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

.. _en-us_topic_0000002532013959__en-us_topic_0000002374935137_request_ringhost:

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

Submit the request for scaling in the logical cluster. After the request is submitted, the logical cluster has only one ring (three nodes).

.. code-block:: text

   PUT https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/logical-clusters/0b494d0d-8431-4c4f-8a06-2cc42d0d0c7d

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
     "mode" : null,
     "waiting_for_killing" : 0
   }

Example Responses
-----------------

**Status code: 200**

Request for editing a logical cluster submitted.

.. code-block::

   {
     "error_code" : "DWS.0000",
     "error_msg" : null
   }

Status Codes
------------

=========== ================================================
Status Code Description
=========== ================================================
200         Request for editing a logical cluster submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ================================================
