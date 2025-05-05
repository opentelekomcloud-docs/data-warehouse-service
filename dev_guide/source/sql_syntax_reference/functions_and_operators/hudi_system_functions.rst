:original_name: dws_06_0359.html

.. _dws_06_0359:

Hudi System Functions
=====================

Hudi system functions are supported only by 8.2.1.100 and later versions.

pg_show_custom_settings()
-------------------------

Description: Queries details about Hudi foreign table parameter settings.

Return type: SETOF record

Example:

::

   SELECT * FROM pg_show_custom_settings();
                           name                        |      setting      | unit |      category      |        short_desc        | extra_desc | context | vartype | source  | min_val | max_val |
    enumvals | boot_val | reset_val | sourcefile | sourceline
   ----------------------------------------------------+-------------------+------+--------------------+--------------------------+------------+---------+---------+---------+---------+---------+
   ----------+----------+-----------+------------+------------
    hoodie.public.hudi_mor_ft.consume.ending.timestamp | 20230404172329544 |      | Customized Options | GUC placeholder variable |            | user    | string  | session |         |         |
             |          |           |            |
    hoodie.public.hudi_mor_ft.consume.mode             | incremental       |      | Customized Options | GUC placeholder variable |            | user    | string  | session |         |         |
             |          |           |            |
    hoodie.public.hudi_mor_ft.consume.start.timestamp  | 20230404172329543 |      | Customized Options | GUC placeholder variable |            | user    | string  | session |         |         |
             |          |           |            |
   (3 rows)

hudi_get_options(regclass)
--------------------------

Description: Queries the properties of a Hudi foreign table (hoodie.properties). It is represented by a key-value pair.

Return type: SETOF record

Example:

