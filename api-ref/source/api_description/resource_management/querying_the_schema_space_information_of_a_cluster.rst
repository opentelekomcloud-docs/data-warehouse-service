:original_name: ListSchemas.html

.. _ListSchemas:

Querying the Schema Space Information of a Cluster
==================================================

Function
--------

This API is used to query schema space information of a cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/clusters/{cluster_id}/databases/{database_name}/schemas

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

.. table:: **Table 2** Query Parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                             |
   +=================+=================+=================+=========================================================+
   | sort_key        | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Sorting field.                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **schemaName**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | sort_dir        | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Sorting field.                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **ASC**                                                 |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **DESC**                                                |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | keywords        | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Keyword entered for search.                             |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | limit           | No              | Integer         | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Size of a single page.                                  |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Greater than 0                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | 10                                                      |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | offset          | No              | Integer         | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Page offset, which starts from 0 (page number minus 1). |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Greater than or equal to **0**                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | 0                                                       |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------------+--------------------------+
   | Parameter             | Type                                                                                                                | Description              |
   +=======================+=====================================================================================================================+==========================+
   | schemas               | Array of :ref:`SchemaInfo <en-us_topic_0000002531893993__en-us_topic_0000002374935241_response_schemainfo>` objects | **Definition**           |
   |                       |                                                                                                                     |                          |
   |                       |                                                                                                                     | Schema information list. |
   |                       |                                                                                                                     |                          |
   |                       |                                                                                                                     | **Range**                |
   |                       |                                                                                                                     |                          |
   |                       |                                                                                                                     | N/A                      |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------+--------------------------+
   | count                 | Integer                                                                                                             | **Definition**           |
   |                       |                                                                                                                     |                          |
   |                       |                                                                                                                     | Total number.            |
   |                       |                                                                                                                     |                          |
   |                       |                                                                                                                     | **Range**                |
   |                       |                                                                                                                     |                          |
   |                       |                                                                                                                     | N/A                      |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------+--------------------------+

.. _en-us_topic_0000002531893993__en-us_topic_0000002374935241_response_schemainfo:

.. table:: **Table 4** SchemaInfo

   +-----------------------+-----------------------+-----------------------------------------+
   | Parameter             | Type                  | Description                             |
   +=======================+=======================+=========================================+
   | schema_name           | String                | **Definition**                          |
   |                       |                       |                                         |
   |                       |                       | Cluster schema name.                    |
   |                       |                       |                                         |
   |                       |                       | **Range**                               |
   |                       |                       |                                         |
   |                       |                       | N/A                                     |
   +-----------------------+-----------------------+-----------------------------------------+
   | database_name         | String                | **Definition**                          |
   |                       |                       |                                         |
   |                       |                       | Database name.                          |
   |                       |                       |                                         |
   |                       |                       | **Range**                               |
   |                       |                       |                                         |
   |                       |                       | N/A                                     |
   +-----------------------+-----------------------+-----------------------------------------+
   | total_value           | Integer               | **Definition**                          |
   |                       |                       |                                         |
   |                       |                       | Total space used by the cluster schema. |
   |                       |                       |                                         |
   |                       |                       | **Range**                               |
   |                       |                       |                                         |
   |                       |                       | N/A                                     |
   +-----------------------+-----------------------+-----------------------------------------+
   | perm_space            | Integer               | **Definition**                          |
   |                       |                       |                                         |
   |                       |                       | Space threshold for the cluster schema. |
   |                       |                       |                                         |
   |                       |                       | **Range**                               |
   |                       |                       |                                         |
   |                       |                       | N/A                                     |
   +-----------------------+-----------------------+-----------------------------------------+
   | skew_percent          | Double                | **Definition**                          |
   |                       |                       |                                         |
   |                       |                       | Skew ratio.                             |
   |                       |                       |                                         |
   |                       |                       | **Range**                               |
   |                       |                       |                                         |
   |                       |                       | N/A                                     |
   +-----------------------+-----------------------+-----------------------------------------+
   | min_value             | Integer               | **Definition**                          |
   |                       |                       |                                         |
   |                       |                       | Minimum value.                          |
   |                       |                       |                                         |
   |                       |                       | **Range**                               |
   |                       |                       |                                         |
   |                       |                       | N/A                                     |
   +-----------------------+-----------------------+-----------------------------------------+
   | max_value             | Integer               | **Definition**                          |
   |                       |                       |                                         |
   |                       |                       | Maximum value.                          |
   |                       |                       |                                         |
   |                       |                       | **Range**                               |
   |                       |                       |                                         |
   |                       |                       | N/A                                     |
   +-----------------------+-----------------------+-----------------------------------------+
   | min_dn                | String                | **Definition**                          |
   |                       |                       |                                         |
   |                       |                       | Minimum number of DN nodes.             |
   |                       |                       |                                         |
   |                       |                       | **Range**                               |
   |                       |                       |                                         |
   |                       |                       | N/A                                     |
   +-----------------------+-----------------------+-----------------------------------------+
   | max_dn                | String                | **Definition**                          |
   |                       |                       |                                         |
   |                       |                       | Maximum number of CN nodes.             |
   |                       |                       |                                         |
   |                       |                       | **Range**                               |
   |                       |                       |                                         |
   |                       |                       | N/A                                     |
   +-----------------------+-----------------------+-----------------------------------------+
   | dn_num                | Integer               | **Definition**                          |
   |                       |                       |                                         |
   |                       |                       | Number of DN nodes.                     |
   |                       |                       |                                         |
   |                       |                       | **Range**                               |
   |                       |                       |                                         |
   |                       |                       | N/A                                     |
   +-----------------------+-----------------------+-----------------------------------------+

Example Requests
----------------

.. code-block:: text

   GET https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/databases/gaussdb/schemas

Example Responses
-----------------

**Status code: 200**

Shema space information queried.

.. code-block::

   {
     "schemas" : [ {
       "schema_name" : "gs_logical_cluster",
       "database_name" : "gaussdb",
       "total_value" : 0,
       "perm_space" : -1,
       "skew_percent" : 0.0,
       "min_value" : 0,
       "max_value" : 0,
       "min_dn" : "dn_6001_6002",
       "max_dn" : "",
       "dn_num" : 3
     } ],
     "count" : 2
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Shema space information queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
