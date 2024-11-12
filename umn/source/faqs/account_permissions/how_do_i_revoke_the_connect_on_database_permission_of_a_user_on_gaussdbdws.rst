:original_name: dws_03_0195.html

.. _dws_03_0195:

How Do I Revoke the CONNECT ON DATABASE Permission of a User on GaussDB(DWS)?
=============================================================================

Scenario
--------

In a service, the permission of user **u1** to connect to a database needs to be revoked. After the **REVOKE CONNECT ON DATABASE gaussdb FROM u1;** command is executed successfully, user **u1** can still connect to the database. This means the revocation does not take effect.

Cause Analysis
--------------

If you run the **REVOKE CONNECT ON DATABASE gaussdb from u1** command to revoke the permissions of user **u1**, the revocation does not take effect because the **CONNECT** permission of the database is granted to **PUBLIC**. Therefore, you need to specify **PUBLIC**.

-  GaussDB(DWS) provides an implicitly defined group **PUBLIC** that contains all roles. By default, all new users and roles have the permissions of **PUBLIC**. To revoke permissions of **PUBLIC** from a user or role, or re-grant these permissions to them, add the **PUBLIC** keyword in the **REVOKE** or **GRANT** statement.
-  GaussDB(DWS) grants the permissions for objects of certain types to **PUBLIC**. By default, permissions on tables, columns, sequences, foreign data sources, foreign servers, schemas, and tablespaces are not granted to **PUBLIC**, but the following permissions are granted to **PUBLIC**;

   -  **CONNECT** permission of a database
   -  **CREATE TEMP TABLE** permission of a database
   -  **EXECUTE** permission of a function
   -  **USAGE** permission for languages and data types (including domains)

-  An object owner can revoke the default permissions granted to **PUBLIC** and grant permissions to other users as needed.

Example Operations
------------------

Run the following command to revoke the permission for user **u1** to access database **gaussdb**:

#. Connect to the GaussDB(DWS) database **gaussdb**.

   ::

      gsql -d gaussdb -p 8000 -h 192.168.x.xx -U dbadmin -W password -r
      gaussdb=>

#. Create user **u1**.

   ::

      gaussdb=> CREATE USER u1 IDENTIFIED BY 'xxxxxxxx';

#. Verify that user **u1** can access GaussDB.

   ::

      gsql -d gaussdb -p 8000 -h 192.168.x.xx -U u1 -W password -r
      gaussdb=>

#. Connect to database **gaussdb** as administrator **dbadmin** and run the REVOKE command to revoke the **connect on database** permission of user **public**.

   ::

      gsql -d gaussdb -h 192.168.x.xx -U dbadmin -p 8000 -r

   ::

      gaussdb=> REVOKE CONNECT ON DATABASE gaussdb FROM public;
      REVOKE

#. Verify the result. Use **u1** to connect to the database. If the following information is displayed, the **connect on database** permission of user **u1** has been revoked successfully:

   ::

      gsql -d gaussdb -p 8000 -h 192.168.x.xx -U u1 -W password -r
      gsql: FATAL:  permission denied for database "gaussdb"
      DETAIL:  User does not have CONNECT privilege.
