:original_name: dws_06_0055.html

.. _dws_06_0055:

Server Signaling Functions
==========================

Server signaling functions send control signals to other server processes. Only system administrators can use these functions.

pg_cancel_backend(pid int)
--------------------------

Description: Cancels a current query of the backend.

Return type: boolean

Note: **pg_cancel_backend** sends a query cancellation signal (**SIGINT**) to the backend process identified by **pid**. The PID of an active backend process can be found in the **pid** column of the **pg_stat_activity** view, or can be found by listing the database process using **ps** on the server.

Example:

::

   SELECT pid FROM pg_stat_activity WHERE stmt_type ='RESET';
          pid
   -----------------
    281471222065200
   (1 row)

   SELECT pg_cancel_backend(281471222065200);
    pg_cancel_backend
   -------------------
    t
   (1 row)

pg_reload_conf()
----------------

Description: Notifies the server process to reload the configuration file.

Return type: boolean

Note: **pg_reload_conf** sends a **SIGHUP** signal to the server. As a result, all server processes reload their configuration files.

Example:

::

   SELECT pg_reload_conf();
    pg_reload_conf
   ----------------
    t
   (1 row)

pg_rotate_logfile()
-------------------

Description: Rotates the log files of the server.

Return type: boolean

Note: pg_rotate_logfile instructs the log file manager to immediately switch to a new output file. This function is valid only if the built-in log collector is running.

Example:

::

   SELECT pg_rotate_logfile();
    pg_rotate_logfile
   -------------------
    t
   (1 row)

pg_terminate_backend(pid int)
-----------------------------

Description: Terminates a backend thread.

Return type: boolean

Note: Each of these functions returns **true** if they are successful and **false** otherwise.

Example:

::

   SELECT pid FROM pg_stat_activity;
          pid
   -----------------
    140657876268816
    140433774061312
    140433587902208
    140433656592128
    140433723717376
    140433637189376
    140433552770816
    140433481983744
    140433349310208
   (9 rows)

   SELECT pg_terminate_backend(140657876268816);
    pg_terminate_backend
   ----------------------
    t
   (1 row)

pg_wlm_jump_queue(pid int)
--------------------------

Description: Moves a task to the top of the CN queue.

Return type: Boolean

Note: Each of these functions returns **true** if they are successful and **false** otherwise.

Example:

::

   SELECT pid FROM pg_stat_activity WHERE stmt_type ='RESET';
          pid
   -----------------
    281471222065200
   (1 row)

   SELECT pg_wlm_jump_queue(281471222065200);
    pg_wlm_jump_queue
   -------------------
    t
   (1 row)

gs_wlm_switch_cgroup(pid int, cgroup text)
------------------------------------------

Description: Moves a job to other Cgroup to improve the job priority.

Return type: Boolean

Note: Each of these functions returns **true** if they are successful and **false** otherwise.

pg_cancel_query(queryId int)
----------------------------

Description: Cancels a current query of the backend. This function is supported in 8.1.2 or later.

Return type: boolean

Note: **pg_cancel_query** sends a query cancellation signal (**SIGINT**) to the backend process identified by **query_id**. The **query_id** of an active backend process can be found in the **query_id** column of the **pg_stat_activity** view.

Example:

::

   SELECT query_id FROM pgxc_stat_activity WHERE stmt_type ='RESET';
    query_id
   ----------
           0
           0
   (2 rows)

   SELECT pg_cancel_query(0);
    pg_cancel_query
   -----------------
    f
   (1 row)

pgxc_cancel_query(queryId int)
------------------------------

Description: Cancels the query that is being executed in the current cluster. This function is supported in 8.1.2 or later.

Return type: boolean

Note: If the queries of all nodes have been canceled, **true** is returned. Otherwise, **false** is returned.

Example:

::

   SELECT query_id FROM pgxc_stat_activity WHERE stmt_type ='RESET';
    query_id
   ----------
           0
           0
   (2 rows)

   SELECT pgxc_cancel_query(0);
    pgxc_cancel_query
   -------------------
    f
   (1 row)

pg_terminate_query(queryId int)
-------------------------------

Description: Terminates a current query of the backend. This function is supported in 8.1.2 or later.

Return type: boolean

Example:

::

   SELECT query_id FROM pgxc_stat_activity WHERE stmt_type ='RESET';
    query_id
   ----------
           0
           0
   (2 rows)

   SELECT pg_terminate_query(0);
    pg_terminate_query
   --------------------
    f
   (1 row)

pgxc_terminate_query(queryId int)
---------------------------------

Description: Terminates the query that is being executed in the current cluster. This function is supported in 8.1.2 or later.

Return type: boolean

Example:

::

   SELECT query_id FROM pgxc_stat_activity;
       query_id
   -----------------
    72339069014638631
   (1 rows)

   SELECT pgxc_terminate_query(72339069014638631);
    pgxc_terminate_query
   ----------------------
    t
   (1 row)
