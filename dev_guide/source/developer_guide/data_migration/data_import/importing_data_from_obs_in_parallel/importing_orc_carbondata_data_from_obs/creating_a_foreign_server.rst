:original_name: dws_04_0244.html

.. _dws_04_0244:

Creating a Foreign Server
=========================

This section describes how to create a foreign server that is used to define the information about OBS servers and is invoked by foreign tables. For details about the syntax for creating foreign servers, see CREATE SERVER.

.. _en-us_topic_0000001100335204__en-us_topic_0000001099130938_en-us_topic_0102810708_section590417587243:

(Optional) Creating a User and a Database and Granting the User Foreign Table Permissions
-----------------------------------------------------------------------------------------

Common users do not have permissions to create foreign servers and tables. If you want to use a common user to create foreign servers and tables in a customized database, perform the following steps to create a user and a database, and grant the user foreign table permissions.

In the following example, a common user **dbuser** and a database **mydatabase** are created. Then, an administrator is used to grant foreign table permissions to user **dbuser**.

#. Connect to the default database **gaussdb** as a database administrator through the database client tool provided by GaussDB(DWS).

   For example, use the gsql client to connect to the database by running the following command:

   ::

      gsql -d gaussdb -h 192.168.2.30 -U dbadmin -p 8000 -W password -r

#. Create a common user and use it to create a database.

   Create a user named **dbuser** that has the permission to create databases.

   ::

      CREATE USER dbuser WITH CREATEDB PASSWORD 'password';

   Switch to the created user.

   ::

      SET ROLE dbuser PASSWORD 'password';

   Run the following command to create the database demo:

   ::

      CREATE DATABASE mydatabase;

   Query the database.

   ::

      SELECT * FROM pg_database;

   The database is successfully created if the returned result contains information about **mydatabase**.

   ::

      datname   | datdba | encoding | datcollate | datctype | datistemplate | datallowconn | datconnlimit | datlastsysoid | datfrozenxid | dattablespace | datcompatibility |                       datacl

      ------------+--------+----------+------------+----------+---------------+--------------+--------------+---------------+--------------+---------------+------------------+--------------------------------------
      --------------
       template1  |     10 |        0 | C          | C        | t             | t            |           -1 |         14146 |         1351 |          1663 | ORA              | {=c/Ruby,Ruby=CTc/Ruby}
       template0  |     10 |        0 | C          | C        | t             | f            |           -1 |         14146 |         1350 |          1663 | ORA              | {=c/Ruby,Ruby=CTc/Ruby}
       gaussdb   |     10 |        0 | C          | C        | f             | t            |           -1 |         14146 |         1352 |          1663 | ORA              | {=Tc/Ruby,Ruby=CTc/Ruby,chaojun=C/Ruby,hu
      obinru=C/Ruby}
       mydatabase |  17000 |        0 | C          | C        | f             | t            |           -1 |         14146 |         1351 |          1663 | ORA              |
      (4 rows)

#. Grant the permissions for creating foreign servers and using foreign tables to a common user as the administrator.

   Connect to the new database as a database administrator through the database client tool provided by GaussDB(DWS).

   You can use the gsql client to run the following command to switch to an administrator user and connect to the new database:

   ::

      \c mydatabase dbadmin;

   Enter the password of the system administrator as prompted.

   .. note::

      Note that you must use the administrator account to connect to the database where a foreign server is to be created and foreign tables are used; and then grant permissions to the common user.

   By default, only system administrators can create foreign servers. Common users can create foreign servers only after being authorized. Run the following command to grant the permission:

   ::

      GRANT ALL ON SCHEMA public TO dbuser;
      GRANT ALL ON FOREIGN DATA WRAPPER dfs_fdw TO dbuser;

   where *fdw_name* can be **hdfs_fdw** or **dfs_fdw**, and **dbuser** is the name of the user who creates SERVER.

   Run the following command to grant the user the permission to use foreign tables:

   ::

      ALTER USER dbuser USEFT;

   Query for the user.

   ::

      SELECT r.rolname, r.rolsuper, r.rolinherit,
        r.rolcreaterole, r.rolcreatedb, r.rolcanlogin,
        r.rolconnlimit, r.rolvalidbegin, r.rolvaliduntil,
        ARRAY(SELECT b.rolname
              FROM pg_catalog.pg_auth_members m
              JOIN pg_catalog.pg_roles b ON (m.roleid = b.oid)
              WHERE m.member = r.oid) as memberof
      , r.rolreplication
      , r.rolauditadmin
      , r.rolsystemadmin
      , r.roluseft
      FROM pg_catalog.pg_roles r
      ORDER BY 1;

   The authorization is successful if the **dbuser** information in the returned result contains the UseFT permission.

   ::

      rolname  | rolsuper | rolinherit | rolcreaterole | rolcreatedb | rolcanlogin | rolconnlimit | rolvalidbegin | rolvaliduntil | memberof | rolreplication | rolauditadmin | rolsystemadmin | roluseft
      -----------+----------+------------+---------------+-------------+-------------+--------------+---------------+---------------+----------+----------------+---------------+----------------+----------
       dbuser    | f        | t          | f             | t           | t           |           -1 |               |               | {}       | f              | f             | f              | t
       lily      | f        | t          | f             | f           | t           |           -1 |               |               | {}       | f              | f             | f              | f
       Ruby       | t        | t          | t             | t           | t           |           -1 |               |               | {}       | t              | t             | t              | t


