:original_name: DWS_DS_024.html

.. _DWS_DS_024:

Users/Roles
===========

Creating a User/Role
--------------------

A database is used by many users, and the users are grouped for management convenience. A database role represents a database user or a group of database users.

Users and roles have similar concepts in databases. In practice, you are advised to use a role to manage permissions rather than to access databases.

-  **Users**: They are set of database users. These users are different from operating system users. These users can assign permissions to other users to access database objects.
-  **Role**: This can be considered as a user or group based on the usage. Roles are at cluster level, and hence applicable to all databases in the cluster. A role is a cluster-level definition and applies to all databases in a cluster.

Dropping a User/Role
--------------------

Right-click the selected user/role and select **Drop User/Role**.

|image1|

Viewing/Editing User/Role Properties
------------------------------------

Right-click the selected user/role and select **Properties**.

Data Studio displays the properties (General, Privilege, and Membership) of the selected user/role in different tabs. Editing of properties can be performed. OID is a non-editable field.

|image2|

Viewing the User/Role DDL
-------------------------

Right-click the selected user/role and select **Show DDL**.

The user/role DDL is displayed in a new **SQL Terminal** tab. You must refresh the **Object Browser** to view the latest DDL.

.. |image1| image:: /_static/images/en-us_image_0000001813599128.png
.. |image2| image:: /_static/images/en-us_image_0000001860319045.png
