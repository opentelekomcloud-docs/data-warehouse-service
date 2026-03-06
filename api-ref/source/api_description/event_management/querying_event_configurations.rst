:original_name: ListEventSpecs.html

.. _ListEventSpecs:

Querying Event Configurations
=============================

Function
--------

This API is used to query event configurations.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/event-specs

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

.. table:: **Table 2** Query Parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                             |
   +=================+=================+=================+=========================================================+
   | spec_name       | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Event configuration name.                               |
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
   | category        | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Event category.                                         |
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
   | severity        | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Event severity.                                         |
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
   | source_type     | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Event source type.                                      |
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
   | tag             | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Event tag.                                              |
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
   | offset          | No              | String          | **Definition**                                          |
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
   | limit           | No              | String          | **Definition**                                          |
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
   |                 |                 |                 | 1000                                                    |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | Parameter             | Type                                                                                                                              | Description                           |
   +=======================+===================================================================================================================================+=======================================+
   | count                 | Integer                                                                                                                           | **Definition**                        |
   |                       |                                                                                                                                   |                                       |
   |                       |                                                                                                                                   | Total number of event configurations. |
   |                       |                                                                                                                                   |                                       |
   |                       |                                                                                                                                   | **Range**                             |
   |                       |                                                                                                                                   |                                       |
   |                       |                                                                                                                                   | Greater than or equal to **0**        |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | event_specs           | Array of :ref:`EventSpecResponse <en-us_topic_0000002500014000__en-us_topic_0000002375015621_response_eventspecresponse>` objects | **Definition**                        |
   |                       |                                                                                                                                   |                                       |
   |                       |                                                                                                                                   | Event configuration list.             |
   |                       |                                                                                                                                   |                                       |
   |                       |                                                                                                                                   | **Range**                             |
   |                       |                                                                                                                                   |                                       |
   |                       |                                                                                                                                   | N/A                                   |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+

.. _en-us_topic_0000002500014000__en-us_topic_0000002375015621_response_eventspecresponse:

.. table:: **Table 4** EventSpecResponse

   +-----------------------+-----------------------+--------------------------------------+
   | Parameter             | Type                  | Description                          |
   +=======================+=======================+======================================+
   | id                    | String                | **Definition**                       |
   |                       |                       |                                      |
   |                       |                       | Event configuration ID.              |
   |                       |                       |                                      |
   |                       |                       | **Range**                            |
   |                       |                       |                                      |
   |                       |                       | N/A                                  |
   +-----------------------+-----------------------+--------------------------------------+
   | name                  | String                | **Definition**                       |
   |                       |                       |                                      |
   |                       |                       | Event configuration definition name. |
   |                       |                       |                                      |
   |                       |                       | **Range**                            |
   |                       |                       |                                      |
   |                       |                       | N/A                                  |
   +-----------------------+-----------------------+--------------------------------------+
   | display_name          | String                | **Definition**                       |
   |                       |                       |                                      |
   |                       |                       | Event configuration display name.    |
   |                       |                       |                                      |
   |                       |                       | **Range**                            |
   |                       |                       |                                      |
   |                       |                       | N/A                                  |
   +-----------------------+-----------------------+--------------------------------------+
   | description           | String                | **Definition**                       |
   |                       |                       |                                      |
   |                       |                       | Event configuration description.     |
   |                       |                       |                                      |
   |                       |                       | **Range**                            |
   |                       |                       |                                      |
   |                       |                       | N/A                                  |
   +-----------------------+-----------------------+--------------------------------------+
   | subject               | String                | **Definition**                       |
   |                       |                       |                                      |
   |                       |                       | Event subject.                       |
   |                       |                       |                                      |
   |                       |                       | **Range**                            |
   |                       |                       |                                      |
   |                       |                       | N/A                                  |
   +-----------------------+-----------------------+--------------------------------------+
   | category              | String                | **Definition**                       |
   |                       |                       |                                      |
   |                       |                       | Event category.                      |
   |                       |                       |                                      |
   |                       |                       | **Range**                            |
   |                       |                       |                                      |
   |                       |                       | N/A                                  |
   +-----------------------+-----------------------+--------------------------------------+
   | severity              | String                | **Definition**                       |
   |                       |                       |                                      |
   |                       |                       | Event severity.                      |
   |                       |                       |                                      |
   |                       |                       | **Range**                            |
   |                       |                       |                                      |
   |                       |                       | N/A                                  |
   +-----------------------+-----------------------+--------------------------------------+
   | source_type           | String                | **Definition**                       |
   |                       |                       |                                      |
   |                       |                       | Event source type.                   |
   |                       |                       |                                      |
   |                       |                       | **Range**                            |
   |                       |                       |                                      |
   |                       |                       | N/A                                  |
   +-----------------------+-----------------------+--------------------------------------+
   | name_space            | String                | **Definition**                       |
   |                       |                       |                                      |
   |                       |                       | Service to which the event belongs.  |
   |                       |                       |                                      |
   |                       |                       | **Range**                            |
   |                       |                       |                                      |
   |                       |                       | N/A                                  |
   +-----------------------+-----------------------+--------------------------------------+

Example Requests
----------------

.. code-block::

   https://{Endpoint}/v2/{project_id}/event-specs

Example Responses
-----------------

**Status code: 200**

Event configurations of a cluster queried.

.. code-block::

   {
     "event_specs" : [ {
       "id" : "fa6e1502-9d08-48c7-900c-26d3b5bd6078",
       "name" : "configureMRSExtDataSourcesSuccess",
       "description" : "Succeeded in configuring the MRS data source for the cluster %s",
       "subject" : "DWS event notification",
       "category" : "management",
       "severity" : "normal",
       "display_name" : "Succeeded in configuring the MRS data source for the cluster",
       "source_type" : "cluster",
       "name_space" : "dws"
     } ],
     "count" : 1
   }

Status Codes
------------

=========== ==========================================
Status Code Description
=========== ==========================================
200         Event configurations of a cluster queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ==========================================
