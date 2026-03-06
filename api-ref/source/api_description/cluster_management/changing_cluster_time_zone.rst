:original_name: ModifyClusterTimezone.html

.. _ModifyClusterTimezone:

Changing Cluster Time Zone
==========================

Function
--------

This API is used to change the time zone of a cluster. This operation will change the time zone of the OS as well as the database.

**Constraints**

To change the time zone of a cluster, you need to ensure that the guestAgent plug-in installed in the cluster is 8.3.0.202 or later.

Calling Method
--------------

For details, see :ref:`Calling APIs <dws_02_0062>`.

URI
---

POST /v2/{project_id}/clusters/{cluster_id}/timezone

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

   +------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter        | Mandatory       | Type            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
   +==================+=================+=================+============================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | cluster_timezone | No              | String          | **Definition**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   |                  |                 |                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                  |                 |                 | Time zone. Example value: **UTC**.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                  |                 |                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                  |                 |                 | **Constraints**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                  |                 |                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                  |                 |                 | N/A                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   |                  |                 |                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                  |                 |                 | **Range** ^(Etc/GMT+11|US/Hawaii|Etc/GMT+9|US/Alaska|US/Pacific|Etc/GMT+8|Canada/Mountain|US/Arizona|Canada/Saskatchewan|Etc/GMT+6|US/Central|EST|America/Bogota|Etc/GMT+5|Canada/Atlantic|America/Cuiaba|America/Buenos_Aires|Etc/GMT+3|Canada/Newfoundland|America/Santiago|Etc/GMT+2|Atlantic/Cape_Verde|Europe/London|Africa/Monrovia|UTC|Europe/Belgrade|CET|MET|Europe/Amsterdam|EET|Europe/Athens|Asia/Amman|Asia/Beirut|Europe/Minsk|Africa/Nairobi|Europe/Moscow|Etc/GMT-4|Asia/Tbilisi|Asia/Kabul|Etc/GMT-5|Asia/Calcutta|Etc/GMT-6|Etc/GMT-7|PRC|Asia/Shanghai|Etc/GMT-8|Australia/Perth|Asia/Seoul|Asia/Tokyo|Australia/Darwin|Australia/Adelaide|Australia/Sydney|Australia/Brisbane|Etc/GMT-11|Pacific/Auckland|Etc/GMT-12)$ |
   |                  |                 |                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                  |                 |                 | **Default Value**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
   |                  |                 |                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                  |                 |                 | N/A                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   +------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response Parameters
-------------------

**Status code: 200**

.. table:: **Table 3** Response body parameters

   +-----------------------+-----------------------+--------------------------------------------------------+
   | Parameter             | Type                  | Description                                            |
   +=======================+=======================+========================================================+
   | error_code            | String                | **Definition**                                         |
   |                       |                       |                                                        |
   |                       |                       | Error code.                                            |
   |                       |                       |                                                        |
   |                       |                       | **Range**                                              |
   |                       |                       |                                                        |
   |                       |                       | N/A                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------+
   | error_msg             | String                | **Definition**                                         |
   |                       |                       |                                                        |
   |                       |                       | Error message.                                         |
   |                       |                       |                                                        |
   |                       |                       | **Range**                                              |
   |                       |                       |                                                        |
   |                       |                       | N/A                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------+
   | job_id                | String                | **Definition**                                         |
   |                       |                       |                                                        |
   |                       |                       | Task ID, which can be used to query the task progress. |
   |                       |                       |                                                        |
   |                       |                       | **Range**                                              |
   |                       |                       |                                                        |
   |                       |                       | N/A                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------+

Example Requests
----------------

Change the time zone of a cluster to UTC.

.. code-block:: text

   POST https://{endpoint}/v2/89cd04f168b84af6be287f71730fdb4b/clusters/97cbaab3-939e-4dbc-9187-0fe240f2b9fd/timezone

   {
     "cluster_timezone" : "UTC"
   }

Example Responses
-----------------

**Status code: 200**

Request submitted.

.. code-block::

   {
     "error_code" : null,
     "error_msg" : null,
     "job_id" : "2c9080d08cc99d28018ccd139e942498"
   }

Status Codes
------------

=========== =====================================
Status Code Description
=========== =====================================
200         Request submitted.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal server error.
503         Service unavailable.
=========== =====================================
