:original_name: dws_01_0150.html

.. _dws_01_0150:

RBAC Syntax of RBAC Policies
============================

Policy Structure
----------------

An RBAC policy consists of a Version, a Statement, and Depends.


.. figure:: /_static/images/en-us_image_0000001180440441.jpg
   :alt: **Figure 1** RBAC policy structure

   **Figure 1** RBAC policy structure

Policy Syntax
-------------

When selecting a policy for a user group, click |image1| below the policy to view the details of the policy. The **DWS Administrator** policy is used as an example to describe the syntax of RBAC policies.


.. figure:: /_static/images/en-us_image_0000001180440439.png
   :alt: **Figure 2** Syntax of RBAC Policies

   **Figure 2** Syntax of RBAC Policies

.. code-block::

   {
           "Version": "1.0",
           "Statement": [
                   {
                           "Effect": "Allow",
                           "Action": [
                                   "dws:dws:*"
                           ]
                   }
           ],
           "Depends": [
                   {
                           "catalog": "BASE",
                           "display_name": "Server Administrator"
                   },
                   {
                           "catalog": "BASE",
                           "display_name": "Tenant Guest"
                   }
           ]
   }

+-----------------+-----------------+--------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| Parameter       |                 | Meaning                                                      | Value                                                                                            |
+=================+=================+==============================================================+==================================================================================================+
| Version         |                 | Policy version                                               | The value is fixed to **1.0**.                                                                   |
+-----------------+-----------------+--------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| Statement       | Action          | Operations to be performed on GaussDB(DWS)                   | Format: *Service name:Resource type:Operation*.                                                  |
|                 |                 |                                                              |                                                                                                  |
|                 |                 |                                                              | **dws:dws:\***: Permissions for performing all operations on all resource types in GaussDB(DWS). |
+-----------------+-----------------+--------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
|                 | Effect          | Whether the operation defined in an action is allowed        | -  Allow                                                                                         |
|                 |                 |                                                              | -  Deny                                                                                          |
+-----------------+-----------------+--------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| Depends         | catalog         | Name of the service to which dependencies of a policy belong | Service name                                                                                     |
|                 |                 |                                                              |                                                                                                  |
|                 |                 |                                                              | Example: **BASE**                                                                                |
+-----------------+-----------------+--------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
|                 | display_name    | Name of a dependent policy                                   | Policy name                                                                                      |
|                 |                 |                                                              |                                                                                                  |
|                 |                 |                                                              | Example: **Server Administrator**                                                                |
+-----------------+-----------------+--------------------------------------------------------------+--------------------------------------------------------------------------------------------------+

.. note::

   When using RBAC for authentication, pay attention to the **Depends** parameter and grant other dependent permissions at the same time.

   For example, the **DWS Administrator** permission depends on the **Server Administrator** and **Tenant Guest** permissions. When granting the **DWS Administrator** permission to users, you also need to grant the two dependent permissions to the users.

.. |image1| image:: /_static/images/en-us_image_0000001134401070.png
