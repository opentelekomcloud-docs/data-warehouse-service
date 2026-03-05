:original_name: UpdateSchemas.html

.. _UpdateSchemas:

Editing the Space Limit of a Schema
===================================

Function
--------

This API is used to edit the space limit of a schema.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

PUT /v2/{project_id}/clusters/{cluster_id}/databases/{database_name}/schemas

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
   | database_name   | Yes             | String          | **Definition**                                                                                             |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | Database name.                                                                                             |
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

   +-----------------+-----------------+-----------------+------------------------+
   | Parameter       | Mandatory       | Type            | Description            |
   +=================+=================+=================+========================+
   | schema_name     | Yes             | String          | **Definition**         |
   |                 |                 |                 |                        |
   |                 |                 |                 | Schema space name      |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Constraints**        |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Range**              |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Default Value**      |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   +-----------------+-----------------+-----------------+------------------------+
   | perm_space      | Yes             | String          | **Definition**         |
   |                 |                 |                 |                        |
   |                 |                 |                 | Schema space threshold |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Constraints**        |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Range**              |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   |                 |                 |                 |                        |
   |                 |                 |                 | **Default Value**      |
   |                 |                 |                 |                        |
   |                 |                 |                 | N/A                    |
   +-----------------+-----------------+-----------------+------------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | ret_code              | Integer               | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Response code         |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

.. code-block:: text

   PUT https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/databases/gaussdb/schemas

   {
     "schema_name" : "gs_logical_cluster",
     "perm_space" : 10240
   }

Example Responses
-----------------

**Status code: 200**

Schema space limit edited.

.. code-block::

   {
     "ret_code" : 0
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Schema space limit edited.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
