:original_name: RestartLogicalCluster.html

.. _RestartLogicalCluster:

Restarting a Logical Cluster
============================

Function
--------

This API is used to restart a logical cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v2/{project_id}/clusters/{cluster_id}/logical-clusters/{logical_cluster_id}/restart

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
   |                    |                 |                 | ID of the logical cluster to be restarted.                                                                 |
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

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 2** Response body parameters

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

Restart a logical cluster.

.. code-block:: text

   POST https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/logical-clusters/0b494d0d-8431-4c4f-8a06-2cc42d0d0c7d/restart

Example Responses
-----------------

**Status code: 200**

Request for restarting a cluster submitted.

.. code-block::

   {
     "error_code" : "DWS.0000",
     "error_msg" : null
   }

Status Codes
------------

=========== ===========================================
Status Code Description
=========== ===========================================
200         Request for restarting a cluster submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ===========================================
