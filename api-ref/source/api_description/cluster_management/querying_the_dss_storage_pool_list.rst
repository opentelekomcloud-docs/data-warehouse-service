:original_name: ListDssPools.html

.. _ListDssPools:

Querying the DSS Storage Pool List
==================================

Function
--------

This API is used to query the list of DSS storage pools. Only SSD dedicated resource pools that you enabled can be queried.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1.0/{project_id}/dss-pools

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

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 2** Response body parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------+-----------------------------------+
   | Parameter             | Type                                                                                                          | Description                       |
   +=======================+===============================================================================================================+===================================+
   | pools                 | Array of :ref:`DssPool <en-us_topic_0000002500014050__en-us_topic_0000002374935097_response_dsspool>` objects | **Definition**                    |
   |                       |                                                                                                               |                                   |
   |                       |                                                                                                               | Details of the DSS storage pools. |
   |                       |                                                                                                               |                                   |
   |                       |                                                                                                               | **Range**                         |
   |                       |                                                                                                               |                                   |
   |                       |                                                                                                               | N/A                               |
   +-----------------------+---------------------------------------------------------------------------------------------------------------+-----------------------------------+
   | count                 | Integer                                                                                                       | **Definition**                    |
   |                       |                                                                                                               |                                   |
   |                       |                                                                                                               | Number of the DSS storage pools.  |
   |                       |                                                                                                               |                                   |
   |                       |                                                                                                               | **Range**                         |
   |                       |                                                                                                               |                                   |
   |                       |                                                                                                               | N/A                               |
   +-----------------------+---------------------------------------------------------------------------------------------------------------+-----------------------------------+

.. _en-us_topic_0000002500014050__en-us_topic_0000002374935097_response_dsspool:

.. table:: **Table 3** DssPool

   +-----------------------+-----------------------+-----------------------------------------------------------------------------------+
   | Parameter             | Type                  | Description                                                                       |
   +=======================+=======================+===================================================================================+
   | id                    | String                | **Definition**                                                                    |
   |                       |                       |                                                                                   |
   |                       |                       | DSS storage pool name.                                                            |
   |                       |                       |                                                                                   |
   |                       |                       | **Range**                                                                         |
   |                       |                       |                                                                                   |
   |                       |                       | N/A                                                                               |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------+
   | name                  | String                | **Definition**                                                                    |
   |                       |                       |                                                                                   |
   |                       |                       | DSS storage pool ID.                                                              |
   |                       |                       |                                                                                   |
   |                       |                       | **Range**                                                                         |
   |                       |                       |                                                                                   |
   |                       |                       | N/A                                                                               |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------+
   | type                  | String                | **Definition**                                                                    |
   |                       |                       |                                                                                   |
   |                       |                       | DSS storage pool type.                                                            |
   |                       |                       |                                                                                   |
   |                       |                       | -  **SSD**: ultra-high I/O storage pool.                                          |
   |                       |                       |                                                                                   |
   |                       |                       | **Range**                                                                         |
   |                       |                       |                                                                                   |
   |                       |                       | N/A                                                                               |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------+
   | project_id            | String                | **Definition**                                                                    |
   |                       |                       |                                                                                   |
   |                       |                       | Project ID. To obtain the value, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   |                       |                       |                                                                                   |
   |                       |                       | **Constraints**                                                                   |
   |                       |                       |                                                                                   |
   |                       |                       | N/A                                                                               |
   |                       |                       |                                                                                   |
   |                       |                       | **Range**                                                                         |
   |                       |                       |                                                                                   |
   |                       |                       | N/A                                                                               |
   |                       |                       |                                                                                   |
   |                       |                       | **Default Value**                                                                 |
   |                       |                       |                                                                                   |
   |                       |                       | N/A                                                                               |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------+
   | availability_zone     | String                | **Definition**                                                                    |
   |                       |                       |                                                                                   |
   |                       |                       | AZ where the DSS storage pool is located.                                         |
   |                       |                       |                                                                                   |
   |                       |                       | **Range**                                                                         |
   |                       |                       |                                                                                   |
   |                       |                       | N/A                                                                               |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------+
   | capacity              | Integer               | **Definition**                                                                    |
   |                       |                       |                                                                                   |
   |                       |                       | DSS storage pool capacity, in TB.                                                 |
   |                       |                       |                                                                                   |
   |                       |                       | **Range**                                                                         |
   |                       |                       |                                                                                   |
   |                       |                       | N/A                                                                               |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------+
   | status                | String                | **Definition**                                                                    |
   |                       |                       |                                                                                   |
   |                       |                       | DSS storage pool status.                                                          |
   |                       |                       |                                                                                   |
   |                       |                       | **Range**                                                                         |
   |                       |                       |                                                                                   |
   |                       |                       | **available**: The DSS storage pool is available.                                 |
   |                       |                       |                                                                                   |
   |                       |                       | **deploying**: The DSS storage pool is being deployed and cannot be used.         |
   |                       |                       |                                                                                   |
   |                       |                       | **extending**: The DSS storage pool is being expanded and can be used.            |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------+
   | created_at            | String                | **Definition**                                                                    |
   |                       |                       |                                                                                   |
   |                       |                       | Time when the DSS storage pool was created.                                       |
   |                       |                       |                                                                                   |
   |                       |                       | **Range**                                                                         |
   |                       |                       |                                                                                   |
   |                       |                       | Time format: UTC YYYY-MM-DDTHH:MM:SS                                              |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------+

Example Requests
----------------

Query the list of dedicated distributed storage pools.

.. code-block:: text

   GET https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/dss-pools

Example Responses
-----------------

**Status code: 200**

DSS storage pool list queried.

.. code-block::

   {
     "pools" : [ {
       "id" : "c950ee97-587c-4f24-8a74-3367e3da570f",
       "name" : "pool-1",
       "type" : "SSD",
       "project_id" : "63d910f2705a487ebe4e1c274748d9e1",
       "capacity" : "1000",
       "availability_zone" : "AZ1",
       "status" : "available",
       "created_at" : "2014-12-18T15:57:56.299000"
     } ],
     "count" : 1
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         DSS storage pool list queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
