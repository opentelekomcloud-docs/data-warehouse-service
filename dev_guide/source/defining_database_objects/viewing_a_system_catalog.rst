:original_name: dws_04_0034.html

.. _dws_04_0034:

Viewing a System Catalog
========================

In addition to the created tables, a database contains many system catalogs These system catalogs contain cluster installation information and information about various queries and processes in GaussDB(DWS). You can collect information about the database by querying the system catalog.

Querying Database Tables
------------------------

For example, query the **PG_TABLES** system catalog for all tables in the **public** schema.

::

   SELECT distinct(tablename) FROM pg_tables WHERE SCHEMANAME = 'public';

Information similar to the following is displayed:

::

        tablename
   -------------------
    err_hr_staffs
    test
    err_hr_staffs_ft3
    web_returns_p1
    mig_seq_table
    films4
   (6 rows)

Viewing Database Users
----------------------

You can run the **PG_USER** command to view the list of all users in the database, and view the user ID (**USESYSID**) and permissions.

::

   SELECT * FROM pg_user;
   usename | usesysid | usecreatedb | usesuper | usecatupd | userepl |  passwd  | valbegin | valuntil |   respool    | parent | spacelimit | useconfig | nodegroup | tempspacelimit | spillspacelim
   it
   ---------+----------+-------------+----------+-----------+---------+----------+----------+----------+--------------+--------+------------+-----------+-----------+----------------+--------------
   ---
    Ruby     |       10 | t           | t        | t         | t       | ******** |          |          | default_pool |      0 |            |           |           |                |
    dbadmin |    16393 | f           | f        | f         | f       | ******** |          |          | default_pool |      0 |            |           |           |                |
    lily    |    16691 | f           | f        | f         | f       | ******** |          |          | default_pool |      0 |            |           |           |                |
    jack    |    70694 | f           | f        | f         | f       | ******** |          |          | default_pool |      0 |            |           |           |                |
   (4 rows)

GaussDB(DWS) uses Ruby to perform routine management and maintenance. You can add **WHERE usesysid > 10** to the **SELECT** statement to filter queries so that only specified user names are displayed.

::

   SELECT * FROM pg_user WHERE usesysid > 10;
    usename | usesysid | usecreatedb | usesuper | usecatupd | userepl |  passwd  | valbegin | valuntil |   respool    | parent | spacelimit | useconfig | nodegroup | tempspacelimit | spillspacelim
   it
   ---------+----------+-------------+----------+-----------+---------+----------+----------+----------+--------------+--------+------------+-----------+-----------+----------------+--------------
   ---
    dbadmin |    16393 | f           | f        | f         | f       | ******** |          |          | default_pool |      0 |            |           |           |                |
    lily    |    16691 | f           | f        | f         | f       | ******** |          |          | default_pool |      0 |            |           |           |                |
    jack    |    70694 | f           | f        | f         | f       | ******** |          |          | default_pool |      0 |            |           |           |                |
   (3 rows)

Viewing and Stopping the Running Query Statements
-------------------------------------------------

You can view the running query statements in the :ref:`PG_STAT_ACTIVITY <dws_04_0755>` view. Do as follows:

#. Set the parameter **track_activities** to **on**.

   ::

      SET track_activities = on;

   The database collects the running information about active queries only if the parameter is set to **on**.

#. View the running query statements. Run the following command to view the database names, users, query statuses, and PIDs of the running query statements:

   ::

      SELECT datname, usename, state,pid FROM pg_stat_activity;

   If the **state** column is **idle**, the connection is idle and requires a user to enter a command.

   To identify only active query statements, run the following command:

   ::

      SELECT datname, usename, state FROM pg_stat_activity WHERE state != 'idle';

#. To cancel queries that have been running for a long time, use the **PG_TERMINATE_BACKEND** function to end sessions based on the thread ID.

   ::

      SELECT PG_TERMINATE_BACKEND(139834759993104);

   If information similar to the following is displayed, the session is successfully terminated:

   ::

      PG_TERMINATE_BACKEND
      ----------------------
       t
      (1 row)

   If information similar to the following is displayed, a user has terminated the current session.

   ::

      FATAL:  terminating connection due to administrator command
      FATAL:  terminating connection due to administrator command

   .. note::

      If the **PG_TERMINATE_BACKEND** function is used to terminate the backend threads of the current session, the gsql client will be reconnected automatically rather than be logged out. The message "The connection to the server was lost." is returned. Attempting reset: Succeeded."

      ::

         FATAL: terminating connection due to administrator command
         FATAL: terminating connection due to administrator command
         The connection to the server was lost. Attempting reset: Succeeded.
