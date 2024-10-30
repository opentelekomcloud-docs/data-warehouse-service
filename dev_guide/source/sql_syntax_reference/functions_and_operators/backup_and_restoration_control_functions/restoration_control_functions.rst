:original_name: dws_06_0301.html

.. _dws_06_0301:

Restoration Control Functions
=============================

Restoration control functions provide information about the status of standby nodes. These functions may be executed both during restoration and in normal running.

pg_is_in_recovery()
-------------------

Description: Returns **true** if restoration is still in progress.

Return type: bool

Example:

::

   SELECT pg_is_in_recovery();
    pg_is_in_recovery
   -------------------
    f
   (1 row)

pg_last_xlog_receive_location()
-------------------------------

Description: Gets the last transaction log location received and synchronized to disk by streaming replication. While streaming replication is in progress, this will increase monotonically. If restoration has completed, then this value will remain static at the value of the last WAL record received and synchronized to disk during restoration. If streaming replication is disabled or if not yet started, the function return will return **NULL**.

Return type: text

Example:

::

   SELECT pg_last_xlog_receive_location();
    pg_last_xlog_receive_location
   -------------------------------

   (1 row)

pg_last_xlog_replay_location()
------------------------------

Description: Gets last transaction log location replayed during restoration. If restoration is still in progress, this will increase monotonically. If restoration has completed, then this value will remain static at the value of the last WAL record received during that restoration. When the server has been started normally without restoration, the function returns **NULL**.

Return type: text

Example:

::

   SELECT pg_last_xlog_replay_location();
    pg_last_xlog_replay_location
   ------------------------------
    0/2B16530
   (1 row)

pg_last_xact_replay_timestamp()
-------------------------------

Description: Gets the timestamp of last transaction replayed during restoration. This is the time to commit a transaction or abort a WAL record on the primary node. If no transactions have been replayed during restoration, this function will return **NULL**. Otherwise, if restoration is still in progress, this will increase monotonically. If restoration has completed, then this value will remain static at the value of the last WAL record received during that restoration. If the server normally starts without manual intervention, this function will return **NULL**.

Return type: timestamp with time zone

Restoration control functions control restoration processes. These functions may be executed only during restoration.

Example:

::

   SELECT pg_last_xact_replay_timestamp();
    pg_last_xact_replay_timestamp
   -------------------------------
    2023-01-04 07:03:08.098024+00
   (1 row)

pg_is_xlog_replay_paused()
--------------------------

Description: Returns **true** if restoration is paused.

Return type: bool

pg_xlog_replay_pause()
----------------------

Description: Pauses restoration immediately.

Return type: void

pg_xlog_replay_resume()
-----------------------

Description: Restarts restoration if it was paused.

While restoration is paused, no further database changes are applied. In hot standby mode, all new queries will see the same consistent snapshot of the database, and no further query conflicts will be generated until restoration is resumed.

If streaming replication is disabled, the paused state may continue indefinitely without problem. While streaming replication is in progress, WAL records will continue to be received, which will eventually fill available disk space. This progress depends on the duration of the pause, the rate of WAL generation, and available disk space.

Return type: void

pg_xlog_replay_completion()
---------------------------

Description: Displays the progress of xlog redo on the current DN.

Return type: record

Example:

::

   SELECT * FROM pg_xlog_replay_completion();
    replay_start | replay_current | replay_end | replay_percent
   --------------+----------------+------------+----------------
    0/2ACAB80    | 0/2B16530      | 0/4F62B090 | 0%
   (1 row)

The following information is returned:

.. table:: **Table 1** pg_xlog_replay_completion() columns

   ============== ======= ======================================
   Column         Type    Description
   ============== ======= ======================================
   replay_start   integer Start LSN of xlog redo
   replay_current integer LSN of the current replay of xlog redo
   replay_end     integer Maximum LSN that requires xlog redo
   replay_percent integer Completion percentage of xlog redo
   ============== ======= ======================================

pg_data_sync_from_dummy_completion()
------------------------------------

Description: Displays the progress of data page file synchronization during the failover on the current DN.

Return type: record

Example:

::

   SELECT * FROM pg_data_sync_from_dummy_completion();
    start_index | current_index | total_index | sync_percent
   -------------+---------------+-------------+--------------
              0 |             0 |           0 | 100%
   (1 row)

The following information is returned:

.. table:: **Table 2** pg_data_sync_from_dummy_completion() columns

   ============= ======= =============================================
   Column        Type    Description
   ============= ======= =============================================
   start_index   integer Start LSN of data page file synchronization
   current_index integer Current LSN of data page file synchronization
   total_index   integer Maximum LSN of data page file synchronization
   sync_percent  integer Completion percentage of data page files
   ============= ======= =============================================

gs_roach_stop_backup(backupid text)
-----------------------------------

Description: Stops a backup started by the internal backup tool GaussRoach and returns the position where the current log is inserted. This function is similar to **pg_stop_backup**, but is more lightweight.

Return type: text

gs_roach_enable_delay_ddl_recycle(backupid name)
------------------------------------------------

Description: Enables DDL delay and returns the log position of the enabling point. This function is similar to **pg_enable_delay_ddl_recycle**, but is more lightweight. In addition, this function allows you to enable DDL delay for multiple backups.

Return type: text

gs_roach_disable_delay_ddl_recycle(backupid text)
-------------------------------------------------

Description: Disables DDL delay, returns the logs for which DDL delay takes effect, and deletes the physical files of the column-store tables that have been deleted by the user. This function is similar to **pg_enable_delay_ddl_recycle**, but is more lightweight. In addition, this function allows you to disable DDL delay for multiple backups.

Return type: record

gs_roach_switch_xlog(request_ckpt bool)
---------------------------------------

Description: Switches the currently used log segment file and returns the position of the segment log. If the value of **request_ckpt** is **true**, a full check point is triggered.

Return type: text

pg_resume_bkp_flag(backupid name)
---------------------------------

Description: Resumes the delay xlog flag from a specified backup and returns **start_backup_flag boolean**, **to_delay boolean**, **ddl_delay_recycle_ptr text**, and **rewind_time text**.

Return type: record
