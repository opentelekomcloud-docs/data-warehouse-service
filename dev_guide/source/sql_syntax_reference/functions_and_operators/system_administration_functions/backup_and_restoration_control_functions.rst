:original_name: dws_06_0056.html

.. _dws_06_0056:

Backup and Restoration Control Functions
========================================

Backup Control Functions
------------------------

Backup control functions help online backup.

-  pg_create_restore_point(name text)

   Description: Creates a named point for performing the restore operation (restricted to system administrators).

   Return type: text

   Note: **pg_create_restore_point** creates a named transaction log record that can be used as a restoration target, and returns the corresponding transaction log location. The given name can then be used with **recovery_target_name** to specify the point up to which restoration will proceed. Avoid creating multiple restoration points with the same name, since restoration will stop at the first one whose name matches the restoration target.

-  pg_current_xlog_location()

   Description: Obtains the write position of the current transaction log.

   Return type: text

   Note: **pg_current_xlog_location** displays the write position of the current transaction log in the same format as those of the previous functions. Read-only operations do not require rights of the system administrator.

-  pg_current_xlog_insert_location()

   Description: Obtains the insert position of the current transaction log.

   Return type: text

   Note: **pg_current_xlog_insert_location** displays the insert position of the current transaction log. The insertion point is the logical end of the transaction log at any instant, while the write location is the end of what has been written out from the server's internal buffers. The write position is the end that can be detected externally from the server. This operation can be performed to archive only some of completed transaction log files. The insert position is mainly used for commissioning the server. Read-only operations do not require rights of the system administrator.

-  pg_start_backup(label text [, fast boolean ])

   Description: Starts executing online backup (restricted to system administrators or replication roles).

   Return type: text

   Note: **pg_start_backup** receives a user-defined backup label (usually the name of the position where the backup dump file is stored). This function writes a backup label file to the data directory of the database cluster and then returns the starting position of backed up transaction logs in text mode.

   ::

      SELECT pg_start_backup('label_goes_here');
       pg_start_backup
      -----------------
       0/3000020
      (1 row)

-  pg_stop_backup()

   Description: Completes online backup (restricted to system administrators or replication roles).

   Return type: text

   Note: **pg_stop_backup** deletes the label file created by **pg_start_backup** and creates a backup history file in the transaction log archive area. The history file includes the label given to **pg_start_backup**, the starting and ending transaction log locations for the backup, and the starting and ending times of the backup. The return value is the backup's ending transaction log location. After the ending position is calculated, the insert position of the current transaction log automatically goes ahead to the next transaction log file. This way, the ended transaction log file can be immediately archived so that backup is complete.

-  pg_switch_xlog()

   Description: Switches to a new transaction log file (restricted to system administrators).

   Return type: text

   Note: **pg_switch_xlog** moves to the next transaction log file so that the current log file can be archived (if continuous archive is used). The return value is the ending transaction log location + 1 within the just-completed transaction log file. If there has been no transaction log activity since the last transaction log switchover, **pg_switch_xlog** will do nothing but return the start location of the transaction log file currently in use.

-  pg_xlogfile_name(location text)

   Description: Converts the position string in a transaction log to a file name.

   Return type: text

   Note: **pg_xlogfile_name** extracts only the transaction log file name. If the given transaction log position is the transaction log file border, a transaction log file name will be returned for both the two functions. This is usually the desired behavior for managing transaction log archiving, since the preceding file is the last one that currently needs to be archived.

-  pg_xlogfile_name_offset(location text)

   Description: Converts the position string in a transaction log to a file name and returns the byte offset in the file.

   Return type: text, integer

   Note: **pg_xlogfile_name_offset** can extract transaction log file names and byte offsets from the returned results of the preceding functions. For example:

   ::

      SELECT * FROM pg_xlogfile_name_offset(pg_stop_backup());
      NOTICE:  pg_stop_backup cleanup done, waiting for required WAL segments to be archived
      NOTICE:  pg_stop_backup complete, all required WAL segments have been archived
              file_name         | file_offset
      --------------------------+-------------
      000000010000000000000003  |         272
      (1 row)

-  pg_xlog_location_diff(location text, location text)

   Description: **pg_xlog_location_diff** calculates the difference in bytes between two transaction log locations.

   Return type: numeric

