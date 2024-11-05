:original_name: dws_02_0049.html

.. _dws_02_0049:

Querying Resource Tags
======================

Function
--------

This API is used to query tags of a specified resource.

URI
---

.. code-block:: text

   GET /v1.0/{project_id}/clusters/{resource_id}/tags

.. table:: **Table 1** URI parameters

   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type   | Description                                                                                                  |
   +=============+===========+========+==============================================================================================================+
   | project_id  | Yes       | String | Project ID. For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | resource_id | Yes       | String | Resource ID, for example, **7d85f602-a948-4a30-afd4-e84f47471c15**.                                          |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

.. table:: **Table 2** Response body parameters

   +-----------+-----------+------------------------------------------------------------------------------+-------------+
   | Parameter | Mandatory | Type                                                                         | Description |
   +===========+===========+==============================================================================+=============+
   | tags      | Yes       | List<:ref:`resource_tag <en-us_topic_0000001231631325__table1150361719261>`> | Tag list.   |
   +-----------+-----------+------------------------------------------------------------------------------+-------------+

.. _en-us_topic_0000001231631325__table1150361719261:

.. table:: **Table 3** **resource_tag** field description

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                                                                      |
   +=================+=================+=================+==================================================================================================================================================================================================================================================================+
   | key             | Yes             | String          | Key. A tag key can contain a maximum of 36 Unicode characters, which cannot be null. The first and last characters cannot be spaces.                                                                                                                             |
   |                 |                 |                 |                                                                                                                                                                                                                                                                  |
   |                 |                 |                 | It can contain uppercase letters (A to Z), lowercase letters (a to z), digits (0-9), hyphens (-), and underscores (_).                                                                                                                                           |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value           | Yes             | String          | Key value. A tag value can contain a maximum of 43 Unicode characters, which can be null. The first and last characters cannot be spaces. It can contain uppercase letters (A to Z), lowercase letters (a to z), digits (0-9), hyphens (-), and underscores (_). |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

.. code-block:: text

   GET /v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/7d85f602-a948-4a30-afd4-e84f47471c15/tags

Example Responses
-----------------

.. code-block::

   {
       "tags": [
           {
               "key": "Flower",
               "value": "rose"
           },
           {
               "key": "Food",
               "value": "pie"
           }
       ]
   }

Status Code
-----------

============== ========================
Returned Value Description
============== ========================
200            OK
400            Invalid parameter.
401            Authentication failed.
403            Insufficient permission.
404            No resources found.
500            Internal service error.
============== ========================
