:original_name: BatchCreateClusterCn.html

.. _BatchCreateClusterCn:

Adding CN Nodes in Batches
==========================

Function
--------

After a cluster is created, the number of required CN nodes varies with workloads. You can add or delete CN nodes as needed.

-  Other O&M operations cannot be performed during CN addition or deletion.

-  Services need to be stopped during CN node addition or deletion. You are advised to perform this operation during off-peak hours or when services are interrupted.

-  If a fault occurs during CN node addition or deletion and the rollback fails, you need to log in to the backend to rectify the fault. For details, see "Cluster Usage" > "Failed to Roll Back CN Addition or Deletion" in *Troubleshooting*.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v1.0/{project_id}/clusters/{cluster_id}/cns/batch-create

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

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                                                                                                                                                                 |
   +=================+=================+=================+=============================================================================================================================================================================================================================================================================================================================================================+
   | num             | Yes             | Integer         | **Definition**                                                                                                                                                                                                                                                                                                                                              |
   |                 |                 |                 |                                                                                                                                                                                                                                                                                                                                                             |
   |                 |                 |                 | Total number of CN nodes in the cluster after new CN nodes are added in batches. The number of CN nodes supported by a cluster depends on the cluster version and number of nodes. For details, see "Querying CN Nodes in a Cluster", where **min_num** indicates the minimum number of CN nodes, and **max_num** indicates the maximum number of CN nodes. |
   |                 |                 |                 |                                                                                                                                                                                                                                                                                                                                                             |
   |                 |                 |                 | **Constraints**                                                                                                                                                                                                                                                                                                                                             |
   |                 |                 |                 |                                                                                                                                                                                                                                                                                                                                                             |
   |                 |                 |                 | N/A                                                                                                                                                                                                                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                                                                                                                                                                                             |
   |                 |                 |                 | **Range**                                                                                                                                                                                                                                                                                                                                                   |
   |                 |                 |                 |                                                                                                                                                                                                                                                                                                                                                             |
   |                 |                 |                 | N/A                                                                                                                                                                                                                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                                                                                                                                                                                             |
   |                 |                 |                 | **Default Value**                                                                                                                                                                                                                                                                                                                                           |
   |                 |                 |                 |                                                                                                                                                                                                                                                                                                                                                             |
   |                 |                 |                 | N/A                                                                                                                                                                                                                                                                                                                                                         |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-----------------------+------------------------------------------------+
   | Parameter             | Type                  | Description                                    |
   +=======================+=======================+================================================+
   | job_id                | String                | **Definition**                                 |
   |                       |                       |                                                |
   |                       |                       | ID of the task for adding CN nodes in batches. |
   |                       |                       |                                                |
   |                       |                       | **Range**                                      |
   |                       |                       |                                                |
   |                       |                       | N/A                                            |
   +-----------------------+-----------------------+------------------------------------------------+

Example Requests
----------------

Add three CN nodes in batches.

.. code-block:: text

   POST https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/b5c45780-1006-49e3-b2d5-b3229975bbc7/cns/batch-create

   {
     "num" : 3
   }

Example Responses
-----------------

**Status code: 200**

The CN nodes are successfully added in batches.

.. code-block::

   {
     "job_id" : "2c908185841339ce018414e9944b0020"
   }

Status Codes
------------

=========== ===============================================
Status Code Description
=========== ===============================================
200         The CN nodes are successfully added in batches.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ===============================================
