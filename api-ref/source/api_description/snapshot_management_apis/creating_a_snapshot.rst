:original_name: dws_02_0026.html

.. _dws_02_0026:

Creating a Snapshot
===================

Function
--------

This API is used to create snapshots for a specified cluster.

URI
---

-  URI format

   .. code-block:: text

      POST /v1.0/{project_id}/snapshots

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                          |
      +============+===========+========+======================================================================================================+
      | project_id | Yes       | String | Project ID. For details about how to obtain the ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
      +------------+-----------+--------+------------------------------------------------------------------------------------------------------+

Request Message
---------------

-  Request example

   .. code-block:: text

      POST /v1.0/89cd04f168b84af6be287f71730fdb4b/snapshots
      {
          "snapshot": {
             "name": "snapshot-3",
             "cluster_id": "44b277eb-39be-4921-be31-3d61b43651d7",
             "description": "Snapshot-3 description"
           }
      }

-  Parameter description

   .. table:: **Table 2** Request parameters

      +-----------+-----------+------------------------------------------------------------------------------------------------------+------------------+
      | Parameter | Mandatory | Type                                                                                                 | Description      |
      +===========+===========+======================================================================================================+==================+
      | snapshot  | Yes       | :ref:`Snapshot <en-us_topic_0000001179325982__en-us_topic_0000001098656810_request_snapshot>` object | Snapshot object. |
      +-----------+-----------+------------------------------------------------------------------------------------------------------+------------------+

   .. _en-us_topic_0000001179325982__en-us_topic_0000001098656810_request_snapshot:

   .. table:: **Table 3** Snapshot

      +-------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter   | Mandatory | Type   | Description                                                                                                                                                                                    |
      +=============+===========+========+================================================================================================================================================================================================+
      | name        | Yes       | String | Snapshot name, which must be unique and start with a letter. It consists of 4 to 64 characters, which are case-insensitive and contain letters, digits, hyphens (-), and underscores (_) only. |
      +-------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | cluster_id  | Yes       | String | ID of the cluster for which you want to create a snapshot. For details about how to obtain the ID, see :ref:`Obtaining the Cluster ID <dws_02_00068>`.                                         |
      +-------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | description | No        | String | Snapshot description. If no value is specified, the description is empty. Enter a maximum of 256 characters. The following special characters are not allowed: !<>'=&"                         |
      +-------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response Message
----------------

-  Example response

   .. code-block::

      status CODE 200
      {
          "snapshot": {
              "id": "2a4d0f86-67cd-408a-8b66-017454fb7793"
          }
      }

-  Parameter description

   .. table:: **Table 4** Response parameter description

      +-----------+-------------------------------------------------------------------------------------------------------------+-----------------+
      | Parameter | Type                                                                                                        | Description     |
      +===========+=============================================================================================================+=================+
      | snapshot  | :ref:`SnapshotResp <en-us_topic_0000001179325982__en-us_topic_0000001098656810_response_snapshot_1>` object | Snapshot object |
      +-----------+-------------------------------------------------------------------------------------------------------------+-----------------+

   .. _en-us_topic_0000001179325982__en-us_topic_0000001098656810_response_snapshot_1:

   .. table:: **Table 5** SnapshotResp

      ========= ====== ===========
      Parameter Type   Description
      ========= ====== ===========
      id        String Snapshot ID
      ========= ====== ===========

Status Code
-----------

-  Normal

   200

-  Exception

   .. table:: **Table 6** Returned values

      ========================= ===========================
      Returned Value            Description
      ========================= ===========================
      400 Bad Request           Request error.
      401 Unauthorized          Authorization failed.
      403 Forbidden             No operation permission.
      404 Not Found             No resources found.
      500 Internal Server Error Internal service error.
      503 Service Unavailable   The service is unavailable.
      ========================= ===========================
