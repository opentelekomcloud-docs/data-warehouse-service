:original_name: dws_04_0058.html

.. _dws_04_0058:

Roles
=====

A role is a set of permissions. After a role is granted to a user through **GRANT**, the user will have all the permissions of the role. It is recommended that roles be used to efficiently grant permissions. For example, you can create different roles of design, development, and maintenance personnel, grant the roles to users, and then grant specific data permissions required by different users. When permissions are granted or revoked at the role level, these changes take effect on all members of the role.

GaussDB(DWS) provides an implicitly defined group **PUBLIC** that contains all roles. By default, all new users and roles have the permissions of **PUBLIC**. For details about the default permissions of **PUBLIC**, see GRANT. To revoke permissions of **PUBLIC** from a user or role, or re-grant these permissions to them, add the **PUBLIC** keyword in the **REVOKE** or **GRANT** statement.

To view all roles, query the system catalog **PG_ROLES**.

::

   SELECT * FROM PG_ROLES;

Adding, Modifying, and Deleting Roles
-------------------------------------

In non-:ref:`separation-of-duty <dws_04_0056>` scenarios, a role can be created, modified, and deleted only by a system administrator or a user with the **CREATEROLE** attribute. In separation-of-duty scenarios, a role can be created, modified, and deleted only by a user with the **CREATEROLE** attribute.

-  To create a role, use **CREATE ROLE**.
-  To add or delete users in an existing role, use **ALTER ROLE**.
-  To delete a role, use **DROP ROLE**. **DROP ROLE** deletes only a role, rather than member users in the role.
