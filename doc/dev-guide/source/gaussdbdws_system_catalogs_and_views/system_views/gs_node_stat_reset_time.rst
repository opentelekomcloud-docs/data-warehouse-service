:original_name: dws_04_0692.html

.. _dws_04_0692:

GS_NODE_STAT_RESET_TIME
=======================

**GS_NODE_STAT_RESET_TIME** provides the statistics reset time of the current node and returns a timestamp with the time zone.

For details, see the **get_node_stat_reset_time()** function in "Functions and Operators > System Administration Functions > Other Functions" in *SQL Syntax*.

.. note::

   When an instance is running, its statistics keep rising. In the following cases, the statistical values in the memory will be reset to **0**:

   -  The instance is restarted or a cluster switchover occurs.
   -  The database is dropped.
   -  A reset operation is performed. For example, the statistics counter in the database is reset using the **pgstat_recv_resetcounter** function or the Unique SQL statements are cleared using the **reset_instr_unique_sql** function.

   If any of the preceding events occurs, GaussDB(DWS) will record the time when the statistics are reset. You can query the time using the **get_node_stat_reset_time** function.
