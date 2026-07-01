:original_name: dws_03_2108.html

.. _dws_03_2108:

Which System Catalogs in DWS Cannot Undergo the VACUUM FULL Operation?
======================================================================

In clusters earlier than 9.1.1.100, **VACUUM FULL** can be performed on all DWS system catalogs. This operation needs a level-8 lock and blocks services using those catalogs.

In clusters of version 9.1.1.100 or later, you can set **allow_system_table_vacuum_full** to determine whether to execute VACUUM FULL on system catalogs.

The suggestions are based on database versions:

Version 8.1.3 or Later
----------------------

-  For clusters of version 8.1.3 or later, **AUTO VACUUM** is enabled by default (controlled by the **autovacuum** parameter). After you set the parameter, the system automatically performs **VACUUM FULL** on all system catalogs and row-store tables.

   -  If the value of **autovacuum_max_workers** is **0**, neither on the system catalogs nor on ordinary tables will **VACUUM FULL** be automatically performed.
   -  If **autovacuum** is set to **off**, **VACUUM FULL** will be automatically performed only on the system catalogs.

-  This applies only to row-store tables. To automatically trigger **VACUUM** for column-store tables, you need to configure intelligent scheduling tasks on the management console.

Version 8.1.1 or Earlier
------------------------

#. Reforming **VACUUM FULL** on the following system catalogs affects all services. Perform this operation in an idle time window or when services are stopped.

   -  **pg_statistic** (Statistics information. You are advised not to clear it because it affects service query performance.)
   -  pg_attribute
   -  pgxc_class
   -  pg_type
   -  pg_depend
   -  pg_class
   -  pg_index
   -  pg_proc
   -  pg_partition
   -  pg_object
   -  pg_shdepend

#. The following system catalogs affect resource monitoring and table size query interfaces, but do not affect other services.

   -  gs_wlm_user_resource_history
   -  gs_wlm_session_info
   -  gs_wlm_instance_history
   -  gs_respool_resource_history
   -  pg_relfilenode_size

#. Other system catalogs do not occupy space and do not need to be cleared.

#. During routine O&M, you are advised to monitor the sizes of the system catalogs in the following statement every week. If the space must be reclaimed, process key system catalogs first.

   The statement is as follows:

   ::

      SELECT c.oid,c.relname, c.relkind, pg_relation_size(c.oid) AS size  FROM pg_class c WHERE c.relkind IN ('r') AND c.oid <16385 ORDER BY size DESC;
