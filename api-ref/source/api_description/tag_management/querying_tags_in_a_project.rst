:original_name: dws_02_0050.html

.. _dws_02_0050:

Querying Tags in a Project
==========================

Function
--------

This API is used to query the tags of a project.

URI
---

.. code-block:: text

   GET /v1.0/{project_id}/tags

.. table:: **Table 1** URI parameters

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

   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Parameter             | Type                                                                                                                                             | Description           |
   +=======================+==================================================================================================================================================+=======================+
   | tags                  | Array of :ref:`ProjectTag <en-us_topic_0000002361650974__en-us_topic_0000002375065777_en-us_topic_0000002340886604_response_projecttag>` objects | **Definition**        |
   |                       |                                                                                                                                                  |                       |
   |                       |                                                                                                                                                  | Tag object.           |
   |                       |                                                                                                                                                  |                       |
   |                       |                                                                                                                                                  | **Range**             |
   |                       |                                                                                                                                                  |                       |
   |                       |                                                                                                                                                  | N/A                   |
   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

.. _en-us_topic_0000002361650974__en-us_topic_0000002375065777_en-us_topic_0000002340886604_response_projecttag:

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

Example Request
---------------

.. code-block:: text

   GET https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/tags

Example Responses
-----------------

**Status code: 200**

The project tags are queried successfully.

.. code-block::

   {
     "tags" : [ {
       "key" : "key",
       "values" : [ "value-1", "value-2" ]
     } ]
   }

Status Code
-----------

=========== ==========================================
Status Code Description
=========== ==========================================
200         The project tags are queried successfully.
400         Request error.
401         Authorization failed.
403         No operation permission.
404         No resources found.
500         Internal service error.
503         Service unavailable.
=========== ==========================================
