:original_name: ListTagsForResource.html

.. _ListTagsForResource:

Querying the Enterprise Project Information of a Cluster
========================================================

Function
--------

This API is used to query the enterprise project information of a specified cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1/{project_id}/clusters/{cluster_id}/enterprise-projects

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
   | limit           | No              | Integer         | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Page size. The default value is **10**.                 |
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

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-------------------------------------------------------------------------------------------------------+-----------------------+
   | Parameter             | Type                                                                                                  | Description           |
   +=======================+=======================================================================================================+=======================+
   | sys_tags              | Array of :ref:`Tag <en-us_topic_0000002531893971__en-us_topic_0000002341096864_response_tag>` objects | **Definition**        |
   |                       |                                                                                                       |                       |
   |                       |                                                                                                       | Tag list.             |
   |                       |                                                                                                       |                       |
   |                       |                                                                                                       | **Range**             |
   |                       |                                                                                                       |                       |
   |                       |                                                                                                       | N/A                   |
   +-----------------------+-------------------------------------------------------------------------------------------------------+-----------------------+
   | count                 | Integer                                                                                               | **Definition**        |
   |                       |                                                                                                       |                       |
   |                       |                                                                                                       | Number of tags.       |
   |                       |                                                                                                       |                       |
   |                       |                                                                                                       | **Range**             |
   |                       |                                                                                                       |                       |
   |                       |                                                                                                       | N/A                   |
   +-----------------------+-------------------------------------------------------------------------------------------------------+-----------------------+

.. _en-us_topic_0000002531893971__en-us_topic_0000002341096864_response_tag:

.. table:: **Table 4** Tag

   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Type                  | Description                                                                                                                |
   +=======================+=======================+============================================================================================================================+
   | key                   | String                | **Definition**                                                                                                             |
   |                       |                       |                                                                                                                            |
   |                       |                       | Tag key.                                                                                                                   |
   |                       |                       |                                                                                                                            |
   |                       |                       | **Constraints**                                                                                                            |
   |                       |                       |                                                                                                                            |
   |                       |                       | N/A                                                                                                                        |
   |                       |                       |                                                                                                                            |
   |                       |                       | **Range**                                                                                                                  |
   |                       |                       |                                                                                                                            |
   |                       |                       | -  It can contain a maximum of 128 Unicode characters. It cannot be an empty string, and cannot start or end with a space. |
   |                       |                       |                                                                                                                            |
   |                       |                       | -  It cannot contain the following characters: ``=*<>\,|/``                                                                |
   |                       |                       |                                                                                                                            |
   |                       |                       | -  Only uppercase letters, lowercase letters, digits, underscores (_), and hyphens (-) are allowed.                        |
   |                       |                       |                                                                                                                            |
   |                       |                       | **Default Value**                                                                                                          |
   |                       |                       |                                                                                                                            |
   |                       |                       | N/A                                                                                                                        |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | value                 | String                | **Definition**                                                                                                             |
   |                       |                       |                                                                                                                            |
   |                       |                       | Tag value.                                                                                                                 |
   |                       |                       |                                                                                                                            |
   |                       |                       | **Constraints**                                                                                                            |
   |                       |                       |                                                                                                                            |
   |                       |                       | N/A                                                                                                                        |
   |                       |                       |                                                                                                                            |
   |                       |                       | **Range**                                                                                                                  |
   |                       |                       |                                                                                                                            |
   |                       |                       | -  The value can contain a maximum of 256 characters and can be an empty string. It cannot start or end with a space.      |
   |                       |                       |                                                                                                                            |
   |                       |                       | -  It cannot contain the following characters: ``=*<>\,|/``                                                                |
   |                       |                       |                                                                                                                            |
   |                       |                       | -  Only uppercase letters, lowercase letters, digits, underscores (_), and hyphens (-) are allowed.                        |
   |                       |                       |                                                                                                                            |
   |                       |                       | **Default Value**                                                                                                          |
   |                       |                       |                                                                                                                            |
   |                       |                       | N/A                                                                                                                        |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+

Example Requests
----------------

Query the enterprise project information of a specified cluster.

.. code-block:: text

   GET https://{Endpoint}/v1/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/enterprise-projects

Example Responses
-----------------

**Status code: 200**

Query succeeded.

.. code-block::

   {
     "sys_tags" : [ {
       "key" : "key",
       "value" : "value"
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
