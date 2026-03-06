:original_name: ListLogicalClusterTasks.html

.. _ListLogicalClusterTasks:

Querying Task Information About a Logical Cluster
=================================================

Function
--------

This API is used to query task information about a logical cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/clusters/{cluster_id}/logical-clusters/tasks

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

   +----------------------+-----------------+-----------------+---------------------------------------------------------+
   | Parameter            | Mandatory       | Type            | Description                                             |
   +======================+=================+=================+=========================================================+
   | offset               | No              | Integer         | **Definition**                                          |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | Page offset, which starts from 0 (page number minus 1). |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Constraints**                                         |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Range**                                               |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | Greater than or equal to **0**                          |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Default Value**                                       |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | 0                                                       |
   +----------------------+-----------------+-----------------+---------------------------------------------------------+
   | limit                | No              | Integer         | **Definition**                                          |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | Size of a single page.                                  |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Constraints**                                         |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Range**                                               |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | Greater than 0                                          |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Default Value**                                       |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | 10                                                      |
   +----------------------+-----------------+-----------------+---------------------------------------------------------+
   | logical_cluster_name | No              | String          | **Definition**                                          |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | Cluster name.                                           |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Constraints**                                         |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Range**                                               |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Default Value**                                       |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   +----------------------+-----------------+-----------------+---------------------------------------------------------+
   | type                 | No              | String          | **Definition**                                          |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | Type.                                                   |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Constraints**                                         |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Range**                                               |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Default Value**                                       |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   +----------------------+-----------------+-----------------+---------------------------------------------------------+
   | order_by             | No              | String          | **Definition**                                          |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | Sorting field.                                          |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Constraints**                                         |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Range**                                               |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Default Value**                                       |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   +----------------------+-----------------+-----------------+---------------------------------------------------------+
   | order                | No              | String          | **Definition**                                          |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | Sorting order (ascending or descending)                 |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Constraints**                                         |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Range**                                               |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **ASC**                                                 |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **DESC**                                                |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | **Default Value**                                       |
   |                      |                 |                 |                                                         |
   |                      |                 |                 | N/A                                                     |
   +----------------------+-----------------+-----------------+---------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+
   | Parameter             | Type                                                                                                                                        | Description                                |
   +=======================+=============================================================================================================================================+============================================+
   | logical_cluster_tasks | Array of :ref:`LogicalClusterTaskInfo <en-us_topic_0000002500174028__en-us_topic_0000002375015009_response_logicalclustertaskinfo>` objects | **Definition**                             |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | Task information of the logical cluster.   |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | **Constraints**                            |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | N/A                                        |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | **Range**                                  |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | N/A                                        |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | **Default Value**                          |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | N/A                                        |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+
   | count                 | Long                                                                                                                                        | **Definition**                             |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | Total number of the logical cluster tasks. |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | **Constraints**                            |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | N/A                                        |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | **Range**                                  |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | N/A                                        |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | **Default Value**                          |
   |                       |                                                                                                                                             |                                            |
   |                       |                                                                                                                                             | N/A                                        |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+

.. _en-us_topic_0000002500174028__en-us_topic_0000002375015009_response_logicalclustertaskinfo:

.. table:: **Table 4** LogicalClusterTaskInfo

   +-----------------------+-----------------------+------------------------+
   | Parameter             | Type                  | Description            |
   +=======================+=======================+========================+
   | type                  | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Task type.             |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | logical_cluster_name  | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Logical cluster name.  |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | start_time            | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Task start time.       |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | end_time              | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Task end time.         |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | result                | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Task execution result. |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+
   | log                   | String                | **Definition**         |
   |                       |                       |                        |
   |                       |                       | Task execution log.    |
   |                       |                       |                        |
   |                       |                       | **Range**              |
   |                       |                       |                        |
   |                       |                       | N/A                    |
   +-----------------------+-----------------------+------------------------+

Example Requests
----------------

-  Query task information about a logical cluster.

   .. code-block:: text

      GET https://{Endpoint}/v2/9b06d044ea4f49f1a58b2bed2b0084bd/clusters/9b7ff56b-47b3-4d00-a1fd-4c023d34404b/logical-clusters/tasks

-  .. code-block:: text

      GET https://{Endpoint}/v2/9b06d044ea4f49f1a58b2bed2b0084bd/clusters/9b7ff56b-47b3-4d00-a1fd-4c023d34404b/logical-clusters/tasks?offset=0&limit=10&logical_cluster_name=test_logical&type=Expand&order_by=startTime&order=DESC

Example Responses
-----------------

**Status code: 200**

Task information queried.

.. code-block::

   {
     "logical_cluster_tasks" : [ {
       "type" : "Grow",
       "logical_cluster_name" : "elastic_group",
       "start_time" : "2023-06-05 01:58:43",
       "end_time" : "2023-06-05 02:11:50",
       "result" : "success",
       "log" : "Expand from outside success"
     }, {
       "type" : "Create",
       "logical_cluster_name" : "test_logical",
       "start_time" : "2023-06-21 08:35:58",
       "end_time" : "2023-06-21 08:36:14",
       "result" : "failed",
       "log" : "list index out of range\\nChecking whether the reentry command is consistent with the previous command."
     } ],
     "count" : 2
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Task information queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
