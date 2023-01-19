:original_name: dws_01_0039.html

.. _dws_01_0039:

Managing Database Connections
=============================

Scenario
--------

By default, a database supports a certain number of connections. Administrators can manage database connections to learn about the connection performance of the current database or increase the connection limit so that more users or applications can connect to the database at the same time.

Maximum Number of Connections
-----------------------------

The number of connections supported by a cluster depends on its node flavor.

.. table:: **Table 1** Number of supported connections

   +-----------------+---------------------+--------------------------+------------------------------+
   | Parameter       | Node Flavor         | Number of CN Connections | Number of DN Connections     |
   +=================+=====================+==========================+==============================+
   | max_connections | vCPUs < 16          | 512                      | Number of CN connections x 2 |
   +-----------------+---------------------+--------------------------+------------------------------+
   |                 | vCPUs > 16 && <= 32 | 1024                     | Number of CN connections x 2 |
   +-----------------+---------------------+--------------------------+------------------------------+
   |                 | other               | 2048                     | Number of CN connections x 2 |
   +-----------------+---------------------+--------------------------+------------------------------+

.. note::

   The policies of **comm_max_stream**, **poolsize**, and **max_prepared_transactions** are the same as those of **max_connections**.

Viewing the Maximum Number of Connections
-----------------------------------------

#. Use the SQL client tool to connect to the database in a cluster.

#. Run the following command:

   ::

      SHOW max_connections;

   Information similar to the following is displayed, showing that the maximum number of database connections is **200** by default.

   .. code-block::

      max_connections
      -----------------
      200
      (1 row)

Viewing the Number of Used Connections
--------------------------------------

#. Use the SQL client tool to connect to the database in a cluster.

#. View the number of connections in scenarios described in :ref:`Table 2 <en-us_topic_0000001134560490__tecae727d5c1d47f897891d48c13a5589>`.

   .. _en-us_topic_0000001134560490__tecae727d5c1d47f897891d48c13a5589:

   .. table:: **Table 2** Viewing the number of connections

      +---------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Description                                                               | Command                                                                                                                                           |
      +===========================================================================+===================================================================================================================================================+
      | View the maximum number of sessions connected to a specific user.         | Run the following command to view the maximum number of sessions connected to user **dbadmin**.                                                   |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | ::                                                                                                                                                |
      |                                                                           |                                                                                                                                                   |
      |                                                                           |    SELECT ROLNAME,ROLCONNLIMIT FROM PG_ROLES WHERE ROLNAME='dbadmin';                                                                             |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | Information similar to the following is displayed. **-1** indicates that the number of sessions connected to user **dbadmin** is not limited.     |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | .. code-block::                                                                                                                                   |
      |                                                                           |                                                                                                                                                   |
      |                                                                           |     rolname  | rolconnlimit                                                                                                                       |
      |                                                                           |    ----------+--------------                                                                                                                      |
      |                                                                           |     dwsadmin |           -1                                                                                                                       |
      |                                                                           |    (1 row)                                                                                                                                        |
      +---------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the number of session connections that have been used by a user.     | Run the following command to view the number of session connections that have been used by **dbadmin**.                                           |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | ::                                                                                                                                                |
      |                                                                           |                                                                                                                                                   |
      |                                                                           |    SELECT COUNT(*) FROM V$SESSION WHERE USERNAME='dbadmin';                                                                                       |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | Information similar to the following is displayed. **1** indicates the number of session connections used by user **dbadmin**.                    |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | .. code-block::                                                                                                                                   |
      |                                                                           |                                                                                                                                                   |
      |                                                                           |     count                                                                                                                                         |
      |                                                                           |    -------                                                                                                                                        |
      |                                                                           |         1                                                                                                                                         |
      |                                                                           |    (1 row)                                                                                                                                        |
      +---------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the maximum number of sessions connected to a specific database.     | Run the following command to view the upper limit of connections used by **gaussdb**.                                                             |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | ::                                                                                                                                                |
      |                                                                           |                                                                                                                                                   |
      |                                                                           |    SELECT DATNAME,DATCONNLIMIT FROM PG_DATABASE WHERE DATNAME='gaussdb';                                                                          |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | Information similar to the following is displayed. **-1** indicates that the number of sessions connected to database **gaussdb** is not limited. |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | .. code-block::                                                                                                                                   |
      |                                                                           |                                                                                                                                                   |
      |                                                                           |     datname  | datconnlimit                                                                                                                       |
      |                                                                           |    ----------+--------------                                                                                                                      |
      |                                                                           |     gaussdb |           -1                                                                                                                        |
      |                                                                           |    (1 row)                                                                                                                                        |
      +---------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the number of session connections that have been used by a database. | Run the following command to view the number of session connections that have been used by **gaussdb**.                                           |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | ::                                                                                                                                                |
      |                                                                           |                                                                                                                                                   |
      |                                                                           |    SELECT COUNT(*) FROM PG_STAT_ACTIVITY WHERE DATNAME='gaussdb';                                                                                 |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | Information similar to the following is displayed. **1** indicates the number of session connections used by database **gaussdb**.                |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | .. code-block::                                                                                                                                   |
      |                                                                           |                                                                                                                                                   |
      |                                                                           |     count                                                                                                                                         |
      |                                                                           |    -------                                                                                                                                        |
      |                                                                           |         1                                                                                                                                         |
      |                                                                           |    (1 row)                                                                                                                                        |
      +---------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the number of session connections that have been used by all users.  | Run the following command to view the number of session connections that have been used by all users:                                             |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | ::                                                                                                                                                |
      |                                                                           |                                                                                                                                                   |
      |                                                                           |    SELECT COUNT(*) FROM V$SESSION;                                                                                                                |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | Information similar to the following is displayed.                                                                                                |
      |                                                                           |                                                                                                                                                   |
      |                                                                           | .. code-block::                                                                                                                                   |
      |                                                                           |                                                                                                                                                   |
      |                                                                           |     count                                                                                                                                         |
      |                                                                           |    -------                                                                                                                                        |
      |                                                                           |         10                                                                                                                                        |
      |                                                                           |    (1 row)                                                                                                                                        |
      +---------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
