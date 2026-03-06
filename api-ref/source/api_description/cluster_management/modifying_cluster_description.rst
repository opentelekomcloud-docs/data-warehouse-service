:original_name: SaveClusterDescriptionInfo.html

.. _SaveClusterDescriptionInfo:

Modifying Cluster Description
=============================

Function
--------

This API is used to modify the cluster description.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v1/{project_id}/clusters/{cluster_id}/description

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

   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                           |
   +=================+=================+=================+=======================================================================================+
   | namespace       | No              | String          | **Definition**                                                                        |
   |                 |                 |                 |                                                                                       |
   |                 |                 |                 | Namespace.                                                                            |
   |                 |                 |                 |                                                                                       |
   |                 |                 |                 | **Constraints**                                                                       |
   |                 |                 |                 |                                                                                       |
   |                 |                 |                 | The value is fixed at **DWS**. If this parameter is left blank, the value is **DWS**. |
   |                 |                 |                 |                                                                                       |
   |                 |                 |                 | **Range**                                                                             |
   |                 |                 |                 |                                                                                       |
   |                 |                 |                 | DWS                                                                                   |
   |                 |                 |                 |                                                                                       |
   |                 |                 |                 | **Default Value**                                                                     |
   |                 |                 |                 |                                                                                       |
   |                 |                 |                 | DWS                                                                                   |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 3** Request body parameters

   +------------------+-----------------+-----------------+----------------------+
   | Parameter        | Mandatory       | Type            | Description          |
   +==================+=================+=================+======================+
   | description_info | Yes             | String          | **Definition**       |
   |                  |                 |                 |                      |
   |                  |                 |                 | Cluster description. |
   |                  |                 |                 |                      |
   |                  |                 |                 | **Range**            |
   |                  |                 |                 |                      |
   |                  |                 |                 | N/A                  |
   +------------------+-----------------+-----------------+----------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 4** Response body parameters

   +-----------------------+-----------------------+-----------------------+
   | Parameter             | Type                  | Description           |
   +=======================+=======================+=======================+
   | error_code            | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Error code.           |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+
   | error_msg             | String                | **Definition**        |
   |                       |                       |                       |
   |                       |                       | Error message.        |
   |                       |                       |                       |
   |                       |                       | **Range**             |
   |                       |                       |                       |
   |                       |                       | N/A                   |
   +-----------------------+-----------------------+-----------------------+

Example Requests
----------------

Modify the cluster description.

.. code-block:: text

   POST https://{Endpoint}/v1/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/description

   {
     "description_info" : "desc info"
   }

Example Responses
-----------------

**Status code: 200**

Cluster description modified.

.. code-block::

   {
     "error_code" : "DWS.0000",
     "error_msg" : null
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Cluster description modified.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
