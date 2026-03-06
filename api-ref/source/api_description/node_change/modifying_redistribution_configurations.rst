:original_name: UpdateRedistributionConfigurations.html

.. _UpdateRedistributionConfigurations:

Modifying Redistribution Configurations
=======================================

Function
--------

This API is used to modify redistribution configurations.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

PUT /v2/{project_id}/clusters/{cluster_id}/redistribution/configurations

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

   +-----------------+-----------------+-----------------+------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                              |
   +=================+=================+=================+==========================================+
   | parallel_jobs   | Yes             | Integer         | **Definition**                           |
   |                 |                 |                 |                                          |
   |                 |                 |                 | Concurrency. The default value is **4**. |
   |                 |                 |                 |                                          |
   |                 |                 |                 | **Constraints**                          |
   |                 |                 |                 |                                          |
   |                 |                 |                 | N/A                                      |
   |                 |                 |                 |                                          |
   |                 |                 |                 | **Range**                                |
   |                 |                 |                 |                                          |
   |                 |                 |                 | **1** to **200**                         |
   |                 |                 |                 |                                          |
   |                 |                 |                 | **Default Value**                        |
   |                 |                 |                 |                                          |
   |                 |                 |                 | N/A                                      |
   +-----------------+-----------------+-----------------+------------------------------------------+
   | priority_policy | Yes             | String          | **Definition**                           |
   |                 |                 |                 |                                          |
   |                 |                 |                 | Redistribution priority policy.          |
   |                 |                 |                 |                                          |
   |                 |                 |                 | **Constraints**                          |
   |                 |                 |                 |                                          |
   |                 |                 |                 | N/A                                      |
   |                 |                 |                 |                                          |
   |                 |                 |                 | **Range**                                |
   |                 |                 |                 |                                          |
   |                 |                 |                 | -  **default**: default policy           |
   |                 |                 |                 |                                          |
   |                 |                 |                 | -  **small**: small tables first         |
   |                 |                 |                 |                                          |
   |                 |                 |                 | -  **large**: large tables first         |
   |                 |                 |                 |                                          |
   |                 |                 |                 | **Default Value**                        |
   |                 |                 |                 |                                          |
   |                 |                 |                 | N/A                                      |
   +-----------------+-----------------+-----------------+------------------------------------------+

Response Parameters
-------------------

**Status code: 200**

Operation succeeded.

None

Example Requests
----------------

Set the number of concurrent tasks to 4 and the redistribution priority to large tables.

.. code-block:: text

   PUT https://{Endpoint}/v2/05f2cff45100d5112f4bc00b794ea08e/clusters/496c3032-c08f-4cd1-8c66-9b56a89a274d/redistribution/configurations

   {
     "parallel_jobs" : 4,
     "priority_policy" : "large"
   }

Example Responses
-----------------

**Status code: 200**

Operation succeeded.

.. code-block::

   { }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Operation succeeded.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
