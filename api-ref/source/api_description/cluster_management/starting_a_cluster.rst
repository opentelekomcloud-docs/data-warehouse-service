:original_name: StartCluster.html

.. _StartCluster:

Starting a Cluster
==================

Function
--------

This API is used to start a cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v1/{project_id}/clusters/{cluster_id}/start

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

.. table:: **Table 2** Response body parameters

   +-----------------------+-----------------------+----------------------------------------+
   | Parameter             | Type                  | Description                            |
   +=======================+=======================+========================================+
   | job_id                | String                | **Definition**                         |
   |                       |                       |                                        |
   |                       |                       | ID of the task for starting a cluster. |
   |                       |                       |                                        |
   |                       |                       | **Range**                              |
   |                       |                       |                                        |
   |                       |                       | N/A                                    |
   +-----------------------+-----------------------+----------------------------------------+

Example Requests
----------------

Start a cluster.

.. code-block:: text

   POST https://{endpoint}/v1/89cd04f168b84af6be287f71730fdb4b/clusters/97cbaab3-939e-4dbc-9187-0fe240f2b9fd/start

Example Responses
-----------------

**Status code: 200**

Request submitted.

.. code-block::

   {
     "error_code" : null,
     "error_msg" : null,
     "job_id" : "2c9080d08cc99d28018ccd139e942498"
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Request submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