-  pg_cbm_tracked_location()

   Description: Queries for the LSN location parsed by CBM.

   Return type: text

-  pg_cbm_get_merged_file(startLSNArg text, endLSNArg text)

   Description: Combines CBM files within the specified LSN range into one and returns the name of the combined file.

   Return type: text

-  pg_cbm_get_changed_block(startLSNArg text, endLSNArg text)

   Description: Combines CBM files within the specified LSN range into a table and return records of this table.

   Return type: record

   Note: The table columns include the start LSN, end LSN, tablespace OID, database OID, table relfilenode, table fork number, whether the table is deleted, whether the table is created, whether the table is truncated, number of pages in the truncated table, number of modified pages, and list of No. of modified pages.

-  pg_cbm_recycle_file(slotName name, targetLSNArg text)

   Description: Deletes the CBM files that are no longer used and returns the first LSN after the deletion. If **slotName** is empty, **targetLSNArg** is used as the recycling point. During backup and DR, you need to specify a slot name due to parallelism. Record the **targetLSNArg** value of the task to the slot, traverse all backup slots, and find the smallest LSN as the recycling point.

   Return type: text

-  pg_cbm_force_track(targetLSNArg text,timeOut int)

   Description: Forcibly executes the CBM trace to the specified Xlog position and returns the Xlog position of the actual trace end point.

   Return type: text

-  pg_enable_delay_ddl_recycle()

   Description: Enables DDL delay and returns the Xlog position of the enabling point.

   Return type: text

-  pg_disable_delay_ddl_recycle(barrierLSNArg text, isForce bool)

   Description: Disables DDL delay and returns the Xlog range where DDL delay takes effect.

   Return type: record

-  pg_enable_delay_xlog_recycle()

   Description: Enables Xlog recycle delay.

   Return type: void

-  pg_disable_delay_xlog_recycle()

   Description: Disables Xlog recycle delay.

   Return type: void

-  pgxc_get_senders_catchup_time()

   Description: Displays the catchup information of the currently active primary/standby instance sending thread on all DNs.

   Return type: record

   The following information is returned:

   .. table:: **Table 1** pgxc_get_senders_catchup_time() columns

      +------------------------+--------------------------+------------------------------------------------------------+
      | Name                   | Type                     | Description                                                |
      +========================+==========================+============================================================+
      | node_name              | text                     | Node name                                                  |
      +------------------------+--------------------------+------------------------------------------------------------+
      | lwpid                  | integer                  | Current sender lwpid                                       |
      +------------------------+--------------------------+------------------------------------------------------------+
      | local_role             | text                     | Local role                                                 |
      +------------------------+--------------------------+------------------------------------------------------------+
      | peer_role              | text                     | Peer role                                                  |
      +------------------------+--------------------------+------------------------------------------------------------+
      | state                  | text                     | Current sender's replication status                        |
      +------------------------+--------------------------+------------------------------------------------------------+
      | sender                 | text                     | Current sender type                                        |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_start          | timestamp with time zone | Startup time of a catchup task                             |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_end            | timestamp with time zone | End time of a catchup task                                 |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_type           | text                     | Catchup task type, full or incremental                     |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_bcm_filename   | text                     | BCM file executed by the current catchup task              |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_bcm_finished   | integer                  | Number of BCM files completed by a catchup task            |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_bcm_total      | integer                  | Total number of BCM files to be operated by a catchup task |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_percent        | text                     | Completion percentage of a catchup task                    |
      +------------------------+--------------------------+------------------------------------------------------------+
      | catchup_remaining_time | text                     | Estimated remaining time of a catchup task                 |
      +------------------------+--------------------------+------------------------------------------------------------+

Restoration Control Functions
-----------------------------

Restoration control functions provide information about the status of standby nodes. These functions may be executed both during restoration and in normal running.

-  pg_is_in_recovery()

   Description: Returns **true** if restoration is still in progress.

   Return type: bool

-  pg_last_xlog_receive_location()

   Description: Gets the last transaction log location received and synchronized to disk by streaming replication. While streaming replication is in progress, this will increase monotonically. If restoration has completed, then this value will remain static at the value of the last WAL record received and synchronized to disk during restoration. If streaming replication is disabled or if not yet started, the function return will return **NULL**.

   Return type: text

