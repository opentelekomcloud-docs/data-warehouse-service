:original_name: dws_04_0413.html

.. _dws_04_0413:

Checking Blocked Statements
===========================

During database running, query statements are blocked in some service scenarios and run for an excessively long time. In this case, you can forcibly terminate the faulty session.

Procedure
---------

#. View blocked query statements and information about the tables and schemas that block the query statements.

   ::

      SELECT w.query as waiting_query,
      w.pid as w_pid,
      w.usename as w_user,
      l.query as locking_query,
      l.pid as l_pid,
      l.usename as l_user,
      t.schemaname || '.' || t.relname as tablename
      from pg_stat_activity w join pg_locks l1 on w.pid = l1.pid
      and not l1.granted join pg_locks l2 on l1.relation = l2.relation
      and l2.granted join pg_stat_activity l on l2.pid = l.pid join pg_stat_user_tables t on l1.relation = t.relid
      where w.waiting;

   The thread ID, user information, query status, as well as information about the tables and schemas that block the query statements are returned.

#. Run the following command to terminate the required session, where **139834762094352** is the thread ID:

   ::

      SELECT PG_TERMINATE_BACKEND(139834762094352);

   If information similar to the following is displayed, the session is successfully terminated:

   .. code-block::

       PG_TERMINATE_BACKEND
      ----------------------
       t
      (1 row)

   If a command output similar to the following is displayed, a user is attempting to terminate the session, and the session will be reconnected rather than being terminated.

   .. code-block::

      FATAL:  terminating connection due to administrator command
      FATAL:  terminating connection due to administrator command
      The connection to the server was lost. Attempting reset: Succeeded.

   .. note::

      If the **PG_TERMINATE_BACKEND** function is used to terminate the background threads of the session, the gsql client will be reconnected rather than be logged out.
