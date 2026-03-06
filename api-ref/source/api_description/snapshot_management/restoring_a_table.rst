:original_name: RestoreTable.html

.. _RestoreTable:

Restoring a Table
=================

Function
--------

This API is used to restore tables.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v1/{project_id}/snapshots/{snapshot_id}/table-restore

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
   | restore_table_list | Yes             | Array of :ref:`TableDetail <en-us_topic_0000002500014082__en-us_topic_0000002374935313_request_tabledetail>` objects | **Definition**                      |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | Source table information.           |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | **Range**                           |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | N/A                                 |
   +--------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+-------------------------------------+
   | target_table_list  | Yes             | Array of :ref:`TableDetail <en-us_topic_0000002500014082__en-us_topic_0000002374935313_request_tabledetail>` objects | **Definition**                      |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | Destination table information.      |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | **Range**                           |
   |                    |                 |                                                                                                                      |                                     |
   |                    |                 |                                                                                                                      | N/A                                 |
   +--------------------+-----------------+----------------------------------------------------------------------------------------------------------------------+-------------------------------------+

.. _en-us_topic_0000002500014082__en-us_topic_0000002374935313_request_tabledetail:

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

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | job_id                | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Task ID.              |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

.. code-block:: text

   POST https://{Endpoint}/v2/0536cdee2200d5912f7cc00b877980f1/snapshots/c719b1a7-c85c-4cb5-a721-7694908c2c11/table-restore

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

The table is successfully restored.

.. code-block::

   {
     "job_id" : "2c9081c0894918c301894e503ef21b68"
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         The table is successfully restored.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
500         Internal server error.
503         Service unavailable.
=========== =====================================
