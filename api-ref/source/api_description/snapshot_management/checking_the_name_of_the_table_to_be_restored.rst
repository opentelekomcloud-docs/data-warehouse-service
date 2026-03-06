:original_name: CheckTableRestore.html

.. _CheckTableRestore:

Checking the Name of the Table to Be Restored
=============================================

Function
--------

This API is used to check the name of the table to be restored.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v1/{project_id}/snapshots/{snapshot_id}/table-restore-check

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
   | snapshot_id     | Yes             | String          | **Definition**                                                                    |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | Snapshot ID.                                                                      |
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

   +--------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+-------------------------------------+
   | Parameter          | Mandatory       | Type                                                                                                                 | Description                         |
   +====================+=================+======================================================================================================================+=====================================+
   | case_sensitive     | Yes             | Boolean                                                                                                              | **Definition**                      |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | Whether the name is case sensitive. |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | **Range**                           |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | N/A                                 |
   +--------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+-------------------------------------+
   | database           | Yes             | String                                                                                                               | **Definition**                      |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | Database name.                      |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | **Range**                           |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | N/A                                 |
   +--------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+-------------------------------------+
   | restore_table_list | Yes             | Array of :ref:`TableDetail <en-us_topic_0000002531893931__en-us_topic_0000002375015185_request_tabledetail>` objects | **Definition**                      |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | Source table information.           |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | **Range**                           |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | N/A                                 |
   +--------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+-------------------------------------+
   | target_table_list  | Yes             | Array of :ref:`TableDetail <en-us_topic_0000002531893931__en-us_topic_0000002375015185_request_tabledetail>` objects | **Definition**                      |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | Destination table information.      |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | **Range**                           |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | N/A                                 |
   +--------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+-------------------------------------+

.. _en-us_topic_0000002531893931__en-us_topic_0000002375015185_request_tabledetail:

.. table:: **Table 3** TableDetail

   +-----------------+-----------------+-----------------+-----------------+
   | Parameter       | Mandatory       | Type            | Description     |
   +=================+=================+=================+=================+
   | schema_name     | Yes             | String          | **Definition**  |
   |                 |                 |                 |                 |
   |                 |                 |                 | Schema name.    |
   |                 |                 |                 |                 |
   |                 |                 |                 | **Range**       |
   |                 |                 |                 |                 |
   |                 |                 |                 | N/A             |
   +-----------------+-----------------+-----------------+-----------------+
   | table_name      | Yes             | String          | **Definition**  |
   |                 |                 |                 |                 |
   |                 |                 |                 | Table name.     |
   |                 |                 |                 |                 |
   |                 |                 |                 | **Range**       |
   |                 |                 |                 |                 |
   |                 |                 |                 | N/A             |
   +-----------------+-----------------+-----------------+-----------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 4** Response body parameters

   +-------------------------+-------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Parameter               | Type                                                                                                                          | Description           |
   +=========================+===============================================================================================================================+=======================+
   | check_table_name_result | :ref:`CheckTableNameResult <en-us_topic_0000002531893931__en-us_topic_0000002375015185_response_checktablenameresult>` object | **Definition**        |
   |                         |                                                                                                                               |                       |
   |                         |                                                                                                                               | Check result.         |
   |                         |                                                                                                                               |                       |
   |                         |                                                                                                                               | **Range**             |
   |                         |                                                                                                                               |                       |
   |                         |                                                                                                                               | N/A                   |
   +-------------------------+-------------------------------------------------------------------------------------------------------------------------------+-----------------------+

.. _en-us_topic_0000002531893931__en-us_topic_0000002375015185_response_checktablenameresult:

.. table:: **Table 5** CheckTableNameResult

   +-----------------------+-----------------------+------------------------------------------------------------+
   | Parameter             | Type                  | Description                                                |
   +=======================+=======================+============================================================+
   | database              | String                | **Definition**                                             |
   |                       |                       |                                                            |
   |                       |                       | Database name.                                             |
   |                       |                       |                                                            |
   |                       |                       | **Range**                                                  |
   |                       |                       |                                                            |
   |                       |                       | N/A                                                        |
   +-----------------------+-----------------------+------------------------------------------------------------+
   | restore_table_list    | Array of strings      | **Definition**                                             |
   |                       |                       |                                                            |
   |                       |                       | Information about the source tables in a restoration.      |
   |                       |                       |                                                            |
   |                       |                       | **Range**                                                  |
   |                       |                       |                                                            |
   |                       |                       | N/A                                                        |
   +-----------------------+-----------------------+------------------------------------------------------------+
   | target_table_list     | Array of strings      | **Definition**                                             |
   |                       |                       |                                                            |
   |                       |                       | Information about the destination tables in a restoration. |
   |                       |                       |                                                            |
   |                       |                       | **Range**                                                  |
   |                       |                       |                                                            |
   |                       |                       | N/A                                                        |
   +-----------------------+-----------------------+------------------------------------------------------------+

Example Requests
----------------

.. code-block:: text

   POST https://{Endpoint}/v2/0536cdee2200d5912f7cc00b877980f1/snapshots/c719b1a7-c85c-4cb5-a721-7694908c2c11/table-restore-check

   {
     "case_sensitive" : true,
     "database" : "postgres",
     "restore_table_list" : [ {
       "schema_name" : "postgres",
       "table_name" : "public"
     } ],
     "target_table_list" : [ {
       "schema_name" : "postgres",
       "table_name" : "public"
     } ]
   }

Example Responses
-----------------

**Status code: 200**

The name of the table to be restored is checked.

.. code-block::

   {
     "check_table_name_result" : {
       "database" : "postgres",
       "restore_table_list" : null,
       "target_table_list" : null
     }
   }

Status Codes
------------

=========== ================================================
Status Code Description
=========== ================================================
200         The name of the table to be restored is checked.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
500         Internal server error.
503         Service unavailable.
=========== ================================================
