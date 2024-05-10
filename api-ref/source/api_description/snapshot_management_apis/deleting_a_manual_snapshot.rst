:original_name: dws_02_0027.html

.. _dws_02_0027:

Deleting a Manual Snapshot
==========================

Function
--------

This API is used to delete a specified manual snapshot.

URI
---

-  URI format

   .. code-block:: text

      DELETE /v1.0/{project_id}/snapshots/{snapshot_id}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+
      | Parameter   | Mandatory | Type   | Description                                                                                          |
      +=============+===========+========+======================================================================================================+
      | project_id  | Yes       | String | Project ID. For details about how to obtain the ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
      +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+
      | snapshot_id | Yes       | String | Snapshot ID                                                                                          |
      +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+

Request Message
---------------

Request example

.. code-block:: text

   DELETE https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/snapshots/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90

Response Message
----------------

Example response

.. code-block::

   status CODE 202

Status Code
-----------

-  Normal

   202

-  Exception

   .. table:: **Table 2** Returned values

      ========================= ===========================
      Returned Value            Description
      ========================= ===========================
      400 Bad Request           Request error.
      401 Unauthorized          Authorization failed.
      403 Forbidden             No operation permission.
      404 Not Found             No resources found.
      500 Internal Server Error Internal service error.
      503 Service Unavailable   The service is unavailable.
      ========================= ===========================
