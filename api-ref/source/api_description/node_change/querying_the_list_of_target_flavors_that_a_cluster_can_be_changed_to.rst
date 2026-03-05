:original_name: ListTargetFlavors.html

.. _ListTargetFlavors:

Querying the List of Target Flavors That a Cluster Can Be Changed To
====================================================================

Function
--------

This API is used to query the list of flavors that a cluster can be changed to. A maximum of 20 flavors can be returned.

**Constraints**

If **cluster_id** is not specified, all flavors that a cluster can be changed to are returned. However, some flavors may be sold out and cannot be used due to quota reasons.

If **cluster_id** is specified, the flavors with sufficient quotas in the AZ where the cluster is located are automatically returned.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1/{project_id}/flavors/{flavor_id}/target-flavors

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
   | flavor_id       | Yes             | String          | **Definition**                                                                    |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | Current flavor ID of the cluster.                                                 |
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

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                   |
   +=================+=================+=================+===============================================================================================================================================================================+
   | cluster_id      | No              | String          | **Definition**                                                                                                                                                                |
   |                 |                 |                 |                                                                                                                                                                               |
   |                 |                 |                 | Cluster ID. For details about how to obtain the value, see :ref:`Obtaining the Cluster ID <dws_02_00068>`.                                                                    |
   |                 |                 |                 |                                                                                                                                                                               |
   |                 |                 |                 | **Constraints**                                                                                                                                                               |
   |                 |                 |                 |                                                                                                                                                                               |
   |                 |                 |                 | If this parameter is not specified, all flavors that a cluster can be changed to are returned. However, some flavors may be sold out and cannot be used due to quota reasons. |
   |                 |                 |                 |                                                                                                                                                                               |
   |                 |                 |                 | If **cluster_id** is specified, the flavors with sufficient quotas in the AZ where the cluster is located are automatically returned.                                         |
   |                 |                 |                 |                                                                                                                                                                               |
   |                 |                 |                 | **Range**                                                                                                                                                                     |
   |                 |                 |                 |                                                                                                                                                                               |
   |                 |                 |                 | N/A                                                                                                                                                                           |
   |                 |                 |                 |                                                                                                                                                                               |
   |                 |                 |                 | **Default Value**                                                                                                                                                             |
   |                 |                 |                 |                                                                                                                                                                               |
   |                 |                 |                 | null                                                                                                                                                                          |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------+
   | Parameter             | Type                                                                                                                                | Description                                                      |
   +=======================+=====================================================================================================================================+==================================================================+
   | count                 | Integer                                                                                                                             | **Definition**                                                   |
   |                       |                                                                                                                                     |                                                                  |
   |                       |                                                                                                                                     | Number of flavors.                                               |
   |                       |                                                                                                                                     |                                                                  |
   |                       |                                                                                                                                     | **Range**                                                        |
   |                       |                                                                                                                                     |                                                                  |
   |                       |                                                                                                                                     | N/A                                                              |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------+
   | flavors               | Array of :ref:`FlavorInfoResponse <en-us_topic_0000002531893899__en-us_topic_0000002341096912_response_flavorinforesponse>` objects | **Definition**                                                   |
   |                       |                                                                                                                                     |                                                                  |
   |                       |                                                                                                                                     | List of flavor details. A maximum of 20 flavors can be returned. |
   |                       |                                                                                                                                     |                                                                  |
   |                       |                                                                                                                                     | **Range**                                                        |
   |                       |                                                                                                                                     |                                                                  |
   |                       |                                                                                                                                     | N/A                                                              |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------+

.. _en-us_topic_0000002531893899__en-us_topic_0000002341096912_response_flavorinforesponse:

.. table:: **Table 4** FlavorInfoResponse

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | id                    | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Flavor ID.            |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | name                  | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Flavor code.          |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | vcpus                 | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | CPUs.                 |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | ram                   | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Memory size.          |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | is_current_flavor     | Boolean               | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Current flavor.       |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

Query the list of flavors whose ID is **b5c45780-1006-49e3-b2d5-b3229975bbc7**.

.. code-block::

   get https://{Endpoint}/v1/89cd04f168b84af6be287f71730fdb4b/flavors/b5c45780-1006-49e3-b2d5-b3229975bbc7/target-flavors

Example Responses
-----------------

**Status code: 200**

Query succeeded.

.. code-block::

   {
     "flavors" : [ {
       "id" : "4e26f8d5-d64f-458e-80e7-26680b46cd58",
       "name" : "dwsx2.xlarge",
       "vcpus" : "4",
       "ram" : "32",
       "is_current_flavor" : false
     } ],
     "count" : 1
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Query succeeded.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
