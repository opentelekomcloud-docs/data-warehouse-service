:original_name: ListWorkloadRules.html

.. _ListWorkloadRules:

Querying the Exception Rule List of a Cluster
=============================================

Function
--------

This API is used to query the exception rule list of a cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1/{project_id}/clusters/{cluster_id}/workload/rules

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
   |                 |                 |                 | The value must be a valid DWS cluster ID.                                                                  |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Range**                                                                                                  |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | It is a 36-digit UUID.                                                                                     |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | **Default Value**                                                                                          |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | N/A                                                                                                        |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------+

.. table:: **Table 2** Query Parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                             |
   +=================+=================+=================+=========================================================+
   | offset          | No              | Integer         | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Page offset, which starts from 0 (page number minus 1). |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Greater than or equal to **0**                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | 0                                                       |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | limit           | No              | Integer         | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Size of a single page.                                  |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | 10                                                      |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | rule_name       | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Size of a single page.                                  |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Exception rule name.                                    |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+
   | queue_name      | No              | String          | **Definition**                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | Resource pool name.                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Constraints**                                         |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Range**                                               |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | N/A                                                     |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+----------------------------------+
   | Parameter             | Type                                                                                                                    | Description                      |
   +=======================+=========================================================================================================================+==================================+
   | workload_res_code     | Integer                                                                                                                 | **Definition**                   |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | Error code.                      |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | **Range**                        |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | N/A                              |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+----------------------------------+
   | workload_res_str      | String                                                                                                                  | **Definition**                   |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | Error details.                   |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | **Range**                        |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | N/A                              |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+----------------------------------+
   | items                 | Array of :ref:`ExceptRuleBo <en-us_topic_0000002532013981__en-us_topic_0000002374935257_response_exceptrulebo>` objects | **Definition**                   |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | Exception rule list.             |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | **Range**                        |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | N/A                              |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+----------------------------------+
   | count                 | Integer                                                                                                                 | **Definition**                   |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | Total number of exception rules. |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | **Range**                        |
   |                       |                                                                                                                         |                                  |
   |                       |                                                                                                                         | Greater than or equal to **0**   |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+----------------------------------+

.. _en-us_topic_0000002532013981__en-us_topic_0000002374935257_response_exceptrulebo:

.. table:: **Table 4** ExceptRuleBo

   +-----------------------+-----------------------+-----------------------------------------------------+
   | Parameter             | Type                  | Description                                         |
   +=======================+=======================+=====================================================+
   | name                  | String                | **Definition**                                      |
   |                       |                       |                                                     |
   |                       |                       | Rule name.                                          |
   |                       |                       |                                                     |
   |                       |                       | **Range**                                           |
   |                       |                       |                                                     |
   |                       |                       | N/A                                                 |
   +-----------------------+-----------------------+-----------------------------------------------------+
   | action                | String                | **Definition**                                      |
   |                       |                       |                                                     |
   |                       |                       | Action that triggers an exception rule.             |
   |                       |                       |                                                     |
   |                       |                       | **Range**                                           |
   |                       |                       |                                                     |
   |                       |                       | N/A                                                 |
   +-----------------------+-----------------------+-----------------------------------------------------+
   | queues                | Array of strings      | **Definition**                                      |
   |                       |                       |                                                     |
   |                       |                       | Names of resource pools bound to an exception rule. |
   |                       |                       |                                                     |
   |                       |                       | **Range**                                           |
   |                       |                       |                                                     |
   |                       |                       | N/A                                                 |
   +-----------------------+-----------------------+-----------------------------------------------------+
   | except_rules          | Map<String,String>    | **Definition**                                      |
   |                       |                       |                                                     |
   |                       |                       | Exception rule configuration items.                 |
   |                       |                       |                                                     |
   |                       |                       | **Range**                                           |
   |                       |                       |                                                     |
   |                       |                       | N/A                                                 |
   +-----------------------+-----------------------+-----------------------------------------------------+

Example Requests
----------------

Query the exception rule list.

.. code-block:: text

   GET https://{Endpoint}/v1/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/workload/rules

Example Responses
-----------------

**Status code: 200**

Query succeeded.

.. code-block::

   {
     "workload_res_code" : 0,
     "workload_res_str" : null,
     "items" : [ {
       "name" : "default_cpu_percent",
       "action" : "abort",
       "queues" : [ ],
       "except_rules" : {
         "action" : "abort",
         "cpuavgpercent" : "50",
         "elapsedtime" : "900"
       }
     } ],
     "count" : 3
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Query succeeded.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
