:original_name: dws_02_0021.html

.. _dws_02_0021:

Deleting a Cluster
==================

Function
--------

This API is used to delete clusters. All resources of the deleted cluster, including customer data, will be released. For data security, create a snapshot for the cluster that you want to delete before deleting the cluster.

URI
---

.. code-block:: text

   DELETE /v1.0/{project_id}/clusters/{cluster_id}

.. table:: **Table 1** URI parameters

   +------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type   | Description                                                                                                                  |
   +============+===========+========+==============================================================================================================================+
   | project_id | Yes       | String | Project ID. For details about how to obtain the ID, see :ref:`Obtaining a Project ID <dws_02_0011>`.                         |
   +------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------+
   | cluster_id | Yes       | String | ID of the cluster to be deleted. For details about how to obtain the ID, see :ref:`Obtaining the Cluster ID <dws_02_00068>`. |
   +------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +---------------------------+-----------+---------+-----------------------------------+
   | Parameter                 | Mandatory | Type    | Description                       |
   +===========================+===========+=========+===================================+
   | keep_last_manual_snapshot | Yes       | Integer | Keeps the latest manual snapshot. |
   +---------------------------+-----------+---------+-----------------------------------+

Response Parameters
-------------------

None

Example Request
---------------

.. code-block:: text

   DELETE https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90

   {
     "keep_last_manual_snapshot" : 0
   }

Example Responses
-----------------

None

Status Code
-----------

=========== =====================================
Status Code Description
=========== =====================================
202         The cluster has been terminated.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal service error.
503         Service unavailable.
=========== =====================================
