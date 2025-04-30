:original_name: dws_04_1074.html

.. _dws_04_1074:

Synchronizing Hudi Tasks
========================

Creating a Hudi Task
--------------------

**Migration**

If data has been imported to the GaussDB(DWS) table using CDL, use SQL on Hudi to migrate data. Alternatively, use CDM to perform full initialization and then use SQL on Hudi to synchronize incremental data.

#. To create the **hudi.hudi_sync_state** synchronization status table, you must have the administrator permission.

   ::

      SELECT pg_catalog.create_hudi_sync_table();

   Generally, hudi.hudi_sync_state is created only once in each database.

#. To set the CDL synchronization progress, you must have the INSERT and UPDATE permissions on the target table and the SELECT permission on the HUDI foreign table. Otherwise, the synchronization progress cannot be set.

   ::

      SELECT hudi_set_sync_commit('SCHEMA.TABLE', 'SCHEMA.FOREIGN_TABLE', 'LATEST_COMMIT');

   Where:

   -  **SCHEMA.TABLE** indicates the name and schema of the target table for data synchronization.
   -  **SCHEMA.FOREIGN_TABLE** indicates the name and schema of the OBS foreign table.
   -  **LATEST_COMMIT** indicates the end time of the Hudi synchronization.

   Example: Data has been synchronized to the target table **public.in_rel** from hudi by **20220913152131**. Use SQL on Hudi to continue to export data from the OBS foreign table **hudi_read1**.

   ::

      SELECT hudi_set_sync_commit('public.in_rel', 'public.hudi_read1', '20220913152131');

#. Submit the Hudi synchronization task.

   ::

      SELECT hudi_sync_task_submit('SCHEMA.TABLE', 'SCHEMA.FOREIGN_TABLE');

   Example: Use SQL on Hudi to continue to export data from the OBS foreign table **hudi_read1** to the target table **public.in_rel**.

   ::

      SELECT hudi_sync_task_submit('public.in_rel', 'public.hudi_read1');

**Creation**

If the GaussDB(DWS) table is empty and data is synchronized from Hudi for the first time, run the following command to create a task:

::

   SELECT hudi_sync_task_submit('SCHEMA.TABLE', 'SCHEMA.FOREIGN_TABLE');

Querying Hudi Synchronization Tasks
-----------------------------------

Query a Hudi synchronization task. In the query result, **task_id uniquely** identifies a Hudi synchronization task.

::

   SELECT * FROM pg_task_show('SQLonHudi');

Suspending Hudi Synchronization Tasks
-------------------------------------

Query the Hudi task and obtain **task_id** to suspend the Hudi task.

::

   SELECT pg_task_pause('task_id');

Example:

Suspend the synchronization task whose **task_id** is 6\ **4479410-a04c-0700-d150-3037d700fffe**.

::

   SELECT pg_task_pause('64479410-a04c-0700-d150-3037d700fffe');

Resuming Hudi Synchronization Tasks
-----------------------------------

Query the Hudi task, obtain the value of **task_id**, and resume the Hudi task.

::

   SELECT pg_task_resume('task_id');

Example:

Resume the synchronization task whose **task_id** is **64479410-a04c-0700-d150-3037d700fffe**.

::

   SELECT pg_task_resume('64479410-a04c-0700-d150-3037d700fffe');

Deleting a Hudi Synchronization Task
------------------------------------

Query the Hudi task, obtain **task_id**, and delete the Hudi synchronization task.

::

   SELECT pg_task_remove('task_id');

Example:

Delete the synchronization task whose **task_id** is **64479410-a04c-0700-d150-3037d700fffe**.

::

   SELECT pg_task_remove('64479410-a04c-0700-d150-3037d700fffe');

Querying Past Synchronization Information
-----------------------------------------

Use the **hudi_sync_state_history_view** view to query information about past Hudi synchronization tasks. This view is supported only by clusters of version 9.1.0 and later.

::

   SELECT * FROM pg_catalog.hudi_sync_state_history_view;

.. table:: **Table 1** **hudi_sync_state_history_view** columns

   +---------------------+--------------------------+-------------------------------------------------------------+
   | Column              | Type                     | Description                                                 |
   +=====================+==========================+=============================================================+
   | task_id             | TEXT                     | Task ID                                                     |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | target_tbl          | TEXT                     | Name of the synchronization target table                    |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | source_ftbl         | TEXT                     | Name of the synchronization source table (foreign table)    |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | latest_commit       | TEXT                     | Timestamp of the latest successful synchronization          |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | latest_sync_count   | BIGINT                   | Number of rows that are successfully synchronized last time |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | latest_sync_start   | TIMESTAMP WITH TIME ZONE | Start time of the latest synchronization task               |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | latest_sync_end     | TIMESTAMP WITH TIME ZONE | Time when the latest synchronization task ends              |
   +---------------------+--------------------------+-------------------------------------------------------------+
   | hudi_flushdisk_time | TEXT                     | Time when the hudi file is flushed to disks                 |
   +---------------------+--------------------------+-------------------------------------------------------------+

Querying the Status of a Synchronization Task
---------------------------------------------

Use the **hudi_show_sync_state()** function to query the status of a Hudi synchronization task.

::

   SELECT * FROM hudi_show_sync_state();

Resetting a Hudi Synchronization Task with Consecutive Failures
---------------------------------------------------------------

Use the **pg_task_resume()** function to reset a Hudi synchronization task that fails consecutively.

If the number of consecutive failures is greater than or equal to 10, the task is automatically suspended. You need to manually call the **pg_task_resume()** function to reset the task. This function is supported only by clusters of version 9.1.0 and later.

Input parameter: **task_id** of the Hudi task that fails consecutively

::

   SELECT pg_task_resume('task_id');
