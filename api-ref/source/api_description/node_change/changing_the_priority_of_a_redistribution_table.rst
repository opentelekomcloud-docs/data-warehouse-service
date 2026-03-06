:original_name: SetRedistributionPriority.html

.. _SetRedistributionPriority:

Changing the Priority of a Redistribution Table
===============================================

Function
--------

This API is used to change the priority of a redistribution table.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

PUT /v2/{project_id}/clusters/{cluster_id}/redistribution/priority

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

   +-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+
   | Parameter       | Mandatory       | Type                                                                                                                               | Description                                |
   +=================+=================+====================================================================================================================================+============================================+
   | db_name         | No              | String                                                                                                                             | **Definition**                             |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | Database name.                             |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | **Constraints**                            |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | N/A                                        |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | **Range**                                  |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | **1** to **1024**                          |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | **Default Value**                          |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | N/A                                        |
   +-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+
   | priority        | No              | Array of :ref:`RedisPriorityTable <en-us_topic_0000002500174088__en-us_topic_0000002375014997_request_redisprioritytable>` objects | **Definition**                             |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | Priority of the table to be redistributed. |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | **Constraints**                            |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | N/A                                        |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | **Range**                                  |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | N/A                                        |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | **Default Value**                          |
   |                 |                 |                                                                                                                                    |                                            |
   |                 |                 |                                                                                                                                    | N/A                                        |
   +-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+

.. _en-us_topic_0000002500174088__en-us_topic_0000002375014997_request_redisprioritytable:

.. table:: **Table 3** RedisPriorityTable

   +-----------------+-----------------+-----------------+-------------------+
   | Parameter       | Mandatory       | Type            | Description       |
   +=================+=================+=================+===================+
   | schema_name     | No              | String          | **Definition**    |
   |                 |                 |                 |                   |
   |                 |                 |                 | Schema name.      |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Constraints**   |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Range**         |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Default Value** |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   +-----------------+-----------------+-----------------+-------------------+
   | id              | No              | Long            | **Definition**    |
   |                 |                 |                 |                   |
   |                 |                 |                 | Table ID.         |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Constraints**   |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Range**         |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Default Value** |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   +-----------------+-----------------+-----------------+-------------------+
   | table_name      | No              | String          | **Definition**    |
   |                 |                 |                 |                   |
   |                 |                 |                 | Table name.       |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Constraints**   |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Range**         |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Default Value** |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   +-----------------+-----------------+-----------------+-------------------+
   | redis_order     | No              | Integer         | **Definition**    |
   |                 |                 |                 |                   |
   |                 |                 |                 | Priority.         |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Constraints**   |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Range**         |
   |                 |                 |                 |                   |
   |                 |                 |                 | **1** to **1024** |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Default Value** |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   +-----------------+-----------------+-----------------+-------------------+

Response Parameters
-------------------

**Status code: 200**

Operation succeeded.

None

Example Requests
----------------

Set the redistribution priority of the **test_1** table in the **public** schema in the GaussDB database to 55.

.. code-block:: text

   PUT https://{Endpoint}/v2/05f2cff45100d5112f4bc00b794ea08e/clusters/496c3032-c08f-4cd1-8c66-9b56a89a274d/redistribution/priority

   {
     "db_name" : "gaussdb",
     "priority" : [ {
       "id" : 17449,
       "schema_name" : "public",
       "table_name" : "test_1",
       "redis_order" : 55
     } ]
   }

Example Responses
-----------------

**Status code: 200**

Operation succeeded.

.. code-block::

   { }

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