Creating a Foreign Server
-------------------------

#. Use the user who is about to create a foreign server to connect to the corresponding database.

   In this example, use common user **dbuser** created in :ref:`(Optional) Creating a User and a Database and Granting the User Foreign Table Permissions <en-us_topic_0000001100335204__en-us_topic_0000001099130938_en-us_topic_0102810708_section590417587243>` to connect to **mydatabase** created by the user. You need to connect to the database through the database client tool provided by GaussDB(DWS).

   You can use the **gsql** client to log in to the database in either of the following ways:

   -  If you have logged in to the gsql client, run the following command to switch the database and user:

      ::

         \c mydatabase dbuser;

      Enter the password as prompted.

   -  If you have not logged in to the gsql client or have exited the gsql client by running the **\\q** command, run the following command to reconnect to it:

      ::

         gsql -d mydatabase -h 192.168.2.30 -U dbuser -p 8000 -r

      Enter the password as prompted.

#. Create a foreign server.

   For details about the syntax for creating foreign servers, see CREATE SERVER.

   For example, run the following command to create a foreign server named **obs_server**.

   ::

      CREATE SERVER obs_server FOREIGN DATA WRAPPER dfs_fdw
      OPTIONS (
        address 'obs.otc.t-systems.com' ,
        ACCESS_KEY 'access_key_value_to_be_replaced',
        SECRET_ACCESS_KEY 'secret_access_key_value_to_be_replaced',
        encrypt 'on',
        type 'obs'
      );

   Mandatory parameters are described as follows:

   -  *Name of the foreign server*

      You can customize a name.

      In this example, the name is set to **obs_server**.

   -  **FOREIGN DATA WRAPPER**

      *fdw_name* can be **hdfs_fdw** or **dfs_fdw**, which already exists in the database.

   -  **OPTIONS parameters**

      -  **address**

         Specifies the endpoint of the OBS service.

         Obtain the address as follows:

         a. Obtain the OBS path by performing :ref:`2 <en-us_topic_0000001146615221__en-us_topic_0000001145410931_en-us_topic_0102810712_li12771154711>` in :ref:`Preparing Data on OBS <dws_04_0243>`.
         b. The OBS endpoint viewed on the OBS is **obs.**\ *xxx*.\ *xxx*.\ **com**.

      -  (Mandatory) **Access keys (AK and SK)**

         GaussDB(DWS) needs to use the access keys (AK and SK) to access OBS. Therefore, you must obtain the access keys first.

         -  (Mandatory) **access_key**: specifies users' AK information.
         -  (Mandatory) **secret_access_key**: specifies users' SK information.

         For details about how to obtain the access keys, see :ref:`Creating Access Keys (AK and SK) <dws_04_0183>`.

      -  **type**

         Its value is **obs**, which indicates that **dfs_fdw** connects to OBS.

#. View the foreign server.

   ::

      SELECT * FROM pg_foreign_server WHERE srvname='obs_server';

   The server is successfully created if the returned result is as follows:

   ::

        srvname   | srvowner | srvfdw | srvtype | srvversion | srvacl |                                                                                      srvoptions

      ------------+----------+--------+---------+------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
      -----------
       obs_server |    24661 |  13686 |         |            |        | {address=xxx.xxx.x.xxx,access_key=xxxxxxxxxxxxxxxxxxxx,type=obs,secret_access_key=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
      (1 row)
