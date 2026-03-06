:original_name: DeleteClusterDns.html

.. _DeleteClusterDns:

Deleting a Cluster Domain Name
==============================

Function
--------

This API is used to delete the domain name of a specified cluster.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

DELETE /v1.0/{project_id}/clusters/{cluster_id}/dns

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

.. table:: **Table 2** Query Parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                   |
   +=================+=================+=================+===============================================================================+
   | type            | Yes             | String          | **Definition**                                                                |
   |                 |                 |                 |                                                                               |
   |                 |                 |                 | Domain name type. Currently, only public network domain names can be deleted. |
   |                 |                 |                 |                                                                               |
   |                 |                 |                 | **Constraints**                                                               |
   |                 |                 |                 |                                                                               |
   |                 |                 |                 | N/A                                                                           |
   |                 |                 |                 |                                                                               |
   |                 |                 |                 | **Range**                                                                     |
   |                 |                 |                 |                                                                               |
   |                 |                 |                 | **public**                                                                    |
   |                 |                 |                 |                                                                               |
   |                 |                 |                 | **Default Value**                                                             |
   |                 |                 |                 |                                                                               |
   |                 |                 |                 | N/A                                                                           |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------+

Request Parameters
------------------

None

Response Parameters
-------------------

**Status code: 200**

The cluster domain name is deleted successfully.

None

Example Requests
----------------

.. code-block:: text

   DELETE https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/b5c45780-1006-49e3-b2d5-b3229975bbc7/dns?type=public

Example Responses
-----------------

None

Status Codes
------------

=========== ================================================
Status Code Description
=========== ================================================
200         The cluster domain name is deleted successfully.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== ================================================