-  pg_last_xlog_replay_location()

   Description: Gets last transaction log location replayed during restoration. If restoration is still in progress, this will increase monotonically. If restoration has completed, then this value will remain static at the value of the last WAL record received during that restoration. When the server has been started normally without restoration, the function returns **NULL**.

   Return type: text

-  pg_last_xact_replay_timestamp()

   Description: Gets the timestamp of last transaction replayed during restoration. This is the time to commit a transaction or abort a WAL record on the primary node. If no transactions have been replayed during restoration, this function will return **NULL**. Otherwise, if restoration is still in progress, this will increase monotonically. If restoration has completed, then this value will remain static at the value of the last WAL record received during that restoration. If the server normally starts without manual intervention, this function will return **NULL**.

   Return type: timestamp with time zone

Restoration control functions control restoration processes. These functions may be executed only during restoration.

-  pg_is_xlog_replay_paused()

   Description: Returns **true** if restoration is paused.

   Return type: bool

-  pg_xlog_replay_pause()

   Description: Pauses restoration immediately.

   Return type: void

-  pg_xlog_replay_resume()

   Description: Restarts restoration if it was paused.

   Return type: void

While restoration is paused, no further database changes are applied. In hot standby mode, all new queries will see the same consistent snapshot of the database, and no further query conflicts will be generated until restoration is resumed.

If streaming replication is disabled, the paused state may continue indefinitely without problem. While streaming replication is in progress, WAL records will continue to be received, which will eventually fill available disk space. This progress depends on the duration of the pause, the rate of WAL generation, and available disk space.

-  pg_xlog_replay_completion()

   Description: Displays the progress of xlog redo on the current DN.

   Return type: record

   The following information is returned:

   .. table:: **Table 2** pg_xlog_replay_completion() columns

      ============== ======= ======================================
      Column         Type    Description
      ============== ======= ======================================
      replay_start   integer Start LSN of xlog redo
      replay_current integer LSN of the current replay of xlog redo
      replay_end     integer Maximum LSN that requires xlog redo
      replay_percent integer Completion percentage of xlog redo
      ============== ======= ======================================

-  pg_data_sync_from_dummy_completion()

   Description: Displays the progress of data page file synchronization during the failover on the current DN.

   Return type: record

   The following information is returned:

   .. table:: **Table 3** pg_data_sync_from_dummy_completion() columns

      ============= ======= =============================================
      Column        Type    Description
      ============= ======= =============================================
      start_index   integer Start LSN of data page file synchronization
      current_index integer Current LSN of data page file synchronization
      total_index   integer Maximum LSN of data page file synchronization
      sync_percent  integer Completion percentage of data page files
      ============= ======= =============================================

-  gs_roach_stop_backup(backupid text)

   Description: Stops a backup started by the internal backup tool GaussRoach and returns the position where the current log is inserted. This function is similar to **pg_stop_backup**, but is more lightweight.

   Return type: text

-  gs_roach_enable_delay_ddl_recycle(backupid name)

   Description: Enables DDL delay and returns the log position of the enabling point. This function is similar to **pg_enable_delay_ddl_recycle**, but is more lightweight. In addition, this function allows you to enable DDL delay for multiple backups.

   Return type: text

-  gs_roach_disable_delay_ddl_recycle(backupid text)

   Description: Disables DDL delay, returns the logs for which DDL delay takes effect, and deletes the physical files of the column-store tables that have been deleted by the user. This function is similar to **pg_enable_delay_ddl_recycle**, but is more lightweight. In addition, this function allows you to disable DDL delay for multiple backups.

   Return type: record

-  gs_roach_switch_xlog(request_ckpt bool)

   Description: Switches the currently used log segment file and returns the position of the segment log. If the value of **request_ckpt** is **true**, a full check point is triggered.

   Return type: text

-  pg_resume_bkp_flag(backupid name)

   Description: Resumes the delay xlog flag from a specified backup and returns **start_backup_flag boolean**, **to_delay boolean**, **ddl_delay_recycle_ptr text**, and **rewind_time text**.

   Return type: record
