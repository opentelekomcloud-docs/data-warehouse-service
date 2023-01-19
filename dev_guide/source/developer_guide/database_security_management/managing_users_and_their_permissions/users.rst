:original_name: dws_04_0057.html

.. _dws_04_0057:

Users
=====

You can use **CREATE USER** and **ALTER USER** to create and manage database users, respectively. The database cluster has one or more named databases. Users and roles are shared within the entire cluster, but their data is not shared. That is, a user can connect to any database, but after the connection is successful, any user can access only the database declared in the connection request.

In non-:ref:`separation-of-duty <dws_04_0056>` scenarios, a GaussDB(DWS) user account can be created and deleted only by a system administrator or a security administrator with the **CREATEROLE** attribute. In separation-of-duty scenarios, a user account can be created only by a security administrator.

When a user logs in, GaussDB(DWS) authenticates the user. A user can own databases and database objects (such as tables), and grant permissions of these objects to other users and roles. In addition to system administrators, users with the **CREATEDB** attribute can create databases and grant permissions to these databases.

Adding, Modifying, and Deleting Users
-------------------------------------

-  To create a user, use the SQL CREATE USER statement.

   For example, create a user **joe** and set the **CREATEDB** attribute for the user.

   ::

      CREATE USER joe WITH CREATEDB PASSWORD 'password';

-  To create a system administrator, use the CREATE USER statement with the **SYSADMIN** parameter.

-  To delete an existing user, use the DROP USER statement.

-  To change a user account (for example, rename the user or change the password), use the ALTER USER statement.

-  To view a user list, query the :ref:`PG_USER <dws_04_0791>` view.

   ::

      SELECT * FROM pg_user;

-  To view user attributes, query the system catalog :ref:`PG_AUTHID <dws_04_0574>`.

   ::

      SELECT * FROM pg_authid;

Private Users
-------------

If multiple service departments use different database user accounts to perform service operations and a database maintenance department at the same level uses database administrator accounts to perform maintenance operations, service departments may require that database administrators, without specific authorization, can manage (**DROP**, **ALTER**, and **TRUNCATE**) their data but cannot access (**INSERT**, **DELETE**, **UPDATE**, **SELECT**, and **COPY**) the data. That is, the management permissions of database administrators for tables need to be isolated from their access permissions to improve the data security of common users.

In :ref:`Separation of Permissions <dws_04_0056>` mode, a database administrator does not have permissions for the tables in schemas of other users. In this case, database administrators have neither management permissions nor access permissions, which does not meet the requirements of the service departments mentioned above. Therefore, GaussDB(DWS) provides private users to solve the problem. That is, create private users with the **INDEPENDENT** attribute in non-separation-of-duties mode.

::

   CREATE USER user_independent WITH INDEPENDENT IDENTIFIED BY "password";

Database administrators can manage (**DROP**, **ALTER**, and **TRUNCATE**) objects of private users but cannot access (**INSERT**, **DELETE**, **SELECT**, **UPDATE**, **COPY**, **GRANT**, **REVOKE**, and **ALTER OWNER** the objects before being authorized.
