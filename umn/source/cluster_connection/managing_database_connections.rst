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

   +---------------------------+-------------------------------------------------------------------------------------------------------+--------------------------+----------------------------------------------------------------------------+
   | Parameter                 | Description                                                                                           | Number of CN Connections | Number of DN Connections                                                   |
   +===========================+=======================================================================================================+==========================+============================================================================+
   | max_connections           | Specifies the maximum number of concurrent connections to the database.                               | 800                      | Max (Number of vCPU cores/Number of DNs on a single node x 120 + 24, 5000) |
   +---------------------------+-------------------------------------------------------------------------------------------------------+--------------------------+----------------------------------------------------------------------------+
   | max_pool_size             | Specifies the maximum number of connections between the connection pool of a CN and another CN or DN. |                          |                                                                            |
   +---------------------------+-------------------------------------------------------------------------------------------------------+--------------------------+----------------------------------------------------------------------------+
   | max_prepared_transactions | Specifies the maximum number of transactions that can stay in the **prepared** state simultaneously.  |                          |                                                                            |
   +---------------------------+-------------------------------------------------------------------------------------------------------+--------------------------+----------------------------------------------------------------------------+

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

#. View the number of connections in scenarios described in :ref:`Table 2 <en-us_topic_0000001707293777__en-us_topic_0000001372520070_table573712172169>`.

   .. important::

      Except for database and user names that are enclosed with double quotation marks (") during creation, uppercase letters are not allowed in the database and user names in the commands in the following table.

   .. _en-us_topic_0000001707293777__en-us_topic_0000001372520070_table573712172169:

   .. table:: **Table 2** Viewing the number of connections

      +---------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Description                                                               | Command                                                                                                                                                |
      +===========================================================================+========================================================================================================================================================+
      | View the maximum number of sessions connected to a specific user.         | Run the following command to view the maximum number of sessions connected to user **dbadmin**.                                                        |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | ::                                                                                                                                                     |
      |                                                                           |                                                                                                                                                        |
      |                                                                           |    SELECT ROLNAME,ROLCONNLIMIT FROM PG_ROLES WHERE ROLNAME='dbadmin';                                                                                  |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | Information similar to the following is displayed. **-1** indicates that the number of sessions connected to user **dbadmin** is not limited.          |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | .. code-block::                                                                                                                                        |
      |                                                                           |                                                                                                                                                        |
      |                                                                           |     rolname  | rolconnlimit                                                                                                                            |
      |                                                                           |    ----------+--------------                                                                                                                           |
      |                                                                           |     dwsadmin |           -1                                                                                                                            |
      |                                                                           |    (1 row)                                                                                                                                             |
      +---------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the number of session connections that have been used by a user.     | Run the following command to view the number of session connections that have been used by **dbadmin**.                                                |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | ::                                                                                                                                                     |
      |                                                                           |                                                                                                                                                        |
      |                                                                           |    SELECT COUNT(*) FROM V$SESSION WHERE USERNAME='dbadmin';                                                                                            |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | Information similar to the following is displayed. **1** indicates the number of session connections used by user **dbadmin**.                         |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | .. code-block::                                                                                                                                        |
      |                                                                           |                                                                                                                                                        |
      |                                                                           |     count                                                                                                                                              |
      |                                                                           |    -------                                                                                                                                             |
      |                                                                           |         1                                                                                                                                              |
      |                                                                           |    (1 row)                                                                                                                                             |
      +---------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the maximum number of sessions connected to a specific database.     | Run the following command to view the upper limit of connections used by the **postgres** database:                                                    |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | ::                                                                                                                                                     |
      |                                                                           |                                                                                                                                                        |
      |                                                                           |    SELECT DATNAME,DATCONNLIMIT FROM PG_DATABASE WHERE DATNAME='postgres';                                                                              |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | Information similar to the following is displayed. **-1** indicates that the number of sessions connected to the **postgres** database is not limited. |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | .. code-block::                                                                                                                                        |
      |                                                                           |                                                                                                                                                        |
      |                                                                           |     datname  | datconnlimit                                                                                                                            |
      |                                                                           |    ----------+--------------                                                                                                                           |
      |                                                                           |     postgres |           -1                                                                                                                            |
      |                                                                           |    (1 row)                                                                                                                                             |
      +---------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the number of session connections that have been used by a database. | Run the following command to view the number of session connections that have been used by the **postgres** database:                                  |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | ::                                                                                                                                                     |
      |                                                                           |                                                                                                                                                        |
      |                                                                           |    SELECT COUNT(*) FROM PG_STAT_ACTIVITY WHERE DATNAME='postgres';                                                                                     |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | Information similar to the following is displayed. **1** indicates the number of session connections used by the **postgres** database.                |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | .. code-block::                                                                                                                                        |
      |                                                                           |                                                                                                                                                        |
      |                                                                           |     count                                                                                                                                              |
      |                                                                           |    -------                                                                                                                                             |
      |                                                                           |         1                                                                                                                                              |
      |                                                                           |    (1 row)                                                                                                                                             |
      +---------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
      | View the number of session connections that have been used by all users.  | Run the following command to view the number of session connections that have been used by all users:                                                  |
      |                                                                           |                                                                                                                                                        |
      |                                                                           | ::                                                                                                                                                     |
      |                                                                           |                                                                                                                                                        |
      |                                                                           |                                                                                                                                                        |
      |                                                                           |    SELECT COUNT(*) FROM PG_STAT_ACTIVITY;                                                                                                              |
      |                                                                           |     count                                                                                                                                              |
      |                                                                           |    -------                                                                                                                                             |
      |                                                                           |         10                                                                                                                                             |
      |                                                                           |    (1 row)                                                                                                                                             |
      +---------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
