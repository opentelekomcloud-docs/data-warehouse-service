:original_name: dws_04_0057.html

.. _dws_04_0057:

User Management
===============

You can use **CREATE USER** and **ALTER USER** to create and manage database users.

-  In the non-:ref:`separation-of-permission <en-us_topic_0000001480501210>` mode, a GaussDB(DWS) user account can be created and deleted only by a system administrator or a security administrator with the **CREATEROLE** attribute.
-  In separation-of-permission mode, a user account can be created only by a security administrator.

Creating a User
---------------

The **CREATE USER** statement is used to create a GaussDB (DWS) user. After creating a user, you can use the user to connect to the database.

-  Create common user **u1** and assign the **CREATEDB** attribute to the user.

   ::

      CREATE USER u1 WITH CREATEDB PASSWORD '{Password}';

-  To create the system administrator **mydbadmin**, you need to specify the **SYSADMIN** parameter.

   ::

      CREATE USER mydbadmin sysadmin PASSWORD '{Password}';

-  View the created user in the :ref:`PG_USER <dws_04_0791>` view.

   ::

      SELECT * FROM pg_user;

-  To view user attributes, query the system catalog :ref:`PG_AUTHID <dws_04_0574>`.

   ::

      SELECT * FROM pg_authid;

Altering User Attributes
------------------------

The **ALTER USER** statement is used to alter user attributes, such as changing user passwords or permissions.

Example:

-  Rename user **u1** to **u2**.

   ::

      ALTER USER u1 RENAME TO u2;

-  Grant the **CREATEROLE** permission to user **u1**:

   ::

      ALTER USER u1 CREATEROLE;

-  For details about how to change the user password, see :ref:`Setting and Changing a Password <en-us_topic_0000001531101121__en-us_topic_0000001188482292_section1897910435417>`.

Locking a User
--------------

The **ACCOUNT LOCK** \| **ACCOUNT UNLOCK** parameter in the statement is used to lock or unlock a user. A locked user cannot log in to the system. If an account is stolen or illegally accessed, the administrator can manually lock the account. After the account is secured, the administrator can manually unlock the account.

Example:

-  To lock user **u1**, run the following command:

   ::

      ALTER USER u1 ACCOUNT LOCK;

-  To unlock user **u1**, run the following command:

   ::

      ALTER USER u1 ACCOUNT UNLOCK;

Deleting a User
---------------

The **DROP USER** statement is used to delete one or more GaussDB(DWS) users. An administrator can delete an account that is no longer used. Deleted users cannot be restored.

-  If multiple users are deleted at the same time, separate them with commas (,).
-  After a user is deleted successfully, all the permissions of the user are also deleted.
-  When an account to be deleted is in the active state, it is deleted after the session is disconnected.
-  When **CASCADE** is specified in the **DROP USER** statement, objects such as tables that depend on the user will be deleted. That is, the objects whose owner is the user are deleted, and the authorizations of other objects to the user are also deleted.

Example:

-  -- Delete user **u1**.

   ::

      DROP USER u1;

-  Delete account **u2** in a cascading manner.

   ::

      DROP USER u2 CASCADE;
