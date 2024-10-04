:original_name: dws_02_0053.html

.. _dws_02_0053:

Scaling Out a Cluster
=====================

Function
--------

This API is used to scale out a cluster.

URI
---

.. code-block:: text

   POST /v1.0/{project_id}/clusters/{cluster_id}/resize

.. table:: **Table 1** URI parameters

   +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type   | Description                                                                                                                     |
   +============+===========+========+=================================================================================================================================+
   | project_id | Yes       | String | Project ID. For details about how to obtain the ID, see :ref:`Obtaining a Project ID <dws_02_0011>`.                            |
   +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------+
   | cluster_id | Yes       | String | ID of the cluster to be scaled out. For details about how to obtain the ID, see :ref:`Obtaining the Cluster ID <dws_02_00068>`. |
   +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------+-----------+----------------------------------------------------------------------------+-------------------------+
   | Parameter | Mandatory | Type                                                                       | Description             |
   +===========+===========+============================================================================+=========================+
   | scale_out | No        | :ref:`ScaleOut <en-us_topic_0000001231472767__table27082031182617>` object | Object to be scaled out |
   +-----------+-----------+----------------------------------------------------------------------------+-------------------------+

.. _en-us_topic_0000001231472767__table27082031182617:

.. table:: **Table 3** ScaleOut

   ========= ========= ======= ===========================
   Parameter Mandatory Type    Description
   ========= ========= ======= ===========================
   count     Yes       Integer Number of nodes to be added
   ========= ========= ======= ===========================

Response Parameters
-------------------

None

Example Request
---------------

An example of the scale_out API is as follows:

.. code-block:: text

   POST https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/resize

   {
     "scale_out" : {
       "count" : 3
     }
   }

Response Message
----------------

None

Status Code
-----------

=========== =======================================
Status Code Description
=========== =======================================
200         The cluster is scaled out successfully.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal service error.
503         Service unavailable.
=========== =======================================
