:original_name: SwitchOverCluster.html

.. _SwitchOverCluster:

Performing a Switchback
=======================

Function
--------

In the **Unbalanced** state, the number of primary instances on some nodes increases. As a result, the load pressure is high. In this case, the cluster is normal, but the overall performance is not as good as that in a balanced state. You can perform a primary/standby switchback to change the cluster status to **Available**.

**Constraints**

Only 8.1.1.202 and later versions support primary/standby switchbacks.

A switchback interrupts services for a short period of time. The interruption duration depends on the service volume. You are advised to perform this operation during off-peak hours.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v1.0/{project_id}/clusters/{cluster_id}/switchover

.. table:: **Table 1** Path Parameters

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                |
   +=================+=================+=================+============================================================================================================+
   | project_id      | Yes             | String          | **Definition**                                                                                             |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | Project ID. To obtain the value, see :ref:`Obtaining a Project ID <dws_02_0011>`.                          |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Constraints**                                                                                            |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Range**                                                                                                  |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Default Value**                                                                                          |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+
   | cluster_id      | Yes             | String          | **Definition**                                                                                             |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | Cluster ID. For details about how to obtain the value, see :ref:`Obtaining the Cluster ID <dws_02_00068>`. |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Constraints**                                                                                            |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Range**                                                                                                  |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Default Value**                                                                                          |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

The request for performing a primary/standby switchback is submitted.

None

Example Requests
----------------

Perform a primary/standby switchback.

.. code-block:: text

   POST https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/switchover

Example Responses
-----------------

**Status code: 200**

The request for performing a primary/standby switchback is submitted.

.. code-block::

   { }

Status Codes
------------

+-------------+-----------------------------------------------------------------------+
| Status Code | Description                                                           |
+=============+=======================================================================+
| 200         | The request for performing a primary/standby switchback is submitted. |
+-------------+-----------------------------------------------------------------------+
| 400         | Request error.                                                        |
+-------------+-----------------------------------------------------------------------+
| 401         | Authentication failed.                                                |
+-------------+-----------------------------------------------------------------------+
| 403         | You do not have required permissions.                                 |
+-------------+-----------------------------------------------------------------------+
| 404         | No resources found.                                                   |
+-------------+-----------------------------------------------------------------------+
| 500         | Internal server error.                                                |
+-------------+-----------------------------------------------------------------------+
| 503         | Service unavailable.                                                  |
+-------------+-----------------------------------------------------------------------+
