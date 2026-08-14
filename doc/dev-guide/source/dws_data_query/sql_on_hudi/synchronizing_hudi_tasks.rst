:original_name: dws_04_1074.html

.. _dws_04_1074:

Synchronizing Hudi Tasks
========================

Creating a Hudi Task
--------------------

**Migration**

#. To create the **hudi.hudi_sync_state** synchronization status table, you must have the administrator permission.

   ::

      SELECT pg_catalog.create_hudi_sync_table();

   Generally, hudi.hudi_sync_state is created only once in each database.

#. To set the synchronization progress, you must have the INSERT and UPDATE permissions on the target table and the SELECT permission on the Hudi foreign table. Otherwise, the synchronization progress cannot be set.

   ::

      SELECT hudi_set_sync_commit('SCHEMA.TABLE', 'SCHEMA.FOREIGN_TABLE', 'LATEST_COMMIT');

   Where:

   -  **SCHEMA.TABLE** indicates the name of the table to which data is synchronized. **schema** indicates the schema name.
   -  **SCHEMA.FOREIGN_TABLE** indicates the name of the OBS foreign table. **schema** indicates the schema name.
   -  **LATEST_COMMIT** indicates the end time of the Hudi synchronization.

   Example: Data has been synchronized to the target table **public.in_rel** from Hudi by **20220913152131**. Use SQL on Hudi to continue to export data from the OBS foreign table **hudi_read1**.

   ::

      SELECT hudi_set_sync_commit('public.in_rel', 'public.hudi_read1', '20220913152131');

#. Submit the Hudi synchronization task.

   ::

      SELECT hudi_sync_task_submit('SCHEMA.TABLE', 'SCHEMA.FOREIGN_TABLE');

   Example: Use SQL on Hudi to continue to export data from the OBS foreign table **hudi_read1** to the target table **public.in_rel**.

   ::

      SELECT hudi_sync_task_submit('public.in_rel', 'public.hudi_read1');

**Creation**

If the DWS table is empty and data is synchronized from Hudi for the first time, run the following command to create a task:

::

   SELECT hudi_sync_task_submit('SCHEMA.TABLE', 'SCHEMA.FOREIGN_TABLE');

Querying Hudi Synchronization Tasks
-----------------------------------

Querying Hudi Synchronization Tasks

Syntax:

::

   SELECT * FROM pg_task_show('SQLonHudi');

**task_id** in the query result is the unique ID of the Hudi synchronization task.

Example:

::

   SELECT * FROM pg_task_show('SQLonHudi');
                 task_id                |                                                                                          what                                                                                           | category_id | userid | is_broken |  interval  | time_cons |          start_time           | end_time | parameter | task_rank |        next_start_time        |         next_end_time         | last_log | failure_times
   --------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+--------+-----------+------------+-----------+-------------------------------+----------+-----------+-----------+-------------------------------+-------------------------------+----------+---------------
    64d257e9-1e9b-0d00-3ce3-7e61b5e0fffe | call pg_catalog.hudi_sync_custom('public.hudi_read_target', 'public.hudi_read101', '{"_hoodie_commit_seqno" : "_hoodie_commit_seqno", "id" : "id", "ts" : "ts", "long_field" : "ts"}'); | SQLonHudi   |     10 | f         | '00:00:10' |           | 2023-08-08 22:58:15.846903+08 |          |           |         5 | 2023-08-08 22:58:15.846903+08 | 2023-08-08 22:58:24.846903+08 |          |             0
   (1 row)

Suspending Hudi Synchronization Tasks
-------------------------------------

Suspend a Hudi synchronization task by specified **task_id**.

Syntax:

::

   SELECT pg_task_pause('task_id');

Example:

Suspend the synchronization task whose **task_id** is 6\ **4479410-a04c-0700-d150-3037d700fffe**.

::

   SELECT pg_task_pause('64479410-a04c-0700-d150-3037d700fffe');

Resuming Hudi Synchronization Tasks
-----------------------------------

Resume a Hudi synchronization task by specifying **task_id**.

Syntax:

::

   SELECT pg_task_resume('task_id');

Example:

Resume the synchronization task whose **task_id** is **64479410-a04c-0700-d150-3037d700fffe**.

::

   SELECT pg_task_resume('64479410-a04c-0700-d150-3037d700fffe');

Resetting a Hudi Synchronization Task with Consecutive Failures
---------------------------------------------------------------

Reset a Hudi synchronization task with consecutive failures by specifying **task_id**.

If the number of consecutive failures is greater than or equal to 10, the task is automatically suspended. You need to manually call the **pg_task_resume()** function to reset the task.

::

   SELECT pg_task_resume('task_id');

Deleting a Hudi Synchronization Task
------------------------------------

Delete a Hudi synchronization task by specifying **task_id**.

Syntax:

::

   SELECT pg_task_remove('task_id');

Example:

Delete the synchronization task whose **task_id** is **64479410-a04c-0700-d150-3037d700fffe**.

::

   SELECT pg_task_remove('64479410-a04c-0700-d150-3037d700fffe');

Querying Historical Hudi Synchronization Task Information
---------------------------------------------------------

Use the **hudi_sync_state_history_view** view to query information about past Hudi synchronization tasks. This view is supported only by clusters of version 9.1.0 and later.

::

   SELECT * FROM pg_catalog.hudi_sync_state_history_view;

.. table:: **Table 1** **hudi_sync_state_history_view** columns

   +---------------------+--------------------------+-------------------------------------------------------------+
   | Column              | Type                     | Description                                                 |
   +=====================+==========================+=============================================================+
   | task_id             | text                     | Task ID                                                     |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | target_tbl          | text                     | Name of the synchronization target table                    |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | source_ftbl         | text                     | Name of the synchronization source table (foreign table)    |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | latest_commit       | text                     | Timestamp of the latest successful synchronization          |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | latest_sync_count   | bigint                   | Number of rows that are successfully synchronized last time |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | latest_sync_start   | timestamp with time zone | Start time of the latest synchronization task               |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | latest_sync_end     | timestamp with time zone | Time when the latest synchronization task ends              |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | hudi_flushdisk_time | text                     | Time when the hudi file is flushed to disks                 |
   +---------------------+--------------------------+-------------------------------------------------------------+

Querying the Status of a Hudi Synchronization Task
--------------------------------------------------

Use the **hudi_show_sync_state()** function to query the status of a Hudi synchronization task.

::

   SELECT * FROM hudi_show_sync_state();