::

   select * from hudi_get_options('public.hudi_mor_ft');
                          key                       |
                                                                                                                                                     value


   -------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------
   -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   --------------------------------------------------------
    hoodie.table.precombine.field                   | col_int
    hoodie.datasource.write.drop.partition.columns  | false
    hoodie.table.partition.fields                   |
    hoodie.table.type                               | MERGE_ON_READ
    hoodie.archivelog.folder                        | archived
    hoodie.compaction.payload.class                 | org.apache.hudi.common.model.OverwriteWithLatestAvroPayload
    hoodie.timeline.layout.version                  | 1
    hoodie.table.version                            | 4
    hoodie.table.recordkey.fields                   | col_bigint
    hoodie.database.name                            | default
    hoodie.datasource.write.partitionpath.urlencode | false
    hoodie.table.name                               | lt_test_mor_014
    hoodie.table.keygenerator.class                 | org.apache.hudi.keygen.ComplexKeyGenerator
    hoodie.datasource.write.hive_style_partitioning | true
    hoodie.table.create.schema                      | {"type"\:"record","name"\:"lt_test_mor_014_record","namespace"\:"hoodie.lt_test_mor_014","fields"\:[{"name"\:"_hoodie_commit_time","type"\:[
   "string","null"]},{"name"\:"_hoodie_commit_seqno","type"\:["string","null"]},{"name"\:"_hoodie_record_key","type"\:["string","null"]},{"name"\:"_hoodie_partition_path","type"\:["string","null
   "]},{"name"\:"_hoodie_file_name","type"\:["string","null"]},{"name"\:"col_bigint","type"\:["long","null"]},{"name"\:"col_int","type"\:["int","null"]},{"name"\:"col_text","type"\:["string","nu
   ll"]},{"name"\:"col_text2","type"\:["string","null"]}]}
    hoodie.table.checksum                           | 515660817
   (16 rows)

hudi_get_max_commit(regclass)
-----------------------------

Description: This function retrieves the most recent commit timestamp and data write time for the current Hudi external table.

Return type: record

Example:

::

   SELECT * FROM hudi_get_max_commit('public.hudi_mor_ft');
      max_commit   |       write_time
   ----------------+------------------------
    20221207141822 | 2022-12-07 14:18:30+08
   (1 row)

hudi_get_commit(regclass, cstring, int)
---------------------------------------

Description: This function retrieves the timestamp and write time of the commit data for the current Hudi foreign table, starting from the specified commit and up to the *N*\ th commit. If the *N*\ th commit does not exist, the latest commit and the corresponding data write time are returned. This function is supported only by 9.1.0.100 and later versions.

Return type: record

Example:

::

   SELECT * FROM hudi_get_commit('public.hudi_mor_ft', '20230329174744657', 3);
       end_commit     |       write_time
   -------------------+------------------------
    20230329174808908 | 2023-08-31 15:43:08+08
   (1 row)

hudi_sync_task_submit(regclass, regclass)
-----------------------------------------

Description: Submits an automatic Hudi synchronization task. The first input parameter is the synchronization target table, and the second input parameter is the Hudi foreign table. If the task is submitted successfully, the task ID is returned.

Return type: text

.. note::

   -  The synchronization target table must contain a primary key, which must be the same as the value of **hudi recordkey**.
   -  If the hudi table contains the **precombine** field, the synchronization target table must also contain the corresponding field.
   -  If the synchronization target table contains only the primary key (no other fields except the primary key), the synchronization task cannot be submitted normally.
   -  The user must have the **INSERT** and **UPDATE** permissions on the target table and the **SELECT** permission on the Hudi foreign table. Otherwise, the synchronization task cannot be submitted.

Example:

::

   SELECT hudi_sync_task_submit('public.hudi_sync_i','public.hudi_mor_ft');
           hudi_sync_task_submit
   --------------------------------------
    6465efe2-3ea1-0b00-dde5-b57dfb30fffe
   (1 row)

hudi_sync_task_submit(regclass, regclass, interval)
---------------------------------------------------

Description: The function is the same as that of **hudi_sync_task_submit(regclass, regclass)**. The difference is that, in this function, you can specify an input parameter of the interval type to specify the task scheduling period. The value ranges from 5 seconds to 24 hours. If the task is submitted successfully, the task ID is returned. This function is supported only by 8.3.0 and later versions.

Return type: text

.. note::

   -  The synchronization target table must contain a primary key, which must be the same as the value of **hudi recordkey**.
   -  If the hudi table contains the **precombine** field, the synchronization target table must also contain the corresponding field.
   -  If the synchronization target table contains only the primary key (no other fields except the primary key), the synchronization task cannot be submitted normally.
   -  The user must have the **INSERT** and **UPDATE** permissions on the target table and the **SELECT** permission on the Hudi foreign table. Otherwise, the synchronization task cannot be submitted.

Example:

::

   SELECT hudi_sync_task_submit('public.hudi_sync_i','public.hudi_mor_ft','1 hour');
           hudi_sync_task_submit
   --------------------------------------
    6465efe2-3ea1-0b00-dde5-b57dfb30fffe
   (1 row)

hudi_sync_task_submit(regclass, regclass, text, text)
-----------------------------------------------------

Description: The function is the same as that of **hudi_sync_task_submit(regclass, regclass)**. The difference is that you can specify two additional text input parameters to indicate the fields that you expect to be synchronized. The fields are separated by commas (,). Quotation marks and escape characters can be parsed. The number and sequence of fields in the two text parameters must be the same, because the synchronization will be performed based on the mapping between the fields in the two text parameters. If the task is submitted successfully, the task ID is returned.

Return type: text

.. note::

   -  The synchronization target table must contain a primary key, which must be the same as the value of **hudi recordkey**.
   -  If the hudi table contains the **precombine** field, the synchronization target table must also contain the corresponding field.
   -  If the synchronization target table contains only the primary key (no other fields except the primary key), the synchronization task cannot be submitted normally.
   -  The user must have the **INSERT** and **UPDATE** permissions on the target table and the **SELECT** permission on the Hudi foreign table. Otherwise, the synchronization task cannot be submitted.

Example:

::

   SELECT hudi_sync_task_submit('public.hudi_sync_i','public.hudi_mor_ft','_hoodie_commit_time, col_bigint, col_text', '_hoodie_commit_time, col_bigint, col_text');
           hudi_sync_task_submit
   --------------------------------------
    646610bc-cdd1-0d00-d07d-b57e89a0fffe
   (1 row)

hudi_sync_task_submit(regclass, regclass, text, text, interval)
---------------------------------------------------------------

Description: The function is the same as that of **hudi_sync_task_submit(regclass, regclass, text, text)**. The difference is that you can specify an additional input parameter of the interval type to specify the task scheduling period. The value ranges from 5 seconds to 24 hours. This function is supported only by 8.3.0 and later versions.

Return type: text

.. note::

   -  The synchronization target table must contain a primary key, which must be the same as the value of **hudi recordkey**.
   -  If the hudi table contains the **precombine** field, the synchronization target table must also contain the corresponding field.
   -  If the synchronization target table contains only the primary key (no other fields except the primary key), the synchronization task cannot be submitted normally.
   -  The user must have the **INSERT** and **UPDATE** permissions on the target table and the **SELECT** permission on the Hudi foreign table. Otherwise, the synchronization task cannot be submitted.

Example:

::

   SELECT hudi_sync_task_submit('public.hudi_sync_i','public.hudi_mor_ft','_hoodie_commit_time, col_bigint, col_text', '_hoodie_commit_time, col_bigint, col_text', '10 minute 30second');
           hudi_sync_task_submit
   --------------------------------------
    646610bc-cdd1-0d00-d07d-b57e89a0fffe
   (1 row)

hudi_show_sync_state()
----------------------

Description: Obtains the synchronization status of the Hudi automatic synchronization task.

Return type: SETOF record

Example:

::

   SELECT * FROM hudi_show_sync_state();
        target_tbl     |    source_ftbl     |                        payload_type                         | precombine_key |   latest_commit
   --------------------+--------------------+-------------------------------------------------------------+----------------+-------------------
    public.hudi_sync_i | public.hudi_mor_ft | org.apache.hudi.common.model.OverwriteWithLatestAvroPayload | col_int        | 20230511114021573
   (1 row)

hudi_sync(regclass, regclass)
-----------------------------

Description: Stored procedure, which is invoked by the Hudi automatic synchronization task. Tasks submitted using **pg_catalog.hudi_sync_task_submit(regclass, regclass)** will execute the stored procedure. Upon successful execution of the command, the number of synchronized rows and timestamp will be displayed.

Return type: text

Example:

::

   CALL hudi_sync('public.hudi_sync_i', 'public.hudi_mor_ft');
   NOTICE:  execute full sync
   CONTEXT:  PL/pgSQL function hudi_sync(regclass,regclass) line 11 at RETURN
                 hudi_sync
   --------------------------------------
    sync 1 rows up to 20230511114021573.
   (1 row)

hudi_sync_custom(regclass, regclass, text)
------------------------------------------

Description: A stored procedure, which is the entry for invoking the Hudi automatic synchronization task. Users can customize the mapping between the fields in the target table and those in the data source table. Tasks submitted using **pg_catalog.hudi_sync_task_submit(regclass, regclass, text, text)** will execute the stored procedure. In the preceding information, **text** is a JSON string, indicating the synchronization mapping between two table fields. Upon successful execution of the command, the number of synchronized rows and timestamp will be displayed.

Return type: text

Example:

::

   CALL hudi_sync_custom('public.hudi_sync_i', 'public.hudi_mor_ft', '{"_hoodie_commit_time" : "_hoodie_commit_time", "col_bigint" : "col_bigint", "col_text" : "col_text"}');
   NOTICE:  execute full sync
   CONTEXT:  PL/pgSQL function hudi_sync_custom(regclass,regclass,text) line 14 at RETURN
              hudi_sync_custom
   --------------------------------------
    sync 1 rows up to 20230511114021573.
   (1 row)

hudi_set_sync_commit(regclass, regclass, text)
----------------------------------------------

Description: Sets the start timestamp of the first synchronization of the Hudi automatic synchronization task to prevent resynchronization. The first parameter is the synchronization target table, the second parameter is the Hudi foreign table, and the third parameter is the expected synchronization start point. This function must be used before a synchronization task is submitted. This function is supported only by 8.2.1.210 and later versions.

Return type: text

Example:

::

   select hudi_set_sync_commit('public.hudi_sync_i', 'public.hudi_mor_ft', '20230511114021573');
   NOTICE:  set sync commit successfully, the next synchronization will start from 20230511114021573
   CONTEXT:  referenced column: hudi_set_sync_commit
    hudi_set_sync_commit
   ----------------------
    20230511114021573
   (1 row)

.. note::

   You must have the **INSERT** and **UPDATE** permissions on the target table and the **SELECT** permission on the Hudi foreign table. Otherwise, you cannot set the synchronization progress.

hudi_set_sync_commit(text, text)
--------------------------------

Description: Sets the start timestamp of the next synchronization of a Hudi automatic synchronization task. You can use it to sync historical data again or to skip some data. The first parameter is the task ID, and the second parameter is the expected start time of the next synchronization. This function can be used only after a synchronization task is submitted. Before using this function, you need to pause the task. This function is supported only by 8.2.1.210 and later versions.

Return type: text

Example:

::

   select hudi_set_sync_commit('6524c8e3-aae9-0000-5a14-be8ec000fffe', '20230511114021573');
   NOTICE:  set sync commit successfully, the next synchronization will start from 20230511114021573
   CONTEXT:  referenced column: hudi_set_sync_commit
    hudi_set_sync_commit
   ----------------------
    20230511114021573
   (1 row)

.. note::

   -  Only users who have the permission on the target task can invoke this function.
   -  Before invoking this function, ensure that the target task is paused and has been successfully executed at least once.

pg_task_show(text)
------------------

Description: Queries the information about the current automatic scheduling task. For a Hudi synchronization task, the input parameter should be **SQLonHudi**.

Return type: SETOF record

Example:

::

   SELECT * FROM pg_task_show('SQLonHudi');
                 task_id                |                                                                                          what                                                                                           | category_id | userid | is_broken |  interval  | time_cons |          start_time           | end_time | parameter | task_rank |        next_start_time        |         next_end_time         | last_log | failure_times
   --------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+--------+-----------+------------+-----------+-------------------------------+----------+-----------+-----------+-------------------------------+-------------------------------+----------+---------------
    64d257e9-1e9b-0d00-3ce3-7e61b5e0fffe | call pg_catalog.hudi_sync_custom('public.hudi_read_target', 'public.hudi_read101', '{"_hoodie_commit_seqno" : "_hoodie_commit_seqno", "id" : "id", "ts" : "ts", "long_field" : "ts"}'); | SQLonHudi   |     10 | f         | '00:00:10' |           | 2023-08-08 22:58:15.846903+08 |          |           |         5 | 2023-08-08 22:58:15.846903+08 | 2023-08-08 22:58:24.846903+08 |          |             0
   (1 row)

.. note::

   The **last_log** and **failure_times** fields are used to record the status of the last task.

   -  The value of **last_log** is updated when the task is complete. If the task is successful, the content is cleared. If the task fails, the task failure log is recorded.
   -  The value of **failure_times** is updated at the end of the time window. If the task is successful, the value of **failure_times** is set to 0. If the task fails, the value of **failure_times** increases by 1. The value of **failure_times** can be used to infer the time when the first failure occurs.

pg_task_remove(text)
--------------------

Description: Deletes an automatic scheduling task. The input parameter is a task ID. The function returns the number of deleted tasks.

Return type: integer

Example:

::

   SELECT pg_task_remove('64661705-8ada-0100-d07f-b57e89a0fffe');
    pg_task_remove
   ----------------
                 1
   (1 row)

pg_task_pause(text)
-------------------

Description: Suspend an automatic scheduling task. The input parameter is a task ID. The function returns the number of suspended tasks.

Return type: integer

Example:

::

   SELECT pg_task_pause('64661705-8ada-0100-d07f-b57e89a0fffe');
    pg_task_pause
   ---------------
                1
   (1 row)

pg_task_resume(text)
--------------------

Description: Resumes an automatic scheduling task. The input parameter is the ID of a suspended task. The function returns the number of resumed tasks. This function is supported only by 8.3.0 and later versions.

Return type: integer

Example:

::

   SELECT pg_task_resume('64661705-8ada-0100-d07f-b57e89a0fffe');
    pg_task_resume
   ----------------
                 1
   (1 row)

pg_task_reset_interval(text, interval)
--------------------------------------

Description: Modifies the scheduling period of a synchronization task. The first input parameter is **task_id**, and the second input parameter the scheduling period, of which the value ranges from 5 seconds to 24 hours. The function returns the number of tasks whose periods are modified. This function is supported only by 8.3.0 and later versions.

Return type: integer

Example:

::

   select pg_task_reset_interval('64bfd69c-a016-0000-120e-1e802978fffe', '10 hours 30 minutes');
   pg_task_reset_interval
   ------------------------
   1
   (1 row)
