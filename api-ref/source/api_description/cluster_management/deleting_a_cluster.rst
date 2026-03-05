:original_name: DeleteCluster.html

.. _DeleteCluster:

Deleting a Cluster
==================

Function
--------

This API is used to delete a cluster. All resources of the deleted cluster, including customer data, will be released. For data security, you need to create a snapshot for a cluster before deleting it.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

DELETE /v1.0/{project_id}/clusters/{cluster_id}

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

   +---------------------------+-----------------+-----------------+--------------------------------------------------+
   | Parameter                 | Mandatory       | Type            | Description                                      |
   +===========================+=================+=================+==================================================+
   | keep_last_manual_snapshot | Yes             | Integer         | **Definition**                                   |
   |                           |                 |                 |                                                  |
   |                           |                 |                 | Number of snapshots to be retained in a cluster. |
   |                           |                 |                 |                                                  |
   |                           |                 |                 | **Constraints**                                  |
   |                           |                 |                 |                                                  |
   |                           |                 |                 | N/A                                              |
   |                           |                 |                 |                                                  |
   |                           |                 |                 | **Range**                                        |
   |                           |                 |                 |                                                  |
   |                           |                 |                 | N/A                                              |
   |                           |                 |                 |                                                  |
   |                           |                 |                 | **Default Value**                                |
   |                           |                 |                 |                                                  |
   |                           |                 |                 | N/A                                              |
   +---------------------------+-----------------+-----------------+--------------------------------------------------+

Response Parameters
-------------------

**Status code: 202**

The cluster is deleted successfully.

None

Example Requests
----------------

Delete a cluster.

.. code-block:: text

   DELETE https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90

   {
     "keep_last_manual_snapshot" : 0
   }

Example Responses
-----------------

**Status code: 202**

The cluster is deleted successfully.

.. code-block::

   { }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
202         The cluster is deleted successfully.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
