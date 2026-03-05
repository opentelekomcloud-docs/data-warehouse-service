:original_name: CheckClusterShrink.html

.. _CheckClusterShrink:

Performing a Scale-in Check
===========================

Function
--------

This API is used to perform a re-scale-in check. It can ensure that the requirements are met before and after the scale-in.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1/{project_id}/clusters/{cluster_id}/shrink-check

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

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                       |
   +=================+=================+=================+===================================================================================+
   | check_item      | Yes             | String          | **Definition**                                                                    |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | Check item. The value can only be one of the following:                           |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Constraints**                                                                   |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Range**                                                                         |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **guc**: Check whether the current GUC parameters meet the scale-in requirements. |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **schema**: Check whether there are tables that affect scale-in in all schemas.   |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **disk**: Check whether the disk capacity meets the requirements after scale-in.  |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Default Value**                                                                 |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------+
   | shrink_count    | Yes             | Integer         | **Definition**                                                                    |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | Number of nodes to be deleted.                                                    |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Constraints**                                                                   |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Range**                                                                         |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | Minimum value: **3**. Maximum value: *Total number of nodes* - **3**.             |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Default Value**                                                                 |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Type                  | Description                                                                                                                                                                            |
   +=======================+=======================+========================================================================================================================================================================================+
   | is_passed             | Boolean               | **Definition**                                                                                                                                                                         |
   |                       |                       |                                                                                                                                                                                        |
   |                       |                       | Whether the check is passed. If the check fails, you need to adjust the number of nodes to be removed and try again, or the current cluster does not meet the pre-scale-in conditions. |
   |                       |                       |                                                                                                                                                                                        |
   |                       |                       | **Range**                                                                                                                                                                              |
   |                       |                       |                                                                                                                                                                                        |
   |                       |                       | N/A                                                                                                                                                                                    |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Requests
----------------

-  Scale in three nodes and set the check item to **guc**.

   .. code-block:: text

      POST https://{Endpoint} /v1/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/shrink-check?check_item=guc&shrink_count=3

-  Scale in three nodes and set the check item to **schema**.

   .. code-block:: text

      POST https://{Endpoint} /v1/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/shrink-check?check_item=schema&shrink_count=3

-  Scale in three nodes and set the check item to **disk**.

   .. code-block:: text

      POST https://{Endpoint} /v1/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/shrink-check?check_item=disk&shrink_count=3

Example Responses
-----------------

**Status code: 200**

The pre-scale-in check request is submitted.

.. code-block::

   {
     "is_passed" : true
   }

Status Codes
------------

=========== ============================================
Status Code Description
=========== ============================================
200         The pre-scale-in check request is submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ============================================
