:original_name: DeleteWorkloadRule.html

.. _DeleteWorkloadRule:

Deleting an Exception Rule
==========================

Function
--------

This API is used to delete an exception rule.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

DELETE /v1/{project_id}/clusters/{cluster_id}/workload/rules/{rule_name}

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
   | rule_name       | Yes             | String          | **Definition**                                                                                             |
   |                 |                 |                 |                                                                                                            |
   |                 |                 |                 | Exception rule name.                                                                                       |
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
   | items                 | Array of :ref:`ExceptRuleBo <en-us_topic_0000002500014100__en-us_topic_0000002341097088_response_exceptrulebo>` objects | **Definition**                   |
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

.. _en-us_topic_0000002500014100__en-us_topic_0000002341097088_response_exceptrulebo:

.. table:: **Table 3** ExceptRuleBo

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

Delete an exception rule.

.. code-block:: text

   DELETE https://{Endpoint}/v1/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/workload/rules/rule

Example Responses
-----------------

**Status code: 200**

Operation succeeded.

.. code-block::

   {
     "workload_res_code" : 0,
     "workload_res_str" : null
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
