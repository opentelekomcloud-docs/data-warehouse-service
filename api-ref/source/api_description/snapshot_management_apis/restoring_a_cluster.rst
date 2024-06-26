:original_name: dws_02_0032.html

.. _dws_02_0032:

Restoring a Cluster
===================

Function
--------

This API is used to restore clusters using the snapshot.

URI
---

-  URI format

   .. code-block:: text

      POST /v1.0/{project_id}/snapshots/{snapshot_id}/actions

-  Parameter description

   .. table:: **Table 1** URI parameters

      +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+
      | Parameter   | Mandatory | Type   | Description                                                                                          |
      +=============+===========+========+======================================================================================================+
      | project_id  | Yes       | String | Project ID. For details about how to obtain the ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
      +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+
      | snapshot_id | Yes       | String | ID of the snapshot to be restored                                                                    |
      +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+

Request Message
---------------

-  Request example

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

-  Parameter description

   .. table:: **Table 2** Request parameters

      +-----------+-----------+-------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter | Mandatory | Type                                                                                                  | Description           |
      +===========+===========+=======================================================================================================+=======================+
      | restore   | Yes       | :ref:`Restore <en-us_topic_0000001186151624__en-us_topic_0000001145816475_table1765163917178>` object | Object to be restored |
      +-----------+-----------+-------------------------------------------------------------------------------------------------------+-----------------------+

   .. _en-us_topic_0000001186151624__en-us_topic_0000001145816475_table1765163917178:

   .. table:: **Table 3** Restore

      +-----------------------+-----------+-------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter             | Mandatory | Type                                                                                                  | Description                                                                                                                                                                               |
      +=======================+===========+=======================================================================================================+===========================================================================================================================================================================================+
      | name                  | Yes       | String                                                                                                | Cluster name, which must be unique. The cluster name must contain 4 to 64 characters, which must start with a letter. Only letters, digits, hyphens (-), and underscores (_) are allowed. |
      +-----------------------+-----------+-------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | subnet_id             | No        | String                                                                                                | Subnet ID, which is used for configuring cluster network. The default value is the same as that of the original cluster.                                                                  |
      +-----------------------+-----------+-------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | security_group_id     | No        | String                                                                                                | Security group ID, which is used for configuring cluster network. The default value is the same as that of the original cluster.                                                          |
      +-----------------------+-----------+-------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | vpc_id                | No        | String                                                                                                | VPC ID, which is used for configuring cluster network. The default value is the same as that of the original cluster.                                                                     |
      +-----------------------+-----------+-------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | availability_zone     | No        | String                                                                                                | AZ of a cluster. The default value is the same as that of the original cluster.                                                                                                           |
      +-----------------------+-----------+-------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | port                  | No        | Integer                                                                                               | Service port of a cluster. The value ranges from 8000 to 30000. The default value is **8000**.                                                                                            |
      +-----------------------+-----------+-------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | public_ip             | No        | :ref:`PublicIp <en-us_topic_0000001186151624__en-us_topic_0000001145816475_request_public_ip>` object | Public IP address. If the parameter is not specified, public connection is not used by default.                                                                                           |
      +-----------------------+-----------+-------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | enterprise_project_id | No        | String                                                                                                | Enterprise project. The default enterprise project ID is **0**.                                                                                                                           |
      +-----------------------+-----------+-------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. _en-us_topic_0000001186151624__en-us_topic_0000001145816475_request_public_ip:

   .. table:: **Table 4** PublicIp

      +------------------+-----------------+-----------------+----------------------------------------------------------------+
      | Parameter        | Mandatory       | Type            | Description                                                    |
      +==================+=================+=================+================================================================+
      | public_bind_type | Yes             | String          | Binding type of an EIP. The value can be one of the following: |
      |                  |                 |                 |                                                                |
      |                  |                 |                 | -  auto_assign                                                 |
      |                  |                 |                 | -  **not_use**                                                 |
      |                  |                 |                 | -  **bind_existing**                                           |
      +------------------+-----------------+-----------------+----------------------------------------------------------------+
      | eip_id           | No              | String          | EIP ID                                                         |
      +------------------+-----------------+-----------------+----------------------------------------------------------------+

Response Message
----------------

-  Example response

   .. code-block::

      {
          "cluster": {
              "id": "7d85f602-a948-4a30-afd4-e84f47471c15"
           }
      }

-  Parameter description

   .. table:: **Table 5** Response parameter description

      +-----------+-----------------------------------------------------------------------------------------------------+----------------+
      | Parameter | Type                                                                                                | Description    |
      +===========+=====================================================================================================+================+
      | cluster   | :ref:`Cluster <en-us_topic_0000001186151624__en-us_topic_0000001145816475_response_cluster>` object | Cluster object |
      +-----------+-----------------------------------------------------------------------------------------------------+----------------+

   .. _en-us_topic_0000001186151624__en-us_topic_0000001145816475_response_cluster:

   .. table:: **Table 6** Cluster

      ========= ====== ===========
      Parameter Type   Description
      ========= ====== ===========
      id        String Cluster ID
      ========= ====== ===========

Status Code
-----------

-  Normal

   200

-  Exception

   .. table:: **Table 7** Returned values

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
