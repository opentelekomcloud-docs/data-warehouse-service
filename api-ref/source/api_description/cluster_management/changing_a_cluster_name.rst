:original_name: ModifyClusterName.html

.. _ModifyClusterName:

Changing a Cluster Name
=======================

Function
--------

This API is used to change a cluster name.

**Constraints**

This parameter is available only for GuestAgent 8.3.1 or later.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

PUT /v1/{project_id}/clusters/{cluster_id}/cluster-name

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

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                        |
   +=================+=================+=================+====================================================================+
   | cluster_name    | No              | String          | **Definition**                                                     |
   |                 |                 |                 |                                                                    |
   |                 |                 |                 | Request for changing a cluster name.                               |
   |                 |                 |                 |                                                                    |
   |                 |                 |                 | **Constraints**                                                    |
   |                 |                 |                 |                                                                    |
   |                 |                 |                 | This parameter is available only for GuestAgent 8.3.1 or later.    |
   |                 |                 |                 |                                                                    |
   |                 |                 |                 | **Range**                                                          |
   |                 |                 |                 |                                                                    |
   |                 |                 |                 | The value must start with a letter and contain 3 to 64 characters. |
   |                 |                 |                 |                                                                    |
   |                 |                 |                 | **Default Value**                                                  |
   |                 |                 |                 |                                                                    |
   |                 |                 |                 | N/A                                                                |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-----------------------+--------------------------------------------------------+
   | Parameter             | Type                  | Description                                            |
   +=======================+=======================+========================================================+
   | error_code            | String                | **Definition**                                         |
   |                       |                       |                                                        |
   |                       |                       | Error code.                                            |
   |                       |                       |                                                        |
   |                       |                       | **Range**                                              |
   |                       |                       |                                                        |
   |                       |                       | N/A                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------+
   | error_msg             | String                | **Definition**                                         |
   |                       |                       |                                                        |
   |                       |                       | Error message.                                         |
   |                       |                       |                                                        |
   |                       |                       | **Range**                                              |
   |                       |                       |                                                        |
   |                       |                       | N/A                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------+
   | job_id                | String                | **Definition**                                         |
   |                       |                       |                                                        |
   |                       |                       | Task ID, which can be used to query the task progress. |
   |                       |                       |                                                        |
   |                       |                       | **Range**                                              |
   |                       |                       |                                                        |
   |                       |                       | N/A                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------+

Example Requests
----------------

Change the cluster name to **dws_cluster**.

.. code-block::

   put https://{Endpoint}/v1/05f2cff45100d5112f4bc00b794ea08e/clusters/9f76c502-fc9c-4a52-8656-65d0da6e3d57/cluster-name

   {
     "cluster_name" : "dws_cluster"
   }

Example Responses
-----------------

**Status code: 200**

Request submitted.

.. code-block::

   {
     "job_id" : "g6aa82ba1fc9748389b613e8da13e2fe9"
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
500         Internal server error.
503         Service unavailable.
=========== =====================================
