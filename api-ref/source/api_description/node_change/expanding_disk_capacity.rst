:original_name: ExpandInstanceStorage.html

.. _ExpandInstanceStorage:

Expanding Disk Capacity
=======================

Function
--------

Disk capacity is more likely to become the bottleneck of storage as workloads develop. When other resources are sufficient, disk capacity expansion can help you quickly break through the bottleneck without interrupting services or wasting CPU and memory resources.

**Constraints**

Disk capacity expansion can be performed only for cloud data warehouses using SSD or hybrid data warehouses. Only version 8.1.1.203 and later are supported.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v1.0/{project_id}/clusters/{cluster_id}/expand-instance-storage

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

   +-----------+-----------+---------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   +===========+===========+=========+====================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | new_size  | Yes       | Integer | **Definition**\ Available capacity of a single node after disk expansion, in GB. The value must be greater than the existing capacity of a single node and up to the maximum capacity of a single node supported by the cluster specifications. The value must be a multiple of the step supported by the cluster specifications. To query cluster flavors, see :ref:`Querying Cluster Flavors <showclusterflavor>`.\ **Constraints**\ N/A\ **Range**\ N/A\ **Default Value**\ N/A |
   +-----------+-----------+---------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response Parameters
-------------------

**Status code: 200**

Request for expanding the disk capacity submitted.

None

Example Requests
----------------

Expand the cluster disk capacity to 200 GB on a single node.

.. code-block:: text

   POST https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/b5c45780-1006-49e3-b2d5-b3229975bbc7/expand-instance-storage

   {
     "new_size" : 200
   }

Example Responses
-----------------

**Status code: 200**

Request for expanding the disk capacity submitted.

.. code-block::

   { }

Status Codes
------------

=========== ==================================================
Status Code Description
=========== ==================================================
200         Request for expanding the disk capacity submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ==================================================
