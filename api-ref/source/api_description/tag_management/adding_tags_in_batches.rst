:original_name: BatchCreateResourceTag.html

.. _BatchCreateResourceTag:

Adding Tags in Batches
======================

Function
--------

This API is used to batch add tags to a specified cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v1.0/{project_id}/clusters/{cluster_id}/tags/batch-create

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

   +-----------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------+-------------+
   | Parameter | Mandatory | Type                                                                                                                                       | Description |
   +===========+===========+============================================================================================================================================+=============+
   | tags      | Yes       | Array of :ref:`BatchCreateResourceTag <en-us_topic_0000002532013979__en-us_topic_0000002374935821_request_batchcreateresourcetag>` objects | Tag list.   |
   +-----------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------+-------------+

.. _en-us_topic_0000002532013979__en-us_topic_0000002374935821_request_batchcreateresourcetag:

.. table:: **Table 3** BatchCreateResourceTag

   +-----------------+-----------------+-----------------+-----------------+
   | Parameter       | Mandatory       | Type            | Description     |
   +=================+=================+=================+=================+
   | key             | Yes             | String          | **Definition**  |
   |                 |                 |                 |                 |
   |                 |                 |                 | Tag key.        |
   |                 |                 |                 |                 |
   |                 |                 |                 | **Range**       |
   |                 |                 |                 |                 |
   |                 |                 |                 | N/A             |
   +-----------------+-----------------+-----------------+-----------------+
   | value           | Yes             | String          | **Definition**  |
   |                 |                 |                 |                 |
   |                 |                 |                 | Tag value.      |
   |                 |                 |                 |                 |
   |                 |                 |                 | **Range**       |
   |                 |                 |                 |                 |
   |                 |                 |                 | N/A             |
   +-----------------+-----------------+-----------------+-----------------+

Response Parameters
-------------------

**Status code: 200**

Tags are added in batches.

None

Example Requests
----------------

Add tags whose key is **key** and value is **value** in batches.

.. code-block:: text

   POST https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/b5c45780-1006-49e3-b2d5-b3229975bbc7/tags/batch-create

   {
     "tags" : [ {
       "key" : "key",
       "value" : "value"
     } ]
   }

Example Responses
-----------------

None

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Tags are added in batches.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
