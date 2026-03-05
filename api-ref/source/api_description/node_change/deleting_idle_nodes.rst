:original_name: DeleteClusterNodes.html

.. _DeleteClusterNodes:

Deleting Idle Nodes
===================

Function
--------

This API is used to delete idle nodes.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v2/{project_id}/clusters/{cluster_id}/nodes/delete

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

   +-----------------+-----------------+------------------+----------------------------------------------------------+
   | Parameter       | Mandatory       | Type             | Description                                              |
   +=================+=================+==================+==========================================================+
   | node_list       | Yes             | Array of strings | **Definition**                                           |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | List of idle node IDs.                                   |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | **Constraints**                                          |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | N/A                                                      |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | **Range**                                                |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | N/A                                                      |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | **Default Value**                                        |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | N/A                                                      |
   +-----------------+-----------------+------------------+----------------------------------------------------------+
   | operate_type    | Yes             | String           | **Definition**                                           |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | Operation type. Generally, the value is **delete**.      |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | **Constraints**                                          |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | N/A                                                      |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | **Range**                                                |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | **clear**: clearing idle nodes that failed to be created |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | **delete**: deleting idle nodes                          |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | **Default Value**                                        |
   |                 |                 |                  |                                                          |
   |                 |                 |                  | N/A                                                      |
   +-----------------+-----------------+------------------+----------------------------------------------------------+

Response Parameters
-------------------

**Status code: 202**

.. table:: **Table 3** Response body parameters

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

Delete idle nodes.

.. code-block:: text

   POST https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/nodes/delete

   {
     "node_list" : [ "16413746-258e-4a3c-bea9-8496fdbefde3" ],
     "operate_type" : "delete"
   }

Example Responses
-----------------

**Status code: 202**

Request for deleting cluster nodes submitted.

.. code-block::

   {
     "error_code" : "DWS.0000",
     "error_msg" : null
   }

Status Codes
------------

=========== =============================================
Status Code Description
=========== =============================================
202         Request for deleting cluster nodes submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =============================================
