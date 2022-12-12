:original_name: dws_02_0046.html

.. _dws_02_0046:

Adding a Resource Tag
=====================

Function
--------

This API is used to add a resource tag for a resource. A maximum of 10 tags can be added for one resource.

URI
---

-  URI format

   .. code-block:: text

      POST /v1.0/{project_id}/clusters/{resource_id}/tags

-  Parameter description

   .. table:: **Table 1** URI parameters

      +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
      | Parameter   | Mandatory | Type   | Description                                                                                                  |
      +=============+===========+========+==============================================================================================================+
      | project_id  | Yes       | String | Project ID. For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
      +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
      | resource_id | Yes       | String | Resource ID. Currently, you can only add tags to a cluster, so specify this parameter to the cluster ID.     |
      +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+

Request Message
---------------

-  Request example

   .. code-block:: text

      POST /v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/7d85f602-a948-4a30-afd4-e84f47471c15/tags
      {
           "tag":
           {
              "key":"Flower",
              "value":"rose"
           }
      }

-  Parameter description

   .. table:: **Table 2** Request parameters

      +-----------+-----------+---------------------------------------------------------------------+-------------+
      | Parameter | Mandatory | Type                                                                | Description |
      +===========+===========+=====================================================================+=============+
      | tag       | Yes       | :ref:`tag <en-us_topic_0000001180324321__table354113151341>` object | Tags.       |
      +-----------+-----------+---------------------------------------------------------------------+-------------+

   .. _en-us_topic_0000001180324321__table354113151341:

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

Response Message
----------------

Example response

.. code-block::

   status CODE 204

Status Code
-----------

-  Normal

   .. table:: **Table 4** Returned value for successful requests

      ============== ===========
      Returned Value Description
      ============== ===========
      204            No Content
      ============== ===========

-  Exception

   .. table:: **Table 5** Returned value for failed requests

      ============== ========================================================
      Returned Value Description
      ============== ========================================================
      400            Invalid tag.
      401            Authentication failed.
      403            You do not have the permission to perform the operation.
      404            The requested resource was not found.
      500            Internal service error.
      ============== ========================================================
