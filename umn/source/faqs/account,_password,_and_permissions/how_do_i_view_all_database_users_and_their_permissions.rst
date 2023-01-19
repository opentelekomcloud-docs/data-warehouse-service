:original_name: dws_03_0095.html

.. _dws_03_0095:

How Do I View All Database Users and Their Permissions?
=======================================================

-  To view the user list, query the **PG_USER** view.

   ::


      SELECT * FROM pg_user;

-  To view user attributes, query the **PG_AUTHID** system catalog.

   ::


      SELECT * FROM pg_authid;

-  Query the permissions of user **joe**.

   ::

      SELECT * FROM pg_authid where rolname = 'joe';
