:original_name: dws_03_2108.html

.. _dws_03_2108:

Which System Catalogs in GaussDB(DWS) Cannot Undergo the VACUUM FULL Operation?
===============================================================================

From a functional perspective, **VACUUM FULL** can be performed on all GaussDB(DWS) system catalogs, which however involves using eight levels of locks, thereby blocking services related to these system catalogs.

The suggestions are based on database versions:

Version 8.1.3 or Later
----------------------

-  For clusters of version 8.1.3 or later, **AUTO VACUUM** is enabled by default (controlled by the **autovacuum** parameter). After you set the parameter, the system automatically performs **VACUUM FULL** on all system catalogs and row-store tables.

   -  If the value of **autovacuum_max_workers** is **0**, neither on the system catalogs nor on ordinary tables will **VACUUM FULL** be automatically performed.
   -  If **autovacuum** is set to **off**, **VACUUM FULL** will be automatically performed on ordinary tables, but not system catalogs.

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

#. During routine O&M, you are advised to monitor the sizes of these system catalogs, and collect statistics every week. If the space must be reclaimed, clear the space based on the sizes of the system tables.

   The statement is as follows:

   ::

      SELECT c.oid,c.relname, c.relkind, pg_relation_size(c.oid) AS size  FROM pg_class c WHERE c.relkind IN ('r') AND c.oid <16385 ORDER BY size DESC;
