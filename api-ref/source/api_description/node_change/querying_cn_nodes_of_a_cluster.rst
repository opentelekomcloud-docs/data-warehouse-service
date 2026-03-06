:original_name: ListClusterCn.html

.. _ListClusterCn:

Querying CN Nodes of a Cluster
==============================

Function
--------

This API is used to query the CN node list of a cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1.0/{project_id}/clusters/{cluster_id}/cns

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

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------+
   | Parameter             | Type                                                                                                                          | Description                                          |
   +=======================+===============================================================================================================================+======================================================+
   | min_num               | Integer                                                                                                                       | **Definition**                                       |
   |                       |                                                                                                                               |                                                      |
   |                       |                                                                                                                               | Minimum number of CN nodes supported by the cluster. |
   |                       |                                                                                                                               |                                                      |
   |                       |                                                                                                                               | **Range**                                            |
   |                       |                                                                                                                               |                                                      |
   |                       |                                                                                                                               | N/A                                                  |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------+
   | max_num               | Integer                                                                                                                       | **Definition**                                       |
   |                       |                                                                                                                               |                                                      |
   |                       |                                                                                                                               | Maximum number of CN nodes supported by the cluster. |
   |                       |                                                                                                                               |                                                      |
   |                       |                                                                                                                               | **Range**                                            |
   |                       |                                                                                                                               |                                                      |
   |                       |                                                                                                                               | N/A                                                  |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------+
   | instances             | Array of :ref:`CoordinatorNode <en-us_topic_0000002531893895__en-us_topic_0000002374935109_response_coordinatornode>` objects | **Definition**                                       |
   |                       |                                                                                                                               |                                                      |
   |                       |                                                                                                                               | List of CN details.                                  |
   |                       |                                                                                                                               |                                                      |
   |                       |                                                                                                                               | **Range**                                            |
   |                       |                                                                                                                               |                                                      |
   |                       |                                                                                                                               | N/A                                                  |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------+

.. _en-us_topic_0000002531893895__en-us_topic_0000002374935109_response_coordinatornode:

.. table:: **Table 3** CoordinatorNode

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | id                    | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Node ID.              |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | name                  | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Node name.            |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | private_ip            | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Private IP address.   |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

Query CN node list of a cluster.

.. code-block:: text

   GET https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/cns

Example Responses
-----------------

**Status code: 200**

CN nodes of the cluster queried.

.. code-block::

   {
     "min_num" : 2,
     "max_num" : 3,
     "instances" : [ {
       "id" : "e07d1bfb-6eac-4827-96e0-d10b8ca29c41",
       "name" : "demo-dws-cn-cn-1-1",
       "private_ip" : "172.16.69.106"
     } ]
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         CN nodes of the cluster queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
