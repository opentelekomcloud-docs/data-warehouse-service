:original_name: dws_02_0027.html

.. _dws_02_0027:

Deleting a Manual Snapshot
==========================

Function
--------

This API is used to delete a specified manual snapshot.

URI
---

.. code-block:: text

   DELETE /v1.0/{project_id}/snapshots/{snapshot_id}

.. table:: **Table 1** URI parameter description

   +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type   | Description                                                                                          |
   +=============+===========+========+======================================================================================================+
   | project_id  | Yes       | String | Project ID. For details about how to obtain the ID, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+
   | snapshot_id | Yes       | String | Snapshot ID                                                                                          |
   +-------------+-----------+--------+------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

None

Example Requests
----------------

.. code-block:: text

   DELETE https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/snapshots/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90

Example Response
----------------

None

Status Code
-----------

=========== =====================================
Status Code Description
=========== =====================================
202         The snapshot is deleted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal service error.
503         Service unavailable.
=========== =====================================
