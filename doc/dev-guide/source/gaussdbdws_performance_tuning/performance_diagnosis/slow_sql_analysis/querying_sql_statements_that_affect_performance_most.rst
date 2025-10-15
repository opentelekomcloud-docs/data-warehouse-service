:original_name: dws_04_0412.html

.. _dws_04_0412:

Querying SQL Statements That Affect Performance Most
====================================================

This section describes how to query SQL statements whose execution takes a long time, leading to poor system performance.

Procedure
---------

#. Query the statements that are run for a long time in the database.

   ::

      SELECT current_timestamp - query_start AS runtime, datname, usename, query FROM pg_stat_activity where state != 'idle' ORDER BY 1 desc;

   After the query, query statements are returned as a list, ranked by execution time in descending order. The first result is the query statement that has the longest execution time in the system. The returned result contains the SQL statement invoked by the system and the SQL statement run by users. Find the statements that were run by users and took a long time.

   Alternatively, you can set **current_timestamp - query_start** to be greater than a threshold to identify query statements that are executed for a duration longer than this threshold.

   ::

      SELECT query FROM pg_stat_activity WHERE current_timestamp - query_start > interval '1 days';

#. Set the parameter **track_activities** to **on**.

   ::

      SET track_activities = on;

   The database collects the running information about active queries only if the parameter is set to **on**.

#. View the running query statements.

   Viewing **pg_stat_activity** is used as an example here.

   ::

      SELECT datname, usename, state FROM pg_stat_activity;
       datname  | usename | state  |
      ----------+---------+--------+
       postgres |   omm   | idle   |
       postgres |   omm   | active |
      (2 rows)

   If the **state** column is idle, the connection is idle and requires a user to enter a command.

   To identify only active query statements, run the following command:

   ::

      SELECT datname, usename, state FROM pg_stat_activity WHERE state != 'idle';

#. Analyze the status of the query statements that were run for a long time.

   -  If the query statement is normal, wait until the execution is complete.

   -  If a query statement is blocked, run the following command to view this query statement:

      ::

         SELECT datname, usename, state, query FROM pg_stat_activity WHERE waiting = true;

      The command output lists a query statement in the block state. The lock resource requested by this query statement is occupied by another session, so this query statement is waiting for the session to release the lock resource.

      .. note::

         Only when the query is blocked by internal lock resources, the **waiting** field is **true**. In most cases, block happens when query statements are waiting for lock resources to be released. However, query statements may be blocked because they are waiting to write in files or for timers. Such blocked queries are not displayed in the **pg_stat_activity** view.
