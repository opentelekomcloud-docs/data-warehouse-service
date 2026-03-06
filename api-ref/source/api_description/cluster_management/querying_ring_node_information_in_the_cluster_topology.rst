:original_name: ListTopoRings.html

.. _ListTopoRings:

Querying Ring Node Information in the Cluster Topology
======================================================

Function
--------

This API is used to query ring node information in the cluster topology.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v2/{project_id}/clusters/{cluster_id}/topo/rings

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
   |                 |                 |                 | Greater than 0                                          |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | **Default Value**                                       |
   |                 |                 |                 |                                                         |
   |                 |                 |                 | 10                                                      |
   +-----------------+-----------------+-----------------+---------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------------------+
   | Parameter             | Type                                                                                                                    | Description                 |
   +=======================+=========================================================================================================================+=============================+
   | cluster_rings         | Array of :ref:`TopoRingInfo <en-us_topic_0000002531893875__en-us_topic_0000002341096896_response_toporinginfo>` objects | **Definition**              |
   |                       |                                                                                                                         |                             |
   |                       |                                                                                                                         | Cluster topology ring list. |
   |                       |                                                                                                                         |                             |
   |                       |                                                                                                                         | **Range**                   |
   |                       |                                                                                                                         |                             |
   |                       |                                                                                                                         | N/A                         |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------------------+
   | count                 | Integer                                                                                                                 | **Definition**              |
   |                       |                                                                                                                         |                             |
   |                       |                                                                                                                         | Number of cluster rings.    |
   |                       |                                                                                                                         |                             |
   |                       |                                                                                                                         | **Range**                   |
   |                       |                                                                                                                         |                             |
   |                       |                                                                                                                         | N/A                         |
   +-----------------------+-------------------------------------------------------------------------------------------------------------------------+-----------------------------+

.. _en-us_topic_0000002531893875__en-us_topic_0000002341096896_response_toporinginfo:

.. table:: **Table 4** TopoRingInfo

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | Parameter             | Type                                                                                                                            | Description            |
   +=======================+=================================================================================================================================+========================+
   | instance_info_lists   | Array of :ref:`TopoInstanceInfo <en-us_topic_0000002531893875__en-us_topic_0000002341096896_response_topoinstanceinfo>` objects | **Definition**         |
   |                       |                                                                                                                                 |                        |
   |                       |                                                                                                                                 | Cluster instance list. |
   |                       |                                                                                                                                 |                        |
   |                       |                                                                                                                                 | **Range**              |
   |                       |                                                                                                                                 |                        |
   |                       |                                                                                                                                 | N/A                    |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------+------------------------+

.. _en-us_topic_0000002531893875__en-us_topic_0000002341096896_response_topoinstanceinfo:

.. table:: **Table 5** TopoInstanceInfo

   +-----------------------+-----------------------+------------------------------------+
   | Parameter             | Type                  | Description                        |
   +=======================+=======================+====================================+
   | id                    | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | Instance ID.                       |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+
   | name                  | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | Instance name.                     |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+
   | manage_ip             | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | Instance management IP address.    |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+
   | traffic_ip            | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | Service IP address.                |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+
   | internal_ip           | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | Internal communication IP address. |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+
   | internal_mgnt_ip      | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | Internal management IP address.    |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+
   | eip                   | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | Public IP address information.     |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+
   | elb                   | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | ELB address.                       |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+
   | status                | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | Instance status.                   |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+
   | az_code               | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | AZ code.                           |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+

Example Requests
----------------

Query ring node information in the cluster topology.

.. code-block:: text

   GET https://{Endpoint}/v2/9b06d044ea4f49f1a58b2bed2b0084bd/clusters/9b7ff56b-47b3-4d00-a1fd-4c023d34404b/logical-clusters/rings

Example Responses
-----------------

**Status code: 200**

Ring node information of the cluster topology queried.

.. code-block::

   {
     "cluster_rings" : [ {
       "instance_info_lists" : [ {
         "id" : "a57e49db-c04b-45c7-9863-f7b6f3eed1b8",
         "name" : "ty-default--BGy6PUIN-K-dws-cn-cn-1-1",
         "manage_ip" : "172.16.26.233",
         "traffic_ip" : "192.168.0.217",
         "internal_ip" : "172.16.66.153",
         "internal_mgnt_ip" : null,
         "eip" : null,
         "elb" : null,
         "status" : 200,
         "az_code" : "eu-de-01c"
       }, {
         "id" : "3a37f794-be37-42d1-a299-a3eb94888ccb",
         "name" : "ty-default--BGy6PUIN-K-dws-cn-cn-2-1",
         "manage_ip" : "172.16.34.21",
         "traffic_ip" : "192.168.0.80",
         "internal_ip" : "172.16.65.89",
         "internal_mgnt_ip" : null,
         "eip" : null,
         "elb" : null,
         "status" : 200,
         "az_code" : "eu-de-01c"
       }, {
         "id" : "8763cbf1-5851-44a5-9e71-cbae35201f27",
         "name" : "ty-default--BGy6PUIN-K-dws-dn-1-1",
         "manage_ip" : "172.16.9.16",
         "traffic_ip" : "192.168.0.88",
         "internal_ip" : "172.16.67.64",
         "internal_mgnt_ip" : null,
         "eip" : null,
         "elb" : null,
         "status" : 200,
         "az_code" : "eu-de-01c"
       } ]
     } ],
     "count" : 1
   }

Status Codes
------------

=========== ======================================================
Status Code Description
=========== ======================================================
200         Ring node information of the cluster topology queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ======================================================
