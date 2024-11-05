:original_name: dws_02_0050.html

.. _dws_02_0050:

Querying Tags in a Specified Project
====================================

Function
--------

This API is used to query all tags of a tenant for a specified resource type in a specified project.

URI
---

.. code-block:: text

   GET /v1.0/{project_id}/clusters/tags

.. table:: **Table 1** URI parameter description

   +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type   | Description                                                                                                  |
   +============+===========+========+==============================================================================================================+
   | project_id | Yes       | String | Project ID. For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

.. table:: **Table 2** Response body parameters

   +-----------+-----------+---------------------------------------------------------------------+-------------+
   | Parameter | Mandatory | Type                                                                | Description |
   +===========+===========+=====================================================================+=============+
   | tags      | Yes       | List<:ref:`tag <en-us_topic_0000001185991708__table1682218436286>`> | Tag list.   |
   +-----------+-----------+---------------------------------------------------------------------+-------------+

.. _en-us_topic_0000001185991708__table1682218436286:

.. table:: **Table 3** tag

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                                                                  |
   +=================+=================+=================+==============================================================================================================================================================================================================================================================+
   | key             | Yes             | String          | Key. A tag key can contain a maximum of 36 Unicode characters, which cannot be null. The first and last characters cannot be spaces.                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                                                                                              |
   |                 |                 |                 | It can contain uppercase letters (A to Z), lowercase letters (a to z), digits (0-9), hyphens (-), and underscores (_).                                                                                                                                       |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | values          | Yes             | List<String>    | Value. A tag value can contain a maximum of 43 Unicode characters, which can be null. The first and last characters cannot be spaces. It can contain uppercase letters (A to Z), lowercase letters (a to z), digits (0-9), hyphens (-), and underscores (_). |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

.. code-block:: text

   GET /v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/tags

Example Responses
-----------------

.. code-block::

   {
         "tags": [
           {
               "key": "key1",
               "values": [
                   "value1",
                   "value2"
                ]
           },
           {
               "key": "key2",
               "values": [
                   "value1",
                   "value2"
                ]
           }
       ]
   }

Status Code
-----------

============== ========================
Returned Value Description
============== ========================
200            Query succeeded.
400            Invalid parameter.
401            Authentication failed.
403            Insufficient permission.
404            No resources found.
500            Internal service error.
============== ========================
