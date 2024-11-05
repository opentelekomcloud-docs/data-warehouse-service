:original_name: dws_02_0054.html

.. _dws_02_0054:

Resetting a Password
====================

Function
--------

This API is used to reset the password of cluster administrator.

URI
---

.. code-block:: text

   POST /v1.0/{project_id}/clusters/{cluster_id}/reset-password

.. table:: **Table 1** URI parameters

   +------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type   | Description                                                                                                                                  |
   +============+===========+========+==============================================================================================================================================+
   | project_id | Yes       | String | Project ID. For details about how to obtain the ID, see :ref:`Obtaining a Project ID <dws_02_0011>`.                                         |
   +------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------+
   | cluster_id | Yes       | String | ID of the cluster whose password is to be reset. For details about how to obtain the ID, see :ref:`Obtaining the Cluster ID <dws_02_00068>`. |
   +------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request body parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                     |
   +=================+=================+=================+=================================================================================+
   | new_password    | Yes             | String          | New administrator password for logging in to a data warehouse cluster           |
   |                 |                 |                 |                                                                                 |
   |                 |                 |                 | A password must conform to the following rules:                                 |
   |                 |                 |                 |                                                                                 |
   |                 |                 |                 | -  Contains 12 to 32 characters.                                                |
   |                 |                 |                 | -  Cannot be the same as the username or the username written in reverse order. |
   |                 |                 |                 | -  Contains at least three types of the following:                              |
   |                 |                 |                 |                                                                                 |
   |                 |                 |                 |    -  Lowercase letters                                                         |
   |                 |                 |                 |    -  Uppercase letters                                                         |
   |                 |                 |                 |    -  Digits                                                                    |
   |                 |                 |                 |    -  Special characters:``~!?,.:;-_'"(){}[]/<>@#%^&*+|\=``                     |
   |                 |                 |                 |                                                                                 |
   |                 |                 |                 | -  Cannot be the same as previous passwords.                                    |
   |                 |                 |                 | -  Cannot be a weak password.                                                   |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------+

Example Request
---------------

.. code-block:: text

   POST https://{Endpoint}/v1.0/89cd04f168b84af6be287f71730fdb4b/clusters/4ca46bf1-5c61-48ff-b4f3-0ad4e5e3ba90/reset-password

   {
     "new_password" : "NewPassword!"
   }

Response Example
----------------

None

Status Code
-----------

=========== =====================================
Status Code Description
=========== =====================================
200         The password is reset.
400         Request error.
401         Authentication failed.
403         You do not have required permissions.
404         No resources found.
500         Internal service error.
503         Service unavailable.
=========== =====================================
