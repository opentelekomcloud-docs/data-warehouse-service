:original_name: AddWorkloadRule.html

.. _AddWorkloadRule:

Adding an Exception Rule
========================

Function
--------

This API is used to add an exception rule.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v1/{project_id}/clusters/{cluster_id}/workload/rules

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

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------+-------------------------------------+
   | Parameter       | Mandatory       | Type                                                                                                               | Description                         |
   +=================+=================+====================================================================================================================+=====================================+
   | rule_name       | No              | String                                                                                                             | **Definition**                      |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | Exception rule name.                |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | **Constraints**                     |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | It cannot be left blank.            |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | **Range**                           |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | N/A                                 |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | **Default Value**                   |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | N/A                                 |
   +-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------+-------------------------------------+
   | except_rules    | No              | Array of :ref:`ExceptRule <en-us_topic_0000002532013899__en-us_topic_0000002340937300_request_exceptrule>` objects | **Definition**                      |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | Exception rule configuration items. |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | **Constraints**                     |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | It cannot be left blank.            |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | **Range**                           |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | N/A                                 |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | **Default Value**                   |
   |                 |                 |                                                                                                                    |                                     |
   |                 |                 |                                                                                                                    | N/A                                 |
   +-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------+-------------------------------------+

.. _en-us_topic_0000002532013899__en-us_topic_0000002340937300_request_exceptrule:

.. table:: **Table 3** ExceptRule

   +-----------------+-----------------+-----------------+-------------------+
   | Parameter       | Mandatory       | Type            | Description       |
   +=================+=================+=================+===================+
   | rule_key        | No              | String          | **Definition**    |
   |                 |                 |                 |                   |
   |                 |                 |                 | Rule item name.   |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Constraints**   |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Range**         |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Default Value** |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   +-----------------+-----------------+-----------------+-------------------+
   | rule_value      | No              | String          | **Definition**    |
   |                 |                 |                 |                   |
   |                 |                 |                 | Rule item value.  |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Constraints**   |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Range**         |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   |                 |                 |                 |                   |
   |                 |                 |                 | **Default Value** |
   |                 |                 |                 |                   |
   |                 |                 |                 | N/A               |
   +-----------------+-----------------+-----------------+-------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 4** Response body parameters

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
   | items                 | Array of :ref:`ExceptRuleBo <en-us_topic_0000002532013899__en-us_topic_0000002340937300_response_exceptrulebo>` objects | **Definition**                   |
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

.. _en-us_topic_0000002532013899__en-us_topic_0000002340937300_response_exceptrulebo:

.. table:: **Table 5** ExceptRuleBo

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

Create an exception rule.

.. code-block:: text

   POST https://{Endpoint}/v1/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/workload/rules

   {
     "rule_name" : "rule910_1",
     "except_rules" : [ {
       "rule_key" : "blocktime",
       "rule_value" : "600"
     }, {
       "rule_key" : "elapsedtime",
       "rule_value" : "300"
     }, {
       "rule_key" : "action",
       "rule_value" : "abort"
     } ]
   }

Example Responses
-----------------

**Status code: 200**

Operation succeeded.

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
200         Operation succeeded.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
