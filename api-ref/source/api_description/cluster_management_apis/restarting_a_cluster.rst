:original_name: dws_02_0052.html

.. _dws_02_0052:

Restarting a Cluster
====================

Function
--------

This API is used to restart clusters.

URI
---

-  URI format

   .. code-block:: text

      POST /v1.0/{project_id}/clusters/{cluster_id}/restart

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                    |
      +============+===========+========+================================================================================================================================+
      | project_id | Yes       | String | Project ID. For details about how to obtain the ID, see :ref:`Obtaining a Project ID <dws_02_0011>`.                           |
      +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------+
      | cluster_id | Yes       | String | ID of the cluster to be restarted. For details about how to obtain the ID, see :ref:`Obtaining the Cluster ID <dws_02_00068>`. |
      +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------+

Request Message
---------------

-  Request example

   .. code-block:: text

      POST /v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/restart
      {
          "restart":{}
      }

-  Parameter description

   .. table:: **Table 2** Request parameters

      ========= ========= ====== =============
      Parameter Mandatory Type   Description
      ========= ========= ====== =============
      restart   Yes       Object Restart flag.
      ========= ========= ====== =============

Response Message
----------------

Example response

.. code-block::

   status CODE 200

Status Code
-----------

-  Normal

   200

-  Exception

   .. table:: **Table 3** Returned values

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
