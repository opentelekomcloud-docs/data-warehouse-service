:original_name: dws_04_1072.html

.. _dws_04_1072:

Hudi User Interfaces
====================

Querying Real-Time Views and Incremental Views
----------------------------------------------

GaussDB(DWS) provides table-level parameters similar to spark-sql to support real-time and incremental views.

The parameters are described as follows. Replace **SCHEMA.FOREIGN_TABLE** with the actual schema name and foreign table name.

.. table:: **Table 1** Parameters for querying real-time views and incremental views

   +-------------------------------------------------------+----------------+-------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                             | Value          | Description                                                                                                             |
   +=======================================================+================+=========================================================================================================================+
   | hoodie.SCHEMA.FOREIGN_TABLE.consume.mode              | SNAPSHOT       | Queries the real-time view.                                                                                             |
   +-------------------------------------------------------+----------------+-------------------------------------------------------------------------------------------------------------------------+
   |                                                       | INCREMENTAL    | Queries the incremental view.                                                                                           |
   +-------------------------------------------------------+----------------+-------------------------------------------------------------------------------------------------------------------------+
   | hoodie.SCHEMA.FOREIGN_TABLE.consume. start.timestamp  | hudi timestamp | Specifies the start commit of incremental synchronization.                                                              |
   +-------------------------------------------------------+----------------+-------------------------------------------------------------------------------------------------------------------------+
   | hoodie.SCHEMA.FOREIGN_TABLE.consume. ending.timestamp | hudi timestamp | Specifies the end commit of incremental synchronization. If this parameter is not specified, the latest commit is used. |
   +-------------------------------------------------------+----------------+-------------------------------------------------------------------------------------------------------------------------+

.. note::

   -  The preceding parameters can be set by running the set command and are valid only in the current session. You can run the reset command to restore the default values.
   -  You can use the system function **pg_catalog.pg_show_custom_settings()** to query the parameter setting details.
   -  When querying the incremental view of the MOR table, you need to use the **WHERE** condition to filter the **\_hoodie_commit_time** column to prevent the log file data that is not compacted from being read. This operation is not required for the COW table.

Querying Hudi Foreign Table and Automatically Synchronizing Tasks
-----------------------------------------------------------------

GaussDB(DWS) provides a series of system functions to obtain Hudi foreign table information and create Hudi automatic synchronization tasks. The automatic Hudi synchronization task periodically synchronizes data from Hudi foreign tables to GaussDB(DWS) internal tables.

.. table:: **Table 2** Hudi system functions

   +-----+-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No. | Function                                              | Type               | Functionality                                                                                                                                                     |
   +=====+=======================================================+====================+===================================================================================================================================================================+
   | 1   | pg_show_custom_settings()                             | Built-in functions | Queries details about the parameter settings of an HUDI foreign table.                                                                                            |
   +-----+-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 2   | hudi_get_options(regclass)                            | Built-in functions | Queries the attributes of an HUDI foreign table (hoodie.properties).                                                                                              |
   +-----+-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 3   | hudi_get_max_commit(regclass)                         | Built-in functions | Obtains the latest commit timestamp of the current HUDI foreign table.                                                                                            |
   +-----+-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 4   | hudi_sync_task_submit(regclass, regclass)             | Built-in functions | Submits the HUDI automatic synchronization task.                                                                                                                  |
   +-----+-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |     | hudi_sync_task_submit(regclass, regclass, text, text) |                    |                                                                                                                                                                   |
   +-----+-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 5   | hudi_show_sync_state()                                | Built-in functions | Obtains the synchronization status of the HUDI automatic synchronization task.                                                                                    |
   +-----+-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 6   | hudi_sync(regclass, regclass)                         | Stored procedure   | Specifies the entry for invoking the HUDI automatic synchronization task.                                                                                         |
   +-----+-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 7   | hudi_sync_custom(regclass, regclass, text)            | Stored procedure   | Specifies the entry for invoking the HUDI automatic synchronization task. Users can define the mapping between fields in the target table and data source table.  |
   +-----+-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 8   | hudi_set_sync_commit(regclass, regclass, text)        | Built-in functions | Sets the start timestamp of the first synchronization of the HUDI automatic synchronization task to prevent resynchronization.                                    |
   +-----+-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |     | hudi_set_sync_commit(text, text)                      |                    | Sets the start timestamp of the next synchronization of a HUDI automatic synchronization task. You can use it to sync historical data again or to skip some data. |
   +-----+-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
