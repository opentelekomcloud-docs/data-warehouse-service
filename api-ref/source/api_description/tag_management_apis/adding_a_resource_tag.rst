:original_name: dws_02_0046.html

.. _dws_02_0046:

Adding a Resource Tag
=====================

Function
--------

This API is used to add a resource tag for a resource. A maximum of 10 tags can be added for one resource.

URI
---

.. code-block:: text

   POST /v1.0/{project_id}/clusters/{resource_id}/tags

.. table:: **Table 1** URI parameters

   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type   | Description                                                                                                  |
   +=============+===========+========+==============================================================================================================+
   | project_id  | Yes       | String | Project ID. For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | resource_id | Yes       | String | Resource ID, Currently, you can only add tags to a cluster, so specify this parameter to the cluster ID.     |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------+-----------+--------------------------------------------------------------------+-------------+
   | Parameter | Mandatory | Type                                                               | Description |
   +===========+===========+====================================================================+=============+
   | tag       | Yes       | :ref:`tag <en-us_topic_0000001185833146__table12208596184>` object | Tags.       |
   +-----------+-----------+--------------------------------------------------------------------+-------------+

.. _en-us_topic_0000001185833146__table12208596184:

.. table:: **Table 3** **tag** field description

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                                                                      |
   +=================+=================+=================+==================================================================================================================================================================================================================================================================+
   | key             | Yes             | String          | Tag key. A tag key can contain a maximum of 36 Unicode characters, which cannot be null. The first and last characters cannot be spaces.                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                                                                                                  |
   |                 |                 |                 | It can contain uppercase letters (A to Z), lowercase letters (a to z), digits (0-9), hyphens (-), and underscores (_).                                                                                                                                           |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value           | Yes             | String          | Key value. A tag value can contain a maximum of 43 Unicode characters, which can be null. The first and last characters cannot be spaces. It can contain uppercase letters (A to Z), lowercase letters (a to z), digits (0-9), hyphens (-), and underscores (_). |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response Parameters
-------------------

None

Example Request
---------------

.. code-block:: text

   POST /v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/7d85f602-a948-4a30-afd4-e84f47471c15/tags
   {
        "tag":
        {
           "key":"Flower",
           "value":"rose"
        }
   }

Example Responses
-----------------

None

Status Code
-----------

============== ========================
Returned Value Description
============== ========================
204            Tags added.
400            Invalid tag.
401            Authentication failed.
403            Insufficient permission.
404            No resources found.
500            Internal service error.
============== ========================
