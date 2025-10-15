:original_name: dws_04_0923.html

.. _dws_04_0923:

Automatic Cleanup
=================

The automatic cleanup process in the system automatically runs the **VACUUM** and **ANALYZE** statements to reclaim the record space marked as deleted and update statistics in the table.

autovacuum_compaction_rows_limit
--------------------------------

**Parameter description**: Specifies the small CU threshold. A CU whose number of live tuples is less than the value of this parameter is considered as a small CU. This parameter is supported only by clusters of version 8.2.1.300 or later.

**Type**: USERSET

**Value range**: an integer ranging from -1 to 5000

**Default value**: **2500**

.. important::

   If the version is earlier than 9.1.0.100, do not set this parameter. Otherwise, duplicate primary key data may occur.

.. note::

   -  If the version is earlier than 9.1.0.100, value **-1** indicates that the 0 CU switch is disabled.
   -  In version 9.1.0.100, the default value of this parameter is **0**.
   -  In 9.1.0.200 and later versions, the default value of this parameter is **2500**.
   -  **You are advised not to modify this parameter. If you need to modify this parameter, contact technical support.**

autoanalyze_mode
----------------

**Parameter description**: Specifies the autoanalyze mode. This parameter is supported by clusters of version 8.2.0 or later.

**Type**: USERSET

**Value range**: enumerated values

-  **normal** indicates common autoanalyze.
-  **light** indicates lightweight autoanalyze.

**Default value**:

-  If the current cluster is upgraded from an earlier version to 8.2.0, the default value is **normal** to ensure forward compatibility.
-  If the cluster version 8.2.0 is newly installed, the default value is **light**.

analyze_stats_mode
------------------

**Parameter description**: Specifies the mode for **ANALYZE** to calculate statistics.

**Type**: USERSET

**Value range**: enumerated values

-  **memory** indicates that the memory is forcibly used to calculate statistics. Multi-column statistics are not calculated.
-  **sample_table** indicates that temporary sampling tables are forcibly used to calculate statistics. Temporary tables do not support this mode.
-  **dynamic** indicates that the statistics calculation mode is determined based on the size of **maintenance_work_mem**. If :ref:`maintenance_work_mem <en-us_topic_0000001811610373__sfbfa78b6871442cb85a84a425335ce38>` can store samples, the memory mode is used. Otherwise, the temporary sampling table mode is used.

**Default value**:

-  If the current cluster is upgraded from an earlier version to 8.2.0.100, the default value is **memory** to ensure forward compatibility.
-  If the cluster version 8.2.0.100 is newly installed, the default value is **dynamic**.

analyze_sample_mode
-------------------

**Parameter description**: Specifies the sampling model used by **ANALYZE**.

**Type**: USERSET

**Value range**: an integer ranging from **0** to **2**

-  **0** indicates the default reservoir sampling.
-  **1** indicates the optimized reservoir sampling.
-  **2** indicates range sampling.

**Default value**: **0**

.. _en-us_topic_0000001764650468__s502d4304994d4da5bd3cda661aab27ac:

autovacuum_max_workers
----------------------

**Parameter description**: Specifies the maximum number of automatic cleanup threads running at the same time.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 128. **0** indicates that **autovacuum** is disabled.

**Default value**: **6**

.. note::

   This parameter works with autovacuum. The rules for clearing system catalogs and user tables are as follows:

   -  When **autovacuum_max_workers** is set to **0**, **autovacuum** is disabled and no tables are cleared.
   -  When **autovacuum_max_workers** is set to a value greater than 0 and **autovacuum** is set to **off**, the system only clears the system catalogs and column-store tables with delta tables enabled (such as vacuum delta tables, vacuum cudesc tables, and delta merge).
   -  When **autovacuum_max_workers** is set to a value greater than 0 and **autovacuum** is set to **on**, all tables will be cleared.

autovacuum_max_workers_hstore
-----------------------------

**Parameter description**: Specifies the maximum number of concurrent automatic cleanup threads used for hstore tables in **autovacuum_max_workers**.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 128. **0** indicates that the automatic cleanup function of HStore tables is disabled.

**Default value**: **3**

.. note::

   To use HStore tables, set the following parameters, or the HStore performance will deteriorate severely. The recommended settings are as follows:

   **autovacuum_max_workers_hstore=3, autovacuum_max_workers=6, autovacuum=true**

autovacuum_naptime
------------------

**Parameter description**: Specifies the interval between two automatic cleanup operations.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to 2147483. The unit is second.

**Default value**: **60s**

autovacuum_vacuum_cost_delay
----------------------------

**Parameter description**: Specifies the value of the cost delay used in the **autovacuum** operation.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to 100. The unit is ms. **-1** indicates that the normal vacuum cost delay is used.

**Default value**: **2ms**

check_crossvw_write
-------------------

**Parameter description**: Specifies whether to enable cross-VW write detection. This parameter is supported only by clusters of version 9.1.0.100 or later.

**Type**: USERSET

**Value range**: an integer, -1 or 1.

-  The value **-1** indicates that it is compatible with the capabilities of version 9.0.3. For the v3 table vacuum, it only clears non-last files for all epochs.
-  The value **1** indicates checking whether it is a cross-VW write scenario. For the v3 table vacuum, if it is determined to be a non-cross-VW write scenario, it clears non-last files for all epochs, clears the last file for the current epoch, and clears the last file for epochs that are less than the current epoch. If it is determined to be a cross-VW write scenario, CNs will obtain epoch information from all DNs and package it into an epochList to be sent to the metadata VW. The v3 table vacuum will clear non-last files for all epochs and clear the last file for epochs that are less than max{epochList} and not in epochList.

**Default value**: **1**

.. _en-us_topic_0000001764650468__section4328534144311:

enable_pg_stat_object
---------------------

**Parameter description**: Specifies whether **AUTO VACUUM** updates the :ref:`PG_STAT_OBJECT <dws_04_1062>` system catalog. This parameter is supported only by clusters of version 8.2.1 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the **PG_STAT_OBJECT** system catalog is updated during **AUTO VACUUM**.
-  **off** indicates that the **PG_STAT_OBJECT** system catalog is not updated during **AUTO VACUUM**.

**Default value**: **on**
