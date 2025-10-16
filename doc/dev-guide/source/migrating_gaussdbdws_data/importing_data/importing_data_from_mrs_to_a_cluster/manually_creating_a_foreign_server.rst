:original_name: dws_04_0213.html

.. _dws_04_0213:

Manually Creating a Foreign Server
==================================

In the syntax **CREATE FOREIGN TABLE (SQL on Hadoop or OBS)** for creating a foreign table, you need to specify a foreign server associated with the MRS data source connection.

When you create an MRS data source connection on the GaussDB(DWS) management console, the database administrator dbadmin automatically creates a foreign server in the default database **postgres**. If you want to create a foreign table in the default database **postgres** to read MRS data, skip this section.

To allow a common user to create a foreign table in a user-defined database to read MRS data, you must manually create a foreign server in the user-defined database. This section describes how does a common user create a foreign server in a user-defined database. The procedure is as follows:

#. Ensure that an MRS data source connection has been created for the GaussDB(DWS) cluster.

   For details, see section "MRS Data Sources > Creating an MRS Data Source Connection" in the *Data Warehouse Service User Guide*.

#. :ref:`Creating a User and a Database and Granting the User Foreign Table Permissions <en-us_topic_0000001764491984__en-us_topic_0000001188642138_en-us_topic_0000001082926737_en-us_topic_0109259516_en-us_topic_0101997156_section765119474519>`

#. :ref:`Manually Creating a Foreign Server <en-us_topic_0000001764491984__en-us_topic_0000001188642138_en-us_topic_0000001082926737_en-us_topic_0109259516_en-us_topic_0101997156_section070174417129>`

.. note::

   If you no longer need to read data from the MRS data source and have deleted the MRS data source on the GaussDB(DWS) management console, only the foreign server automatically created in the default database **postgres** will be deleted, and the manually created foreign server needs to be deleted manually. For details about the deletion, see :ref:`Deleting the Manually Created Foreign Server <en-us_topic_0000001811610425__en-us_topic_0000001233681609_en-us_topic_0000001082926731_en-us_topic_0109259519_en-us_topic_0102427953_section79551640133718>`.

.. _en-us_topic_0000001764491984__en-us_topic_0000001188642138_en-us_topic_0000001082926737_en-us_topic_0109259516_en-us_topic_0101997156_section765119474519:

Creating a User and a Database and Granting the User Foreign Table Permissions
------------------------------------------------------------------------------

In the following example, a common user **dbuser** and a database **mydatabase** are created. Then, an administrator is used to grant foreign table permissions to user **dbuser**.

#. Connect to the default database **gaussdb** as a database administrator through the database client tool provided by GaussDB(DWS).

   For example, use the **gsql** client to connect to the database by running the following command:

   ::

      gsql -d gaussdb -h 192.168.2.30 -U dbadmin -p 8000 -W password -r

#. Create a common user and use it to create a database.

   Create a user named **dbuser** that has the permission to create databases.

   ::

      CREATE USER dbuser WITH CREATEDB PASSWORD 'password';

   Switch to the created user.

   ::

      SET ROLE dbuser PASSWORD 'password';

   Run the following command to create a database:

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

   Use the connection to create a database as a database administrator.

   You can use the **gsql** client to run the following command, switching to an administrator user, and connect to the new database:

   ::

      \c mydatabase dbadmin;

   Enter the password as prompted.

   .. note::

      Note that you must use the administrator account to connect to the database where a foreign server is to be created and foreign tables are used; and then grant permissions to the common user.

   By default, only system administrators can create foreign servers. Common users can create foreign servers only after being authorized. Run the following command to grant the permission:

   ::

      GRANT ALL ON FOREIGN DATA WRAPPER hdfs_fdw TO dbuser;

   The name of **FOREIGN DATA WRAPPER** must be **hdfs_fdw**. **dbuser** is the username for creating **SERVER**.

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

   The authorization is successful if the **dbuser** information in the returned result contains the **UseFT** permission.

   ::

       rolname  | rolsuper | rolinherit | rolcreaterole | rolcreatedb | rolcanlogin | rolconnlimit | rolvalidbegin | rolvaliduntil | memberof | rolreplication | rolauditadmin | rolsystemadmin | roluseft
      -----------+----------+------------+---------------+-------------+-------------+--------------+---------------+---------------+----------+----------------+---------------+----------------+----------
       dbuser    | f        | t          | f             | t           | t           |           -1 |               |               | {}       | f              | f             | f              | t
       lily      | f        | t          | f             | f           | t           |           -1 |               |               | {}       | f              | f             | f              | f
       Ruby       | t        | t          | t             | t           | t           |           -1 |               |               | {}       | t              | t             | t              | t

.. _en-us_topic_0000001764491984__en-us_topic_0000001188642138_en-us_topic_0000001082926737_en-us_topic_0109259516_en-us_topic_0101997156_section070174417129:


Manually Creating a Foreign Server
----------------------------------

