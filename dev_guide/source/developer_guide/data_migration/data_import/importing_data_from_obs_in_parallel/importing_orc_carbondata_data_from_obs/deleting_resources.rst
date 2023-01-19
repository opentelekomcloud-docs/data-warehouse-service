:original_name: dws_04_0247.html

.. _dws_04_0247:

Deleting Resources
==================

After completing operations in this tutorial, if you no longer need to use the resources created during the operations, you can delete them to avoid resource waste or quota occupation. The procedure is as follows:

#. :ref:`Deleting the Foreign Table and Target Table <en-us_topic_0000001146815077__en-us_topic_0000001145491107_en-us_topic_0102810711_section436416582815>`

#. :ref:`Deleting the Created Foreign Server <en-us_topic_0000001146815077__en-us_topic_0000001145491107_en-us_topic_0102810711_section12369357281>`

#. :ref:`Deleting the Database and the User to Which the Database Belongs <en-us_topic_0000001146815077__en-us_topic_0000001145491107_en-us_topic_0102810711_section12067301412>`

   If you have performed steps in :ref:`(Optional) Creating a User and a Database and Granting the User Foreign Table Permissions <en-us_topic_0000001100335204__en-us_topic_0000001099130938_en-us_topic_0102810708_section590417587243>`, delete the database and the user to which the database belongs.

.. _en-us_topic_0000001146815077__en-us_topic_0000001145491107_en-us_topic_0102810711_section436416582815:

Deleting the Foreign Table and Target Table
-------------------------------------------

#. (Optional) If you have performed steps in :ref:`Querying Data After Importing It <en-us_topic_0000001146735141__en-us_topic_0000001099130958_en-us_topic_0102810710_section152121815193012>`, run the following command to delete the target table:

   ::

      DROP TABLE product_info;

   If the following information is displayed, the table has been deleted.

   .. code-block::

      DROP TABLE

#. Run the following statement to delete the foreign table:

   ::

      DROP FOREIGN TABLE product_info_ext_obs;

   If the following information is displayed, the table has been deleted.

   .. code-block::

      DROP FOREIGN TABLE

.. _en-us_topic_0000001146815077__en-us_topic_0000001145491107_en-us_topic_0102810711_section12369357281:

Deleting the Created Foreign Server
-----------------------------------

#. Use the user who created the foreign server to connect to the database where the foreign server is located.

   In this example, common user **dbuser** is used to create the foreign server in **mydatabase**. You need to connect to the database through the database client tool provided by GaussDB(DWS). You can use the gsql client to log in to the database in either of the following ways:

   -  If you have logged in to the gsql client, run the following command to switch the database and user:

      ::

         \c mydatabase dbuser;

      Enter the password as prompted.

   -  If you have logged in to the gsql client, you can run the **\\q** command to exit gsql, and run the following command to reconnect to it:

      ::

         gsql -d mydatabase -h 192.168.2.30 -U dbuser -p 8000 -r

      Enter the password as prompted.

#. Delete the created foreign server.

   Run the following command to delete the server. For details about the syntax, see DROP SERVER.

   ::

      DROP SERVER obs_server;

   The database is deleted if the following information is displayed:

   .. code-block::

      DROP SERVER

   View the foreign server.

   ::

      SELECT * FROM pg_foreign_server WHERE srvname='obs_server';

   The server is successfully deleted if the returned result is as follows:

   .. code-block::

       srvname | srvowner | srvfdw | srvtype | srvversion | srvacl | srvoptions
      ---------+----------+--------+---------+------------+--------+------------
      (0 rows)

.. _en-us_topic_0000001146815077__en-us_topic_0000001145491107_en-us_topic_0102810711_section12067301412:

Deleting the Database and the User to Which the Database Belongs
----------------------------------------------------------------

If you have performed steps in :ref:`(Optional) Creating a User and a Database and Granting the User Foreign Table Permissions <en-us_topic_0000001100335204__en-us_topic_0000001099130938_en-us_topic_0102810708_section590417587243>`, perform the following steps to delete the database and the user to which the database belongs.

#. Delete the customized database.

   Connect to the default database **gaussdb** through the database client tool provided by GaussDB(DWS).

   If you have logged in to the database using the gsql client, run the following command to switch the database and user:

   Switch to the default database.

   .. code-block::

      \c gaussdb

   Enter your password as prompted.

   Run the following command to delete the customized database:

   ::

      DROP DATABASE mydatabase;

   The database is deleted if the following information is displayed:

   .. code-block::

      DROP DATABASE

#. Delete the common user created in this example as the administrator.

   Connect to the database as a database administrator through the database client tool provided by GaussDB(DWS).

   If you have logged in to the database using the **gsql** client, run the following command to switch the database and user:

   .. code-block::

      \c gaussdb dbadmin

   Run the following command to reclaim the permission for creating foreign servers:

   ::

      REVOKE ALL ON FOREIGN DATA WRAPPER dfs_fdw FROM dbuser;

   The name of **FOREIGN DATA WRAPPER** must be **dfs_fdw**. **dbuser** is the username for creating **SERVER**.

   Run the following command to delete the user:

   ::

      DROP USER dbuser;

   You can run the **\\du** command to query for the user and check whether the user has been deleted.
