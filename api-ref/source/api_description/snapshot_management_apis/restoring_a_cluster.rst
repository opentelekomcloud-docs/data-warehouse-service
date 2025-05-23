:original_name: dws_02_0032.html

.. _dws_02_0032:

Restoring a Cluster
===================

Function
--------

This API is used to restore clusters using the snapshot.

URI
---

.. code-block:: text

   POST /v1.0/{project_id}/snapshots/{snapshot_id}/actions

.. table:: **Table 1** URI parameters

   +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type   | Description                                                                                          |
   +=============+===========+========+======================================================================================================+
   | project_id  | Yes       | String | Project ID. For details about how to obtain the ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+
   | snapshot_id | Yes       | String | ID of the snapshot to be restored                                                                    |
   +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------+-----------+---------------------------------------------------------------------------+-----------------------+
   | Parameter | Mandatory | Type                                                                      | Description           |
   +===========+===========+===========================================================================+=======================+
   | restore   | Yes       | :ref:`Restore <en-us_topic_0000001186151624__table15447172418545>` object | Object to be restored |
   +-----------+-----------+---------------------------------------------------------------------------+-----------------------+

.. _en-us_topic_0000001186151624__table15447172418545:

.. table:: **Table 3** Restore

   +-----------------------+-----------+----------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory | Type                                                                       | Description                                                                                                                                                                               |
   +=======================+===========+============================================================================+===========================================================================================================================================================================================+
   | name                  | Yes       | String                                                                     | Cluster name, which must be unique. The cluster name must contain 4 to 64 characters, which must start with a letter. Only letters, digits, hyphens (-), and underscores (_) are allowed. |
   +-----------------------+-----------+----------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | subnet_id             | No        | String                                                                     | Subnet ID, which is used for configuring cluster network. The default value is the same as that of the original cluster.                                                                  |
   +-----------------------+-----------+----------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | security_group_id     | No        | String                                                                     | Security group ID, which is used for configuring cluster network. The default value is the same as that of the original cluster.                                                          |
   +-----------------------+-----------+----------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | vpc_id                | No        | String                                                                     | VPC ID, which is used for configuring cluster network. The default value is the same as that of the original cluster.                                                                     |
   +-----------------------+-----------+----------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | availability_zone     | No        | String                                                                     | AZ of a cluster. The default value is the same as that of the original cluster.                                                                                                           |
   +-----------------------+-----------+----------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | port                  | No        | Integer                                                                    | Service port of a cluster. The value ranges from 8000 to 30000. The default value is **8000**.                                                                                            |
   +-----------------------+-----------+----------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | public_ip             | No        | :ref:`PublicIp <en-us_topic_0000001186151624__table44541224145418>` object | Public IP address. If the parameter is not specified, public connection is not used by default.                                                                                           |
   +-----------------------+-----------+----------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | enterprise_project_id | No        | String                                                                     | Enterprise project. The default enterprise project ID is **0**.                                                                                                                           |
   +-----------------------+-----------+----------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001186151624__table44541224145418:

.. table:: **Table 4** PublicIp

   +------------------+-----------------+-----------------+----------------------------------------------------------------+
   | Parameter        | Mandatory       | Type            | Description                                                    |
   +==================+=================+=================+================================================================+
   | public_bind_type | Yes             | String          | Binding type of an EIP. The value can be one of the following: |
   |                  |                 |                 |                                                                |
   |                  |                 |                 | -  **auto_assign**                                             |
   |                  |                 |                 | -  **not_use**                                                 |
   |                  |                 |                 | -  **bind_existing**                                           |
   +------------------+-----------------+-----------------+----------------------------------------------------------------+
   | eip_id           | No              | String          | EIP ID                                                         |
   +------------------+-----------------+-----------------+----------------------------------------------------------------+

Response Parameters
-------------------

.. table:: **Table 5** Response body parameters

   +-----------+----------------------------------------------------------------------------+----------------+
   | Parameter | Type                                                                       | Description    |
   +===========+============================================================================+================+
   | cluster   | :ref:`Cluster <en-us_topic_0000001186151624__table153641059125418>` object | Cluster object |
   +-----------+----------------------------------------------------------------------------+----------------+

.. _en-us_topic_0000001186151624__table153641059125418:

.. table:: **Table 6** Cluster

   ========= ====== ===========
   Parameter Type   Description
   ========= ====== ===========
   id        String Cluster ID
   ========= ====== ===========

Example Request
---------------

.. code-block:: text

   POST
   https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/snapshots/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/actions
   {"restore": {
           "name": "dws-1",
           "subnet_id": "374eca02-cfc4-4de7-8ab5-dbebf7d9a720",
           "security_group_id": "dc3ec145-9029-4b39-b5a3-ace5a01f772b",
           "vpc_id": "85b20d7e-9eb7-4b2a-98f3-3c8843ea3574",
           "availability_zone": "eu-de-01",
           "port": 8000,
           "public_ip": {
               "public_bind_type": "auto_assign",
               "eip_id": ""
           },
           "enterprise_project_id":"aca4e50a-266f-4786-827c-f8d6cc3fbada"
       }
   }

Example Responses
-----------------

.. code-block::

   {
       "cluster": {
           "id": "7d85f602-a948-4a30-afd4-e84f47471c15"
        }
   }

Status Code
-----------

=========== =====================================
Status Code Description
=========== =====================================
200         The cluster is restored.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal service error.
503         Service unavailable.
=========== =====================================
