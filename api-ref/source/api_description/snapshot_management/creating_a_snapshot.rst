:original_name: CreateSnapshot.html

.. _CreateSnapshot:

Creating a Snapshot
===================

Function
--------

This API is used to create a snapshot for a specified cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v1.0/{project_id}/snapshots

.. table:: **Table 1** Path Parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                       |
   +=================+=================+=================+===================================================================================+
   | project_id      | Yes             | String          | **Definition**                                                                    |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | Project ID. To obtain the value, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Constraints**                                                                   |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Range**                                                                         |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Default Value**                                                                 |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+------------------------------------------------------------------------------------------------------+-------------------+
   | Parameter       | Mandatory       | Type                                                                                                 | Description       |
   +=================+=================+======================================================================================================+===================+
   | snapshot        | Yes             | :ref:`Snapshot <en-us_topic_0000002500173974__en-us_topic_0000002340937352_request_snapshot>` object | **Definition**    |
   |                 |                 |                                                                                                      |                   |
   |                 |                 |                                                                                                      | Snapshot object.  |
   |                 |                 |                                                                                                      |                   |
   |                 |                 |                                                                                                      | **Constraints**   |
   |                 |                 |                                                                                                      |                   |
   |                 |                 |                                                                                                      | N/A               |
   |                 |                 |                                                                                                      |                   |
   |                 |                 |                                                                                                      | **Range**         |
   |                 |                 |                                                                                                      |                   |
   |                 |                 |                                                                                                      | N/A               |
   |                 |                 |                                                                                                      |                   |
   |                 |                 |                                                                                                      | **Default Value** |
   |                 |                 |                                                                                                      |                   |
   |                 |                 |                                                                                                      | N/A               |
   +-----------------+-----------------+------------------------------------------------------------------------------------------------------+-------------------+

.. _en-us_topic_0000002500173974__en-us_topic_0000002340937352_request_snapshot:

.. table:: **Table 3** Snapshot

   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                            |
   +=================+=================+=================+========================================================================================================================================================================================+
   | name            | Yes             | String          | **Definition**                                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                        |
   |                 |                 |                 | Snapshot name. It must be unique and start with a letter. It consists of 4 to 64 characters. Only letters (case-insensitive), digits, hyphens (-), and underscores (_) are allowed.    |
   |                 |                 |                 |                                                                                                                                                                                        |
   |                 |                 |                 | **Range**                                                                                                                                                                              |
   |                 |                 |                 |                                                                                                                                                                                        |
   |                 |                 |                 | N/A                                                                                                                                                                                    |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cluster_id      | Yes             | String          | **Definition**                                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                        |
   |                 |                 |                 | ID of the cluster for which you want to create a snapshot.                                                                                                                             |
   |                 |                 |                 |                                                                                                                                                                                        |
   |                 |                 |                 | **Range**                                                                                                                                                                              |
   |                 |                 |                 |                                                                                                                                                                                        |
   |                 |                 |                 | N/A                                                                                                                                                                                    |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | description     | No              | String          | **Definition**                                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                        |
   |                 |                 |                 | Snapshot description. If no value is specified, the description is empty. The value can contain a maximum of 256 characters. The following special characters are not allowed: !<>'=&" |
   |                 |                 |                 |                                                                                                                                                                                        |
   |                 |                 |                 | **Range**                                                                                                                                                                              |
   |                 |                 |                 |                                                                                                                                                                                        |
   |                 |                 |                 | N/A                                                                                                                                                                                    |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 4** Response body parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+
   | Parameter             | Type                                                                                                          | Description           |
   +=======================+===============================================================================================================+=======================+
   | snapshot              | :ref:`SnapshotResp <en-us_topic_0000002500173974__en-us_topic_0000002340937352_response_snapshotresp>` object | **Definition**        |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | Snapshot object.      |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | **Range**             |
   |                       |                                                                                                               |                       |
   |                       |                                                                                                               | N/A                   |
   +-----------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+

.. _en-us_topic_0000002500173974__en-us_topic_0000002340937352_response_snapshotresp:

.. table:: **Table 5** SnapshotResp

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | id                    | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Snapshot ID.          |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

Create a manual snapshot named **snapshot-3** for the cluster whose ID is **44b277eb-39be-4921-be31-3d61b43651d7**.

.. code-block:: text

   POST https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/snapshots

   {
     "snapshot" : {
       "name" : "snapshot-3",
       "cluster_id" : "44b277eb-39be-4921-be31-3d61b43651d7",
       "description" : "Snapshot-3 description"
     }
   }

Example Responses
-----------------

**Status code: 200**

The snapshot is created.

.. code-block::

   {
     "snapshot" : {
       "id" : "2a4d0f86-67cd-408a-8b66-017454fb7793"
     }
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         The snapshot is created.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
