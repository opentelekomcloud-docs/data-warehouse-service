:original_name: DeleteDwsCluster.html

.. _DeleteDwsCluster:

Deleting a Cluster (V2)
=======================

Function
--------

This API is used to delete a cluster. All resources of the deleted cluster, including customer data, will be released. For data security, you need to create a snapshot for a cluster before deleting it.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

DELETE /v2/{project_id}/clusters/{cluster_id}

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

   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory       | Type            | Description                                                                                                      |
   +=========================+=================+=================+==================================================================================================================+
   | keep_last_manual_backup | No              | String          | **Definition**                                                                                                   |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | Number of snapshots to be retained in a cluster.                                                                 |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | **Constraints**                                                                                                  |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | N/A                                                                                                              |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | **Range**                                                                                                        |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | N/A                                                                                                              |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | **Default Value**                                                                                                |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | 0                                                                                                                |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------+
   | release_eip_type        | No              | String          | **Definition**                                                                                                   |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | Whether the EIP is released. The default value is **NO_RELEASE**, indicating that the bound EIP is not released. |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | **Constraints**                                                                                                  |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | N/A                                                                                                              |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | **Range**                                                                                                        |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | **NO_RELEASE**: The bound EIP is not released.                                                                   |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | **RELEASE_BINDING**: The bound EIP is released.                                                                  |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | **Default Value**                                                                                                |
   |                         |                 |                 |                                                                                                                  |
   |                         |                 |                 | NO_RELEASE                                                                                                       |
   +-------------------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 202**

Cluster deleted.

None

Example Requests
----------------

Delete a cluster.

.. code-block:: text

   DELETE https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90

Example Responses
-----------------

None

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
202         Cluster deleted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
