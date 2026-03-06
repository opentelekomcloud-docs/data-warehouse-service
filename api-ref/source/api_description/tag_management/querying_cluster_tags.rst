:original_name: ListClusterTags.html

.. _ListClusterTags:

Querying Cluster Tags
=====================

Function
--------

This API is used to query tag information of a cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1.0/{project_id}/clusters/{cluster_id}/tags

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

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 2** Response body parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Parameter             | Type                                                                                                                  | Description           |
   +=======================+=======================================================================================================================+=======================+
   | tags                  | Array of :ref:`ResourceTag <en-us_topic_0000002500014110__en-us_topic_0000002375015693_response_resourcetag>` objects | **Definition**        |
   |                       |                                                                                                                       |                       |
   |                       |                                                                                                                       | Tag list.             |
   |                       |                                                                                                                       |                       |
   |                       |                                                                                                                       | **Range**             |
   |                       |                                                                                                                       |                       |
   |                       |                                                                                                                       | N/A                   |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------+-----------------------+

.. _en-us_topic_0000002500014110__en-us_topic_0000002375015693_response_resourcetag:

.. table:: **Table 3** ResourceTag

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | key                   | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Tag key.              |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | value                 | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Tag value.            |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

.. code-block:: text

   GET https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/tags

Example Responses
-----------------

**Status code: 200**

Tag information of the cluster queried.

.. code-block::

   {
     "tags" : [ {
       "key" : "key",
       "value" : "value"
     } ]
   }

Status Codes
------------

=========== =======================================
Status Code Description
=========== =======================================
200         Tag information of the cluster queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =======================================
