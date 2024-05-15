:original_name: dws_04_0216.html

.. _dws_04_0216:

Deleting Resources
==================

After completing operations in this tutorial, if you no longer need to use the resources created during the operations, you can delete them to avoid resource waste or quota occupation.

Deleting the Foreign Table and Target Table
-------------------------------------------

#. (Optional) If operations in :ref:`Querying Data After Importing It <en-us_topic_0000001233883187__en-us_topic_0000001083024575_en-us_topic_0109259518_en-us_topic_0101477887_section1375535445410>` have been performed, run the following command to delete the target table:

   ::

      DROP TABLE product_info;

#. Run the following command to delete the foreign table:

   ::

      DROP FOREIGN TABLE foreign_product_info;

.. _en-us_topic_0000001233681609__en-us_topic_0000001082926731_en-us_topic_0109259519_en-us_topic_0102427953_section79551640133718:

Deleting the Manually Created Foreign Server
--------------------------------------------

If operations in :ref:`Manually Creating a Foreign Server <dws_04_0213>` have been performed, perform the following steps to delete the foreign server, database, and user:

#. Use the client provided by GaussDB(DWS) to connect to the database where the foreign server resides as the user who created the foreign server.

   You can use the **gsql** client to log in to the database in either of the following ways:

   -  If you have logged in to the gsql client, run the following command to switch the database and user:

      ::

         \c mydatabase dbuser;

      Enter the password as prompted.

   -  If you have logged in to the gsql client, you can run the **\\q** command to exit gsql, and run the following command to reconnect to it:

      ::

         gsql -d mydatabase -h 192.168.2.30 -U dbuser -p 8000 -r

      Enter the password as prompted.

#. Delete the manually created foreign server.

   Run the following command to delete the server. For details about the syntax, see DROP SERVER.

   ::

      DROP SERVER hdfs_server_8f79ada0_d998_4026_9020_80d6de2692ca;

   The foreign server is deleted if the following information is displayed:

   ::

      DROP SERVER

   View the foreign server.

   ::

      SELECT * FROM pg_foreign_server WHERE srvname='hdfs_server_8f79ada0_d998_4026_9020_80d6de2692ca';

   The server is successfully deleted if the returned result is as follows:

   ::

       srvname | srvowner | srvfdw | srvtype | srvversion | srvacl | srvoptions
      ---------+----------+--------+---------+------------+--------+------------
      (0 rows)

#. Delete the customized database.

   Connect to the default database **postgres** through the database client tool provided by GaussDB(DWS).

   If you have logged in to the database using the **gsql** client, run the following command to switch the database and user:

   ::

      \c postgres

   Enter your password as prompted.

   Run the following command to delete the customized database:

   ::

      DROP DATABASE mydatabase;

   The database is deleted if the following information is displayed:

   ::

      DROP DATABASE

#. Delete the common user created in this example as the administrator.

   Connect to the database as a database administrator through the database client tool provided by GaussDB(DWS).

   If you have logged in to the database using the **gsql** client, run the following command to switch the database and user:

   ::

      \c postgres dbadmin

   Run the following command to reclaim the permission for creating foreign servers:

   ::

      REVOKE ALL ON FOREIGN DATA WRAPPER hdfs_fdw FROM dbuser;

   The name of **FOREIGN DATA WRAPPER** must be **hdfs_fdw**. **dbuser** is the username for creating **SERVER**.

   Run the following command to delete the user:

   ::

      DROP USER dbuser;

   You can run the **\\du** command to query for the user and check whether the user has been deleted.
