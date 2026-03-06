:original_name: ChangeSecurityGroup.html

.. _ChangeSecurityGroup:

Modifying the Security Group of a Cluster
=========================================

Function
--------

This API is used to modify the security group of a cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

PUT /v1/{project_id}/clusters/{cluster_id}/security-group

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

   +-----------------+-----------------+------------------+--------------------------+
   | Parameter       | Mandatory       | Type             | Description              |
   +=================+=================+==================+==========================+
   | security_groups | Yes             | Array of strings | **Definition**           |
   |                 |                 |                  |                          |
   |                 |                 |                  | Security group ID array. |
   |                 |                 |                  |                          |
   |                 |                 |                  | **Constraints**          |
   |                 |                 |                  |                          |
   |                 |                 |                  | N/A                      |
   |                 |                 |                  |                          |
   |                 |                 |                  | **Range**                |
   |                 |                 |                  |                          |
   |                 |                 |                  | N/A                      |
   |                 |                 |                  |                          |
   |                 |                 |                  | **Default Value**        |
   |                 |                 |                  |                          |
   |                 |                 |                  | N/A                      |
   +-----------------+-----------------+------------------+--------------------------+

Response Parameters
-------------------

**Status code: 200**

Cluster security group modified.

None

Example Requests
----------------

Request example for modifying a cluster security group.

.. code-block:: text

   PUT https://{Endpoint}/v1/05f2cff45100d5112f4bc00b794ea08a/clusters/58031bb5-7c1a-4449-a95f-3fbb6a91dfda/security-group

   {
     "security_groups" : [ "b3c812cb-2d90-4d89-a3df-c5480d915e2e", "0fbe2c34-8123-42b5-955f-242229e9318e" ]
   }

Example Responses
-----------------

None

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Cluster security group modified.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
500         Internal server error.
503         Service unavailable.
=========== =====================================
