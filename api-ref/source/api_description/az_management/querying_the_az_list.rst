:original_name: ListAvailabilityZones.html

.. _ListAvailabilityZones:

Querying the AZ List
====================

Function
--------

This API is used to query the ID of the AZ, which you will need to create an instance.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

GET /v1.0/{project_id}/availability-zones

.. table:: **Table 1** Path Parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                       |
   +=================+=================+=================+===================================================================================+
   | project_id      | Yes             | String          | **Definition**                                                                    |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | Project ID. To obtain the value, see :ref:`Obtaining a Project ID <dws_02_0011>`. |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Constraints**                                                                   |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | **Range**                                                                         |
   |                 |                 |                 |                                                                                   |
   |                 |                 |                 | N/A                                                                               |
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

.. table:: **Table 2** Response body parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | Parameter             | Type                                                                                                                            | Description            |
   +=======================+=================================================================================================================================+========================+
   | availability_zones    | Array of :ref:`AvailabilityZone <en-us_topic_0000002500174076__en-us_topic_0000002341096860_response_availabilityzone>` objects | **Definition**         |
   |                       |                                                                                                                                 |                        |
   |                       |                                                                                                                                 | AZ list.               |
   |                       |                                                                                                                                 |                        |
   |                       |                                                                                                                                 | **Range**              |
   |                       |                                                                                                                                 |                        |
   |                       |                                                                                                                                 | Non-empty object list. |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | count                 | Integer                                                                                                                         | **Definition**         |
   |                       |                                                                                                                                 |                        |
   |                       |                                                                                                                                 | Number of AZs.         |
   |                       |                                                                                                                                 |                        |
   |                       |                                                                                                                                 | **Range**              |
   |                       |                                                                                                                                 |                        |
   |                       |                                                                                                                                 | N/A                    |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------+------------------------+

.. _en-us_topic_0000002500174076__en-us_topic_0000002341096860_response_availabilityzone:

.. table:: **Table 3** AvailabilityZone

   +-----------------------+-----------------------+------------------------------------+
   | Parameter             | Type                  | Description                        |
   +=======================+=======================+====================================+
   | code                  | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | AZ unique code.                    |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+
   | name                  | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | AZ name.                           |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+
   | status                | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | AZ status.                         |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | **available**                      |
   |                       |                       |                                    |
   |                       |                       | **unavailable**                    |
   +-----------------------+-----------------------+------------------------------------+
   | public_border_group   | String                | **Definition**                     |
   |                       |                       |                                    |
   |                       |                       | AZ group, for example, **center**. |
   |                       |                       |                                    |
   |                       |                       | **Range**                          |
   |                       |                       |                                    |
   |                       |                       | N/A                                |
   +-----------------------+-----------------------+------------------------------------+

Example Requests
----------------

Query AZs.

.. code-block:: text

   GET https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/availability-zones

Example Responses
-----------------

**Status code: 200**

AZ list queried.

.. code-block::

   {
     "availability_zones" : [ {
       "code" : "az1",
       "name" : "AZ1",
       "status" : "available",
       "public_border_group" : "center"
     } ],
     "count" : 1
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         AZ list queried.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
