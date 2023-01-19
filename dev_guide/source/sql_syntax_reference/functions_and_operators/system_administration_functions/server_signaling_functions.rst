:original_name: dws_06_0055.html

.. _dws_06_0055:

Server Signaling Functions
==========================

Server signaling functions send control signals to other server processes. Only system administrators can use these functions.

-  pg_cancel_backend(pid int)

   Description: Cancels the current query of a backend.

   Return type: boolean

   Note: **pg_cancel_backend** sends a query cancellation (SIGINT) signal to the backend process identified by **pid**. The PID of an active backend process can be found in the **pid** column of the **pg_stat_activity** view, or can be found by listing the database process using **ps** on the server.

-  pg_reload_conf()

   Description: Causes all server processes to reload their configuration files.

   Return type: boolean

   Note: **pg_reload_conf** sends a SIGHUP signal to the server. As a result, all server processes reload their configuration files.

-  pg_rotate_logfile()

   Description: Rotates the log files of the server.

   Return type: boolean

   Note: pg_rotate_logfile instructs the log file manager to immediately switch to a new output file. This function is valid only if the built-in log collector is running.

-  pg_terminate_backend(pid int)

   Description: Terminates a backend thread.

   Return type: boolean

   Note: Each of these functions returns **true** if they are successful and **false** otherwise.

   For example:

   ::

      SELECT pid from pg_stat_activity;
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
      (1 rows)

      SELECT pg_terminate_backend(140657876268816);
       pg_terminate_backend
      ----------------------
       t
      (1 row)
