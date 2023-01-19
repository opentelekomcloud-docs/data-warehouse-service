:original_name: dws_04_0055.html

.. _dws_04_0055:

System Administrator
====================

A system administrator is an account with the **SYSADMIN** permission. After a cluster is installed, a system administrator has the permissions of all object owners by default.

The user **dbadmin** created upon GaussDB(DWS) startup is a system administrator.

To create a database administrator, connect to the database as an administrator and run the **CREATE USER** or **ALTER** statement with **SYSADMIN** specified.

::

   CREATE USER sysadmin WITH SYSADMIN password 'password';

Alternatively, you can run the following statement:

::

   ALTER USER joe SYSADMIN;

To run the **ALTER USER** statement, the user must exist.
