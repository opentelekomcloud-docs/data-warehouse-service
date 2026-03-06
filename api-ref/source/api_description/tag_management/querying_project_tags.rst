:original_name: ListTags.html

.. _ListTags:

Querying Project Tags
=====================

Function
--------

This API is used to query project tag list.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1.0/{project_id}/tags

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

   +-----------------------+---------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Parameter             | Type                                                                                                                | Description           |
   +=======================+=====================================================================================================================+=======================+
   | tags                  | Array of :ref:`ProjectTag <en-us_topic_0000002500014046__en-us_topic_0000002341097616_response_projecttag>` objects | **Definition**        |
   |                       |                                                                                                                     |                       |
   |                       |                                                                                                                     | Tag list objects.     |
   |                       |                                                                                                                     |                       |
   |                       |                                                                                                                     | **Range**             |
   |                       |                                                                                                                     |                       |
   |                       |                                                                                                                     | N/A                   |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------+-----------------------+

.. _en-us_topic_0000002500014046__en-us_topic_0000002341097616_response_projecttag:

.. table:: **Table 3** ProjectTag

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | key                   | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Key.                  |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | values                | Array of strings      | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Value.                |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

.. code-block:: text

   GET https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/tags

Example Responses
-----------------

**Status code: 200**

Project tags queried.

.. code-block::

   {
     "tags" : [ {
       "key" : "key",
       "values" : [ "value-1", "value-2" ]
     } ]
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Project tags queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
