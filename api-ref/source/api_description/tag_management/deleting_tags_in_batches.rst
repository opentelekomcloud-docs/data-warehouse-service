:original_name: dws_02_0048.html

.. _dws_02_0048:

Deleting Tags in Batches
========================

Function
--------

This API is used to delete tags from a cluster in batches.

URI
---

.. code-block:: text

   POST /v1.0/{project_id}/clusters/{cluster_id}/tags/batch-delete

.. table:: **Table 1** URI parameters

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                          |
   +=================+=================+=================+======================================================================================+
   | project_id      | Yes             | String          | **Definition**                                                                       |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | Project ID. To obtain the value, see :ref:`Obtaining a Project ID <dws_02_0011>`.    |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | **Constraints**                                                                      |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | N/A                                                                                  |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | **Range**                                                                            |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | N/A                                                                                  |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | **Default Value**                                                                    |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | N/A                                                                                  |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------+
   | cluster_id      | Yes             | String          | **Definition**                                                                       |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | Cluster ID. To obtain the value, see :ref:`Obtaining the Cluster ID <dws_02_00068>`. |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | **Constraints**                                                                      |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | N/A                                                                                  |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | **Range**                                                                            |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | N/A                                                                                  |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | **Default Value**                                                                    |
   |                 |                 |                 |                                                                                      |
   |                 |                 |                 | N/A                                                                                  |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
   | Parameter       | Mandatory       | Type                                                                                                                                                                    | Description       |
   +=================+=================+=========================================================================================================================================================================+===================+
   | tags            | Yes             | Array of :ref:`BatchDeleteResourceTag <en-us_topic_0000002395330901__en-us_topic_0000002341147714_en-us_topic_0000002374844677_request_batchdeleteresourcetag>` objects | **Definition**    |
   |                 |                 |                                                                                                                                                                         |                   |
   |                 |                 |                                                                                                                                                                         | Tag list.         |
   |                 |                 |                                                                                                                                                                         |                   |
   |                 |                 |                                                                                                                                                                         | **Constraints**   |
   |                 |                 |                                                                                                                                                                         |                   |
   |                 |                 |                                                                                                                                                                         | N/A               |
   |                 |                 |                                                                                                                                                                         |                   |
   |                 |                 |                                                                                                                                                                         | **Range**         |
   |                 |                 |                                                                                                                                                                         |                   |
   |                 |                 |                                                                                                                                                                         | N/A               |
   |                 |                 |                                                                                                                                                                         |                   |
   |                 |                 |                                                                                                                                                                         | **Default Value** |
   |                 |                 |                                                                                                                                                                         |                   |
   |                 |                 |                                                                                                                                                                         | N/A               |
   +-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+

.. _en-us_topic_0000002395330901__en-us_topic_0000002341147714_en-us_topic_0000002374844677_request_batchdeleteresourcetag:

.. table:: **Table 3** BatchDeleteResourceTag

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

Tags are deleted in batches.

None

Example Request
---------------

Delete tags whose key is **key** and value is **value** in batches.

.. code-block:: text

   POST https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/b5c45780-1006-49e3-b2d5-b3229975bbc7/tags/batch-delete

   {
     "tags" : [ {
       "key" : "key",
       "value" : "value"
     } ]
   }

Example Responses
-----------------

None

Status Code
-----------

=========== ============================
Status Code Description
=========== ============================
200         Tags are deleted in batches.
400         Request error.
401         Authorization failed.
403         No operation permission.
404         No resources found.
500         Internal service error.
503         Service unavailable.
=========== ============================
