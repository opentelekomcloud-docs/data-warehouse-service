:original_name: dws_04_1027.html

.. _dws_04_1027:

Hybrid Data Warehouse Functions
===============================

hstore_light_merge(rel_name text)
---------------------------------

Description: This function is used to manually perform lightweight cleanup on HStore tables and holds the level-3 lock of the target table.

Return type: int

Example:

::

   SELECT hstore_light_merge('reason_select');

hstore_full_merge(rel_name text)
--------------------------------

Description: This function is used to manually perform full cleanup on HStore tables.

Return type: int

.. important::

   -  This operation forcibly merges all the visible operations of the delta table to the primary table, and then creates an empty delta table. During this period, this operation holds the level-8 lock of the table.
   -  The duration of this operation depends on the amount of data in the delta table. You must enable the HStore clearing thread to ensure unnecessary data in the HStore table is cleared in a timely manner.

Example:

::

   SELECT hstore_full_merge('reason_select');
