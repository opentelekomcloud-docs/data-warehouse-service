:original_name: EnableLogicalCluster.html

.. _EnableLogicalCluster:

Switching to a Logical Cluster
==============================

Function
--------

This API is used to control whether the function modules related to a logical cluster are displayed on the page.

If the cluster is already a logical cluster, this API has no function or impact.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

PUT /v2/{project_id}/clusters/{cluster_id}/logical-clusters/enable

.. table:: **Table 1** Path Parameters

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                |
   +=================+=================+=================+============================================================================================================+
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

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+-----------------+--------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                            |
   +=================+=================+=================+========================================================+
   | enable          | Yes             | Boolean         | **Definition**                                         |
   |                 |                 |                 |                                                        |
   |                 |                 |                 | Request for converting to a logic cluster.             |
   |                 |                 |                 |                                                        |
   |                 |                 |                 | **Constraints**                                        |
   |                 |                 |                 |                                                        |
   |                 |                 |                 | N/A                                                    |
   |                 |                 |                 |                                                        |
   |                 |                 |                 | **Range**                                              |
   |                 |                 |                 |                                                        |
   |                 |                 |                 | **true**: The **Logical Cluster** switch is enabled.   |
   |                 |                 |                 |                                                        |
   |                 |                 |                 | **false**: The **Logical Cluster** switch is disabled. |
   |                 |                 |                 |                                                        |
   |                 |                 |                 | **Default Value**                                      |
   |                 |                 |                 |                                                        |
   |                 |                 |                 | N/A                                                    |
   +-----------------+-----------------+-----------------+--------------------------------------------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | error_code            | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Error code.           |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | error_msg             | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Error message.        |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

Switch to a logical cluster.

.. code-block:: text

   PUT https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/logical-clusters/enable

   {
     "enable" : true
   }

Example Responses
-----------------

**Status code: 200**

Cluster switch changed.

.. code-block::

   {
     "error_code" : "DWS.0000",
     "error_msg" : null
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Cluster switch changed.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
