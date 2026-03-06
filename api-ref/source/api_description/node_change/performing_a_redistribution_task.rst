:original_name: ExecuteRedistributionCluster.html

.. _ExecuteRedistributionCluster:

Performing a Redistribution Task
================================

Function
--------

This API is used to evenly distribute data from old nodes to new nodes after cluster scale-out. After data redistribution, the service response speed is greatly improved.

The redistribution function is supported by DWS clusters of version 2.0 8.1.1.200 or later.

Offline scheduling is not supported in 8.2.0 or later.

This function can be manually enabled only when the cluster task information displays **To be redistributed** after scale-out.

You can also select the redistribution mode when you configure cluster scale-out (see Configure advanced parameters).

Redistribution queues are sorted based on the relpage size of tables. To ensure that the relpage size is correct, you are advised to perform the **ANALYZE** operation on the tables to be redistributed.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v2/{project_id}/clusters/{cluster_id}/redistribution

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

   +-----------------+-----------------+-----------------+-----------------------------+
   | Parameter       | Mandatory       | Type            | Description                 |
   +=================+=================+=================+=============================+
   | redis_mode      | Yes             | String          | **Definition**              |
   |                 |                 |                 |                             |
   |                 |                 |                 | Redistribution mode.        |
   |                 |                 |                 |                             |
   |                 |                 |                 | **Constraints**             |
   |                 |                 |                 |                             |
   |                 |                 |                 | N/A                         |
   |                 |                 |                 |                             |
   |                 |                 |                 | **Range**                   |
   |                 |                 |                 |                             |
   |                 |                 |                 | **online**                  |
   |                 |                 |                 |                             |
   |                 |                 |                 | **offline**                 |
   |                 |                 |                 |                             |
   |                 |                 |                 | **Default Value**           |
   |                 |                 |                 |                             |
   |                 |                 |                 | offline                     |
   +-----------------+-----------------+-----------------+-----------------------------+
   | parallel_jobs   | Yes             | Integer         | **Definition**              |
   |                 |                 |                 |                             |
   |                 |                 |                 | Redistribution concurrency. |
   |                 |                 |                 |                             |
   |                 |                 |                 | **Constraints**             |
   |                 |                 |                 |                             |
   |                 |                 |                 | N/A                         |
   |                 |                 |                 |                             |
   |                 |                 |                 | **Range**                   |
   |                 |                 |                 |                             |
   |                 |                 |                 | **4** to **200**            |
   |                 |                 |                 |                             |
   |                 |                 |                 | **Default Value**           |
   |                 |                 |                 |                             |
   |                 |                 |                 | **4**                       |
   +-----------------+-----------------+-----------------+-----------------------------+

Response Parameters
-------------------

**Status code: 200**

Redistribution task submitted.

None

Example Requests
----------------

Perform offline cluster redistribution task and set the number of concurrent tasks to 100.

.. code-block:: text

   POST https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/e59d6b86-9072-46eb-a996-13f8b44994c1/redistribution

   {
     "redis_mode" : "offline",
     "parallel_jobs" : 100
   }

Example Responses
-----------------

**Status code: 200**

Redistribution task submitted.

.. code-block::

   { }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Redistribution task submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
