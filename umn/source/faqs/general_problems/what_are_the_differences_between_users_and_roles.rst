:original_name: dws_03_0067.html

.. _dws_03_0067:

What Are the Differences Between Users and Roles?
=================================================

Users and roles are shared within the entire cluster, but their data is not shared. That is, a user can connect to any database, but after the connection is successful, any user can access only the database declared in the connection request.

-  A role is a set of permissions. Generally, roles are used to sort permissions. Users are used to manage permissions and perform operations.
-  A role can inherit permissions from other roles. All users in a user group automatically inherit the operation permissions of the role of the group.
-  In a database, the permissions of users come from roles.
-  A user group is a group of users who have the same permission.
-  A user can be regarded as a role with the login permission.
-  A role can be regarded as a user without the login permission.

The permissions provided by Gauss(DWS) include the O&M permissions for components on the management plane. You can assign different permissions to users as needed. The management plane uses roles for better permissions management. You can select specified permissions and assign them to roles in a unified manner. In this way, permissions can be viewed and managed in a centralized manner.

The following figure shows the relationships between permissions, roles, and users in unified permissions management.

|image1|

GaussDB(DWS) provides various permissions. Select and assign permissions to different users based on service scenarios. A role can be assigned one or more permissions.

After a role is granted to a user through **GRANT**, the user will have all the permissions of the role. It is recommended that roles be used to efficiently grant permissions. A user has permissions only for their own tables, but does not have permissions for other users' tables in their schemas.

-  Role A is assigned operation permissions A and B. After role A is allocated to user A and user B, user A and user B can obtain operation permissions A and B.

-  Role B is assigned operation permission C. After role B is allocated to user C, user C can obtain operation permissions C.

-  Role C is assigned operation permissions D and E. After role C is allocated to user C, user C can obtain operation permissions D and F.

.. |image1| image:: /_static/images/en-us_image_0000001199655548.png