#. Connect to the default database **postgres** as a database administrator through the database client tool provided by GaussDB(DWS).

   You can use the **gsql** client to log in to the database in either of the following ways:

   You can use either of the following methods to create the connection:

   -  If you have logged in to the gsql client, run the following command to switch the database and user:

      ::

         \c postgres dbadmin;

      Enter the password as prompted.

   -  If you have not logged in to the gsql client or have exited the gsql client by running the **\\q** command, run the following command to reconnect to it:

      ::

         gsql -d postgres -h 192.168.2.30 -U dbadmin -p 8000 -W password -r

#. .. _en-us_topic_0000001764491984__en-us_topic_0000001188642138_en-us_topic_0000001082926737_en-us_topic_0109259516_en-us_topic_0101997156_li142862473118:

   Run the following command to query the information about the foreign server that is automatically created:

   ::

      SELECT * FROM pg_foreign_server;

   The returned result is as follows:

   ::

                           srvname                      | srvowner | srvfdw | srvtype | srvversion | srvacl |                                                     srvoptions
      --------------------------------------------------+----------+--------+---------+------------+--------+---------------------------------------------------------------------------------------------------------------------
       gsmpp_server                                     |       10 |  13673 |         |            |        |
       hdfs_server_8f79ada0_d998_4026_9020_80d6de2692ca |    16476 |  13685 |         |            |        | {"address=192.168.1.245:25000,192.168.1.218:25000",hdfscfgpath=/MRS/8f79ada0-d998-4026-9020-80d6de2692ca,type=hdfs}
      (2 rows)

   In the query result, each row contains the information about a foreign server. The foreign server associated with the MRS data source connection contains the following information:

   -  The value of **srvname** contains **hdfs_server** and the ID of the MRS cluster, which is the same as the MRS ID in the cluster list on the MRS management console.
   -  The **address** parameter in the **srvoptions** field contains the IP addresses and ports of the active and standby nodes in the MRS cluster.

   You can find the foreign server you want based on the above information and record the values of its **srvname** and **srvoptions**.

#. Switch to the user who is about to create a foreign server to connect to the corresponding database.

   In this example, run the following command to use common user **dbuser** created in :ref:`Creating a User and a Database and Granting the User Foreign Table Permissions <en-us_topic_0000001764491984__en-us_topic_0000001188642138_en-us_topic_0000001082926737_en-us_topic_0109259516_en-us_topic_0101997156_section765119474519>` to connect to **mydatabase** created by the user:

   ::

      \c mydatabase dbuser;

#. Create a foreign server.

   For details about the syntax for creating foreign servers, see CREATE SERVER. For example:

   ::

      CREATE SERVER hdfs_server_8f79ada0_d998_4026_9020_80d6de2692cahdfs_server FOREIGN DATA WRAPPER HDFS_FDW
      OPTIONS
      (
      address '192.168.1.245:25000,192.168.1.218:25000',
      hdfscfgpath '/MRS/8f79ada0-d998-4026-9020-80d6de2692ca',
      type 'hdfs'
      );

   Mandatory parameters are described as follows:

   -  *Name of the foreign server*

      You can customize a name.

      In this example, specify the name to the value of the **srvname** field recorded in :ref:`2 <en-us_topic_0000001764491984__en-us_topic_0000001188642138_en-us_topic_0000001082926737_en-us_topic_0109259516_en-us_topic_0101997156_li142862473118>`, such as *hdfs_server_8f79ada0_d998_4026_9020_80d6de2692ca*.

      Resources in different databases are isolated. Therefore, the names of foreign servers in different databases can be the same.

   -  **FOREIGN DATA WRAPPER**

      This parameter can only be set to **HDFS_FDW**, which already exists in the database.

   -  **OPTIONS** parameters

      Set the following parameters to the values under **srvoptions** recorded in :ref:`2 <en-us_topic_0000001764491984__en-us_topic_0000001188642138_en-us_topic_0000001082926737_en-us_topic_0109259516_en-us_topic_0101997156_li142862473118>`.

      -  address

         Specifies the IP address and port number of the primary and standby nodes of the HDFS cluster.

      -  hdfscfgpath

         Specifies the configuration file path of the HDFS cluster. This parameter is available only when **type** is **HDFS**. You can set only one path.

      -  type

         Its value is **hdfs**, which indicates that **HDFS_FDW** connects to HDFS.

#. View the foreign server.

   ::

      SELECT * FROM pg_foreign_server WHERE srvname='hdfs_server_8f79ada0_d998_4026_9020_80d6de2692ca';

   The server is successfully created if the returned result is as follows:

   ::

                           srvname                      | srvowner | srvfdw | srvtype | srvversion | srvacl |                                                     srvoptions
      --------------------------------------------------+----------+--------+---------+------------+--------+---------------------------------------------------------------------------------------------------------------------
       hdfs_server_8f79ada0_d998_4026_9020_80d6de2692ca |    16476 |  13685 |         |            |        | {"address=192.168.1.245:25000,192.168.1.218:25000",hdfscfgpath=/MRS/8f79ada0-d998-4026-9020-80d6de2692ca,type=hdfs}
      (1 row)
