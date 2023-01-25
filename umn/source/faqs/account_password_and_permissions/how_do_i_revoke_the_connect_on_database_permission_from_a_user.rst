:original_name: dws_03_0195.html

.. _dws_03_0195:

How Do I Revoke the CONNECT ON DATABASE Permission from a User?
===============================================================

GaussDB(DWS) provides an implicitly defined group **public** that contains all roles. By default, all new users and roles have the permissions of **public**. To revoke permissions of **public** from a user or role, or re-grant these permissions to them, add the **public** keyword in the **REVOKE** or **GRANT** statement.

GaussDB(DWS) grants the permissions for objects of certain types to **public**. By default, permissions on tables, columns, sequences, foreign data sources, foreign servers, schemas, and tablespaces are not granted to **PUBLIC**, but the following permissions are granted to **PUBLIC**: **CONNECT** and **CREATE TEMP TABLE** permissions on databases, **EXECUTE** permission on functions, and **USAGE** permission on languages and data types (including domains). An object owner can revoke the default permissions granted to **public** and grant permissions to other users. For security purposes, create an object and set its permissions in the same transaction so that other users do not have time windows to use the object. In addition, you can run the **ALTER DEFAULT PRIVILEGES** statement to modify the default permissions.

The following shows an example of revoking the **CONNECT ON DATABASE** permission from a user.

#. Connect to the default database **gaussdb** of a GaussDB(DWS) cluster.

   .. code-block::

      gsql -d gaussdb -h 192.168.0.89 -U dbadmin -p 8000 -r

   If the following information is displayed after you enter the password as prompted, the connection is successful.

   ::

      gaussdb=>

#. Create user **u1**.

   .. code-block::

      CREATE USER u1 IDENTIFIED BY 'password';
      CREATE USER

#. Check whether access of **u1** is normal.

   .. code-block::

      gsql -d gaussdb -h 192.168.0.89 -U u1 -p 8000 -W password -r
      gsql ((GaussDB 8.1.0 build be03b9a0) compiled at 2021-03-12 14:18:02 commit 1237 last mr 2001 release)
      SSL connection (protocol: TLSv1.3, cipher: TLS_AES_128_GCM_SHA256, bits: 128)
      Type "help" for help.

#. Revoke the **CONNECT ON DATABASE** permission from **public**.

   .. code-block::

      gsql -d gaussdb -h 192.168.0.89 -U dbadmin -p 8000 -r
      gaussdb=>

      REVOKE CONNECT ON database gaussdb FROM public;
      REVOKE

   .. note::

      **revoke connect on database postgres from u1** cannot be used directly because the **CONNECT** permission is granted to **public**.

#. Verify the result. If the following information is displayed, the **CONNECT ON DATABASE** permission has been revoked from user **u1**.

   .. code-block::

      gsql -d gaussdb -h 192.168.0.89 -U u1 -p 8000
      gsql: FATAL:  permission denied for database "gaussdb"
      DETAIL:  User does not have CONNECT privilege.
