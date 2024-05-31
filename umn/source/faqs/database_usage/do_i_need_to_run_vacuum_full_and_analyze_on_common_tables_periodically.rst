:original_name: dws_03_0089.html

.. _dws_03_0089:

Do I Need to Run VACUUM FULL and ANALYZE on Common Tables Periodically?
=======================================================================

Yes.

For tables that are frequently added, deleted, or modified, you need to periodically perform **VACUUM FULL** and **ANALYZE** to reclaim the disk space occupied by updated or deleted data, preventing performance deterioration caused by data bloat and inaccurate statistics.

-  Generally, you are advised to perform **ANALYZE** after a large number of **adding or modification** operations are performed on a table.
-  After a table is deleted, you are advised to run **VACUUM** rather than **VACUUM FULL**. However, you can run **VACUUM FULL** in some particular cases, such as when you want to physically narrow a table to decrease the occupied disk space after deleting most rows of the table. For details about the differences between **VACUUM** and **VACUUM FULL**, see :ref:`VACUUM and VACUUM FULL <en-us_topic_0000001330488844__section6930204215810>`.

Syntax
------

Perform **ANALYZE** on a table.

.. code-block::

   ANALYZE table_name;

Perform **ANALYZE** on all tables (non-foreign tables) in the database.

.. code-block::

   ANALYZE;

Perform **VACUUM** on a table.

.. code-block::

   VACUUM table_name;

Perform **VACUUM FULL** on a table.

.. code-block::

   VACUUM FULL table_name;

For details, see sections "VACUUM" and "ANALYZE \| ANALYSE" in the *Developer Guide*.

.. note::

   -  If the physical space usage does not decrease after you run the **VACUUM FULL** command, check whether there were other active transactions (started before you delete data transactions and not ended before you run **VACUUM FULL**). If yes, run this command again when the transactions have finished.
   -  In version 8.1.3 or later, **VACUUM**/**VACUUM FULL** can be invoked on the management plane. For details, see "Intelligent O&M" in the *Data Warehouse Service (DWS) User Guide*.

.. _en-us_topic_0000001330488844__section6930204215810:

VACUUM and VACUUM FULL
----------------------

In GaussDB(DWS), the **VACUUM** operation is like a vacuum cleaner used to absorb dust. Here, "dust" means old data. If the data is not cleared in a timely manner, more database space will be used to store such data, causing performance downgrade or even a system breakdown.

Purposes of VACUUM:

-  Solve space bloat: Clear obsolete tuples and corresponding indexes, which include the tuple (and index) of a committed **DELETE** transaction, the old version (and index) of an **UPDATE** transaction, the inserted tuple (and index) of a rolled back **INSERT** transaction, the new version (and index) of an **UPDATE** transaction, and the tuple (and index) of a **COPY** transaction.
-  VACUUM FREEZE: Prevents system breakdown caused by transaction ID wraparound. It converts transaction IDs smaller than OldestXmin to freeze xids, update relfrozenxids in a table, and update relfrozenxids and truncate clogs in a database.
-  Update statistics: **VACUUM ANALYZE** updates statistics, enabling the optimizer to select a better way to execute SQL statements.

The VACUUM statement includes **VACUUM** and **VACUUM FULL**. Currently, **VACUUM** can only work on row-store tables. **VACUUM FULL** can be used to release space of column-store tables. For details, see the following table.

.. table:: **Table 1** VACUUM and VACUUM FULL

   +--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Item               | VACUUM                                                                                                                                                                                                                                                                                    | VACUUM FULL                                                                                                                                                                                |
   +====================+===========================================================================================================================================================================================================================================================================================+============================================================================================================================================================================================+
   | Clearing space     | If the deleted record is at the end of a table, the space occupied by the deleted record is physically released and returned to the operating system. If the data is not at the end of a table, the space occupied by dead tuples in the table or index is set to be available for reuse. | Despite the position of the deleted data, the space occupied by the data is physically released and returned to the operating system. When data is inserted, a new disk page is allocated. |
   +--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Lock type          | Shared lock. The **VACUUM** operation can be performed in parallel with other operations.                                                                                                                                                                                                 | Exclusive lock. All operations based on the table are suspended during execution.                                                                                                          |
   +--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Physical space     | Not released                                                                                                                                                                                                                                                                              | Released                                                                                                                                                                                   |
   +--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Transaction ID     | Not reclaimed                                                                                                                                                                                                                                                                             | Reclaimed                                                                                                                                                                                  |
   +--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Execution overhead | The overhead is low and the operation can be executed periodically.                                                                                                                                                                                                                       | The overhead is high. You are advised to perform it when the disk page space occupied by the database is close to the threshold and the data operations are few.                           |
   +--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Effect             | It improves the efficiency of operations on the table.                                                                                                                                                                                                                                    | It greatly improves the efficiency of operations on the table.                                                                                                                             |
   +--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
