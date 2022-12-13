:original_name: dws_03_0089.html

.. _dws_03_0089:

Do I Need to Run VACUUM FULL and ANALYZE on Common Tables Periodically?
=======================================================================

Yes. For tables that involve frequent add, delete, or modify operations, perform **VACUUM FULL** and **ANALYZE** to reclaim the disk space occupied by updated or deleted data, preventing performance deterioration caused by data expansion and inaccurate statistics.

Generally, you are advised to use **ANALYZE** after multiple add and modify operations are performed on a table, and use **VACUUM FULL** after delete operations are performed. **VACUUM FULL** can be used only on special occasions, such as when you want to physically narrow a table to decrease the occupied disk space after deleting most rows of the table. **VACUUM FULL** usually shrinks a table more than **VACUUM** does.

Syntax
------

.. code-block::

   -- Perform ANALYZE on a table.
   ANALYZE Table name;
   -- Perform ANALYZE on all tables (non-foreign tables) in the database.
   ANALYZE;
   -- Perform VACUUM on a table.
   VACUUM Table name;
   -- -- Perform VACUUM FULL on a table.
   VACUUM FULL Table_name;

For details, see in "VACUUM" and "ANALYZE \| ANALYSE" in the *Developer Guide*.

.. note::

   If the physical space usage does not decrease after you run the **VACUUM FULL** command, check whether there were other active transactions (started before you delete data transactions and not ended before you run **VACUUM FULL**). If yes, run this command again when the transactions have finished.
