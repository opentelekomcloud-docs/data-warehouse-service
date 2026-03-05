:original_name: ListRedistributionSchema.html

.. _ListRedistributionSchema:

Querying the Schema Information of the Table to Be Redistributed
================================================================

Function
--------

This API is used to query the schema information of the table to be redistributed.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/clusters/{cluster_id}/redistribution/schemas

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

.. table:: **Table 2** Query Parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                             |
   +=================+=================+=================+=========================================================+
   | db_name         | Yes             | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Pagination offset.                                      |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | ^[a-zA-Z0-9\\u4e00-\\u9fa5_.+= :@!#-]{0,255}$           |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | null                                                    |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | limit           | No              | Integer         | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Number of records on a page.                            |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Greater than or equal to **1**                          |
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
   | schema_name     | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Schema name.                                            |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | ^[a-zA-Z0-9\\u4e00-\\u9fa5_.+= ``,:@!#-]{``\ 0,2048}$   |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | null                                                    |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------------------------+------------------------------------------------------+
   | Parameter             | Type                                                                                                                  | Description                                          |
   +=======================+=======================================================================================================================+======================================================+
   | error_code            | String                                                                                                                | **Definition**                                       |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | Error code.                                          |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | **Range**                                            |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | If the request is normal, the value is **DWS.0000**. |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------+------------------------------------------------------+
   | error_msg             | String                                                                                                                | **Definition**                                       |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | Error message.                                       |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | **Range**                                            |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | N/A                                                  |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------+------------------------------------------------------+
   | schemas               | Array of :ref:`RedisSchema <en-us_topic_0000002531893937__en-us_topic_0000002374935121_response_redisschema>` objects | **Definition**                                       |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | List of schemas to be redistributed.                 |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | **Range**                                            |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | N/A                                                  |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------+------------------------------------------------------+
   | count                 | Integer                                                                                                               | **Definition**                                       |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | Total number of records.                             |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | **Range**                                            |
   |                       |                                                                                                                       |                                                      |
   |                       |                                                                                                                       | N/A                                                  |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------+------------------------------------------------------+

.. _en-us_topic_0000002531893937__en-us_topic_0000002374935121_response_redisschema:

.. table:: **Table 4** RedisSchema

   +-----------------------+-----------------------+-------------------------------+
   | Parameter             | Type                  | Description                   |
   +=======================+=======================+===============================+
   | schema_name           | String                | **Definition**                |
   |                       |                       |                               |
   |                       |                       | Schema name.                  |
   |                       |                       |                               |
   |                       |                       | **Range**                     |
   |                       |                       |                               |
   |                       |                       | N/A                           |
   +-----------------------+-----------------------+-------------------------------+
   | redis_order           | Integer               | **Definition**                |
   |                       |                       |                               |
   |                       |                       | Priority No.                  |
   |                       |                       |                               |
   |                       |                       | **Range**                     |
   |                       |                       |                               |
   |                       |                       | An integer greater than **0** |
   +-----------------------+-----------------------+-------------------------------+

Example Requests
----------------

Query the schema of the table to be redistributed in the GaussDB database.

.. code-block:: text

   GET https://{Endpoint}/v2/0536cdee2200d5912f7cc00b877980f1/clusters/bcdfb00c-a5e3-4896-83c7-3c397ed99f28/redistribution/schemas?limit=10&offset=0&db_name=gaussdb

Example Responses
-----------------

**Status code: 200**

Operation succeeded.

.. code-block::

   {
     "error_code" : "DWS.0000",
     "error_msg" : "Request processed.",
     "schemas" : [ {
       "schema_name" : "scheduler",
       "redis_order" : 1024
     } ],
     "count" : 1
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Operation succeeded.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
