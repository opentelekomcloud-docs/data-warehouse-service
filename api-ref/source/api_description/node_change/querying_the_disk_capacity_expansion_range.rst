:original_name: ShowClusterStorageExpandRange.html

.. _ShowClusterStorageExpandRange:

Querying the Disk Capacity Expansion Range
==========================================

Function
--------

This API is used to query capacity range that a disk can be expanded to.

**Constraints**

Disk capacity expansion can be performed only for cloud data warehouses using SSD or hybrid data warehouses. Only version 8.1.1.203 and later are supported.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1/{project_id}/clusters/{cluster_id}/storage-expand-range

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

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Type                  | Description                                                                                                                                                                                                        |
   +=======================+=======================+====================================================================================================================================================================================================================+
   | min_size              | Integer               | **Definition**                                                                                                                                                                                                     |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | Minimum disk capacity of a single node after the expansion, in GB.                                                                                                                                                 |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | **Range**                                                                                                                                                                                                          |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | N/A                                                                                                                                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_size              | Integer               | **Definition**                                                                                                                                                                                                     |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | Maximum disk capacity of a single node after the expansion, in GB.                                                                                                                                                 |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | **Range**                                                                                                                                                                                                          |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | N/A                                                                                                                                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | current_size          | Integer               | **Definition**                                                                                                                                                                                                     |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | Current disk capacity, in GB.                                                                                                                                                                                      |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | **Range**                                                                                                                                                                                                          |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | N/A                                                                                                                                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | step                  | Integer               | **Definition**                                                                                                                                                                                                     |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | How much the disk capacity is expanded at a time, in GB. For example, if the disk capacity of a single node is 20 GB and this parameter is set to **20**, the disk capacity after the expansion is at least 40 GB. |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | **Range**                                                                                                                                                                                                          |
   |                       |                       |                                                                                                                                                                                                                    |
   |                       |                       | Greater than or equal to **10**                                                                                                                                                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Requests
----------------

Query the disk capacity expansion range of the current cluster.

.. code-block::

   get https://{Endpoint}/v1/89cd04f168b84af6be287f71730fdb4b/clusters/b5c45780-1006-49e3-b2d5-b3229975bbc7/storage-expand-range

Example Responses
-----------------

**Status code: 200**

The request for expanding the disk capacity is submitted.

.. code-block::

   {
     "min_size" : 20,
     "max_size" : 2000,
     "current_size" : 20,
     "step" : 10
   }

Status Codes
------------

=========== =========================================================
Status Code Description
=========== =========================================================
200         The request for expanding the disk capacity is submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =========================================================
