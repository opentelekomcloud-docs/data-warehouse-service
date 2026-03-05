:original_name: ResizeClusterWithExistedNodes.html

.. _ResizeClusterWithExistedNodes:

Scaling Out a Cluster with Idle Nodes
=====================================

Function
--------

This API is used to scale out a cluster with idle nodes.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v2/{project_id}/clusters/{cluster_id}/resize-with-existed-nodes

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

   +--------------------------+-----------------+--------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter                | Mandatory       | Type                                                                                                         | Description                                                                                                                 |
   +==========================+=================+==============================================================================================================+=============================================================================================================================+
   | scale_out                | Yes             | :ref:`ScaleOut <en-us_topic_0000002531893991__en-us_topic_0000002340937144_request_scaleout>` object         | **Definition**                                                                                                              |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | Scale-out objects                                                                                                           |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Constraints**                                                                                                             |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Range**                                                                                                                   |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Default Value**                                                                                                           |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   +--------------------------+-----------------+--------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | force_backup             | No              | Boolean                                                                                                      | **Definition**                                                                                                              |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | Whether to forcibly back up data.                                                                                           |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Constraints**                                                                                                             |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Range**                                                                                                                   |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Default Value**                                                                                                           |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   +--------------------------+-----------------+--------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | mode                     | No              | String                                                                                                       | **Definition**                                                                                                              |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | Scale-out mode. If this parameter is not specified, the offline read-only mode is used by default.                          |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Constraints**                                                                                                             |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | The online mode is not supported by most clusters of earlier versions. To use the mode, contact O&M personnel.              |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Range**                                                                                                                   |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **read-only**: offline mode                                                                                                 |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **insert**: online mode                                                                                                     |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Default Value**                                                                                                           |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   +--------------------------+-----------------+--------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | logical_cluster_name     | No              | String                                                                                                       | **Definition**                                                                                                              |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | Logical cluster name.                                                                                                       |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Constraints**                                                                                                             |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Range**                                                                                                                   |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | In non-logical cluster mode, this parameter is left blank. In logical cluster mode, the default value is **elastic_group**. |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Default Value**                                                                                                           |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | elastic_group                                                                                                               |
   +--------------------------+-----------------+--------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | expand_with_existed_node | Yes             | Boolean                                                                                                      | **Definition**                                                                                                              |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | Whether to use added idle nodes for scale-out.                                                                              |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Constraints**                                                                                                             |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Range**                                                                                                                   |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **true**: Idle nodes are used for scale-out.                                                                                |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **false**: Idle nodes are not used for scale-out.                                                                           |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Default Value**                                                                                                           |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | false                                                                                                                       |
   +--------------------------+-----------------+--------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | auto_redistribute        | No              | Boolean                                                                                                      | **Definition**                                                                                                              |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | Whether to automatically start redistribution after scale-out. The default value is **true**.                               |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Constraints**                                                                                                             |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Range**                                                                                                                   |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Default Value**                                                                                                           |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | true                                                                                                                        |
   +--------------------------+-----------------+--------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | redis_conf               | No              | :ref:`RedisConfReq <en-us_topic_0000002531893991__en-us_topic_0000002340937144_request_redisconfreq>` object | **Definition**                                                                                                              |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | Redistribution configuration information.                                                                                   |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Constraints**                                                                                                             |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Range**                                                                                                                   |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | **Default Value**                                                                                                           |
   |                          |                 |                                                                                                              |                                                                                                                             |
   |                          |                 |                                                                                                              | N/A                                                                                                                         |
   +--------------------------+-----------------+--------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000002531893991__en-us_topic_0000002340937144_request_scaleout:

.. table:: **Table 3** ScaleOut

   +-----------------+-----------------+-----------------+-------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                     |
   +=================+=================+=================+=================================================+
   | count           | Yes             | Integer         | **Definition**                                  |
   |                 |                 |                 |                                                 |
   |                 |                 |                 | Number of nodes to be added.                    |
   |                 |                 |                 |                                                 |
   |                 |                 |                 | **Range**                                       |
   |                 |                 |                 |                                                 |
   |                 |                 |                 | Greater than or equal to **3**                  |
   +-----------------+-----------------+-----------------+-------------------------------------------------+
   | subnet_id       | No              | String          | **Definition**                                  |
   |                 |                 |                 |                                                 |
   |                 |                 |                 | Subnet ID.                                      |
   |                 |                 |                 |                                                 |
   |                 |                 |                 | **Range**                                       |
   |                 |                 |                 |                                                 |
   |                 |                 |                 | The value is a valid subnet ID in the same VPC. |
   +-----------------+-----------------+-----------------+-------------------------------------------------+

.. _en-us_topic_0000002531893991__en-us_topic_0000002340937144_request_redisconfreq:

.. table:: **Table 4** RedisConfReq

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                 |
   +=================+=================+=================+=============================================================================================================================================================+
   | redis_mode      | Yes             | String          | **Definition**                                                                                                                                              |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | Redistribution mode. The impact on services varies depending on the mode. You are advised to contact O&M personnel for evaluation before changing the mode. |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | **Constraints**                                                                                                                                             |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | The value must be a valid DWS cluster ID.                                                                                                                   |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | **Range**                                                                                                                                                   |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | **offline**                                                                                                                                                 |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | **online**                                                                                                                                                  |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | **Default Value**                                                                                                                                           |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | **offline**                                                                                                                                                 |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | parallel_jobs   | Yes             | Integer         | **Definition**                                                                                                                                              |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | Number of concurrent jobs. The default value is **4**.                                                                                                      |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | **Constraints**                                                                                                                                             |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | N/A                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | **Range**                                                                                                                                                   |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | **1** to **200**                                                                                                                                            |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | **Default Value**                                                                                                                                           |
   |                 |                 |                 |                                                                                                                                                             |
   |                 |                 |                 | 4                                                                                                                                                           |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response Parameters
-------------------

**Status code: 200**

Request for adding an idle node submitted.

None

Example Requests
----------------

Scale out a cluster with idle nodes.

.. code-block:: text

   POST https://{Endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/resize-with-existed-nodes

   {
     "scale_out" : {
       "count" : 3
     },
     "expand_with_existed_node" : true,
     "auto_redistribute" : true,
     "redis_conf" : {
       "redis_mode" : "offLine",
       "parallel_jobs" : 4
     }
   }

Example Responses
-----------------

None

Status Codes
------------

=========== ==========================================
Status Code Description
=========== ==========================================
200         Request for adding an idle node submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ==========================================
