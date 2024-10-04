:original_name: dws_02_0026.html

.. _dws_02_0026:

Creating a Snapshot
===================

Function
--------

This API is used to create snapshots for a specified cluster.

URI
---

.. code-block:: text

   POST /v1.0/{project_id}/snapshots

.. table:: **Table 1** URI parameters

   +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type   | Description                                                                                                  |
   +============+===========+========+==============================================================================================================+
   | project_id | Yes       | String | Project ID. For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------+-----------+-----------------------------------------------------------------------------+-----------------+
   | Parameter | Mandatory | Type                                                                        | Description     |
   +===========+===========+=============================================================================+=================+
   | snapshot  | Yes       | :ref:`Snapshot <en-us_topic_0000001185673172__table136481731183114>` object | Snapshot object |
   +-----------+-----------+-----------------------------------------------------------------------------+-----------------+

.. _en-us_topic_0000001185673172__table136481731183114:

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

Response Parameters
-------------------

.. table:: **Table 4** Response body parameters

   +-----------+------------------------------------------------------------------------------+-----------------+
   | Parameter | Type                                                                         | Description     |
   +===========+==============================================================================+=================+
   | snapshot  | :ref:`SnapshotResp <en-us_topic_0000001185673172__table195813546313>` object | Snapshot object |
   +-----------+------------------------------------------------------------------------------+-----------------+

.. _en-us_topic_0000001185673172__table195813546313:

.. table:: **Table 5** SnapshotResp

   ========= ====== ===========
   Parameter Type   Description
   ========= ====== ===========
   id        String Snapshot ID
   ========= ====== ===========

Example Request
---------------

.. code-block:: text

   POST https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/snapshots

   {
     "snapshot" : {
       "name" : "snapshot-3",
       "cluster_id" : "44b277eb-39be-4921-be31-3d61b43651d7",
       "description" : "Snapshot-3 description"
     }
   }

Example Response
----------------

.. code-block::

   {
       "snapshot": {
           "id": "2a4d0f86-67cd-408a-8b66-017454fb7793"
       }
   }

Status Code
-----------

=========== =====================================
Status Code Description
=========== =====================================
200         The snapshot is created.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal service error.
503         Service unavailable.
=========== =====================================
