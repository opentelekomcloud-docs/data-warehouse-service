:original_name: dws_06_0057.html

.. _dws_06_0057:

Snapshot Synchronization Functions
==================================

Snapshot synchronization functions save the current snapshot and return its identifier.

create_wdr_snapshot()
---------------------

Description: Creates a performance data snapshot.

Return type: text

.. note::

   -  Only the database administrator **SYSADMIN** can execute this function.
   -  This function can be executed only on CNs. If it is executed on DNs, the following message will be returned: "WDR snapshot can only be created on coordinator."
   -  Before executing this function, ensure that the value of **enable_wdr_snapshot** is **on**. If its value is **off**, the following message will be returned for this function: "WDR snapshot request can't be executed, because GUC parameter 'enable_wdr_snapshot' is off."
   -  If the snapshot thread is not started for some reason, for example, the node is restarted, the following message will be returned for this function: "WDR snapshot request can not be accepted, please retry later."
   -  If this function fails to be executed, the following message will be returned: "Cannot respond to WDR snapshot request."
   -  If this function is successfully executed, the following message will be returned: "WDR snapshot request has been submitted." This message indicates that the snapshot creation request has been sent to the background snapshot thread, but does not mean that the snapshot has been successfully created.

kill_snapshot(scope cstring)
----------------------------

Description: Kills the background snapshot thread. This function sends a command to the background snapshot thread and waits for the thread to stop.

The input parameter **scope** indicates the operation scope. Its value can be **local** or **global**.

-  Value **local** indicates killing the snapshot thread on the current CN.
-  Value **global** indicates killing the snapshot thread on the current CN as well as those on all the other CNs in the cluster.
-  If any other value is passed, error message "Scope is invalid, use "local" or "global"." is displayed.
-  The input parameter can be left empty, in which case the default value **local** will be used.

Return type: none

.. note::

   -  Only the database administrator **SYSADMIN** can execute this function.
   -  This function can be executed only on CNs. If it is executed on DNs, the following message will be returned: "kill_snapshot can only be executed on coordinator."
   -  Executing this function sends a kill signal to the background snapshot thread and waits for it to finish. If the snapshot thread is not killed within 100s, the error message "Kill snapshot thread failed" is displayed.

pg_export_snapshot()
--------------------

Description: Saves the current snapshot and returns its identifier.

Return type: text

Note: **pg_export_snapshot** saves the current snapshot and returns a text string identifying the snapshot. This string must be passed to clients that want to import the snapshot. A snapshot can be imported when the **set transaction snapshot snapshot_id;** command is executed. Doing so is possible only when the transaction is set to the **REPEATABLE READ** isolation level. The output of the function cannot be used as the input of **set transaction snapshot**.

Example:

::

   SELECT pg_export_snapshot();
    pg_export_snapshot
   --------------------
    00000000000A7128-1
   (1 row)
