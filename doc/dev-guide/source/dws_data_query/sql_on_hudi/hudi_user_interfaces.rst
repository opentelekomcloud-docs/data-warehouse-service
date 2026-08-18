:original_name: dws_04_1072.html

.. _dws_04_1072:

Hudi User Interfaces
====================

Querying Real-Time Views and Incremental Views
----------------------------------------------

DWS provides table-level parameters similar to spark-sql to support real-time and incremental views.

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

   -  The preceding parameters can be set by running the **set** command and are valid only in the current session. You can run the **reset** command to restore the default values.
   -  You can use the system function **pg_catalog.pg_show_custom_settings()** to query the parameter setting details.
   -  When querying the incremental views of the **MOR** tables, you need to use the **WHERE** conditions to filter the **\_hoodie_commit_time** field to prevent the log file data that is not combined and does not meet the conditions from being read. This operation is not required for **COW** tables.

Querying Hudi Foreign Table and Automatically Synchronizing Tasks
-----------------------------------------------------------------

DWS provides a series of system functions to obtain Hudi foreign table information and create Hudi automatic synchronization tasks. The automatic Hudi synchronization task periodically synchronizes data from Hudi foreign tables to DWS internal tables.

.. table:: **Table 2** Hudi system functions

   +-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Function                                              | Type               | Functionality                                                                                                                                                     |
   +=======================================================+====================+===================================================================================================================================================================+
   | pg_show_custom_settings()                             | Built-in functions | Queries details about the parameter settings of a Hudi foreign table.                                                                                             |
   +-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hudi_get_options(regclass)                            | Built-in functions | Queries the attributes of a Hudi foreign table (hoodie.properties).                                                                                               |
   +-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hudi_get_max_commit(regclass)                         | Built-in functions | Obtains the latest commit timestamp of the current Hudi foreign table.                                                                                            |
   +-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hudi_sync_task_submit(regclass, regclass)             | Built-in functions | Submits the Hudi automatic synchronization task.                                                                                                                  |
   +-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hudi_sync_task_submit(regclass, regclass, text, text) |                    |                                                                                                                                                                   |
   +-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hudi_show_sync_state()                                | Built-in functions | Obtains the synchronization status of the Hudi automatic synchronization task.                                                                                    |
   +-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hudi_sync(regclass, regclass)                         | Stored procedure   | Specifies the entry for invoking the Hudi automatic synchronization task.                                                                                         |
   +-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hudi_sync_custom(regclass, regclass, text)            | Stored procedure   | Specifies the entry for invoking the Hudi automatic synchronization task. Users can define the mapping between fields in the target table and data source table.  |
   +-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hudi_set_sync_commit(regclass, regclass, text)        | Built-in functions | Sets the start timestamp of the first synchronization of the Hudi automatic synchronization task to prevent resynchronization.                                    |
   +-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hudi_set_sync_commit(text, text)                      |                    | Sets the start timestamp of the next synchronization of a Hudi automatic synchronization task. You can use it to sync historical data again or to skip some data. |
   +-------------------------------------------------------+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
