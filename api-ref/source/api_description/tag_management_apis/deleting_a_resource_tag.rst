:original_name: dws_02_0051.html

.. _dws_02_0051:

Deleting a Resource Tag
=======================

Function
--------

This API is used to delete a resource tag for a resource.

URI
---

-  URI format

   .. code-block:: text

      DELETE /v1.0/{project_id}/clusters/{resource_id}/tags/{key}

-  Parameter description

   .. table:: **Table 1** URI parameter description

      +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
      | Parameter   | Mandatory | Type   | Description                                                                                                  |
      +=============+===========+========+==============================================================================================================+
      | project_id  | Yes       | String | Project ID. For details about how to obtain the project ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
      +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
      | resource_id | Yes       | String | Resource ID                                                                                                  |
      +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
      | key         | Yes       | String | Tag key                                                                                                      |
      +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+

Request Message
---------------

-  Sample request

   .. code-block:: text

      DELETE /v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/7d85f602-a948-4a30-afd4-e84f47471c15/tags/DEV

-  Parameter description

   None

Response Message
----------------

Sample response

.. code-block::

   status CODE 204

Status Code
-----------

-  Normal

   204

-  Abnormal

   .. table:: **Table 2** Returned value for failed requests

      +----------------+-----------------------------------------------------------------+
      | Returned Value | Description                                                     |
      +================+=================================================================+
      | 400            | Invalid parameters.                                             |
      +----------------+-----------------------------------------------------------------+
      | 401            | Authentication failed.                                          |
      +----------------+-----------------------------------------------------------------+
      | 403            | You do not have the permission to perform the operation.        |
      +----------------+-----------------------------------------------------------------+
      | 404            | The requested resource was not found or the key does not exist. |
      +----------------+-----------------------------------------------------------------+
      | 500            | Internal service error.                                         |
      +----------------+-----------------------------------------------------------------+
