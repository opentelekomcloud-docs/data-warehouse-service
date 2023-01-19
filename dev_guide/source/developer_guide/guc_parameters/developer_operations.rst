:original_name: dws_04_0936.html

.. _dws_04_0936:

Developer Operations
====================

enable_light_colupdate
----------------------

**Parameter description**: Specifies whether to enable the lightweight column-store update.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the lightweight column-store update is enabled.
-  **off** indicates that the lightweight column-store update is disabled.

**Default value**: **off**

.. _en-us_topic_0000001145894431__s9b7f64f4f112450490c8c74b520cc915:

enable_fast_query_shipping
--------------------------

**Parameter description**: Specifies whether to use the distributed framework for a query planner.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that execution plans are generated on CNs and DNs separately.
-  **off** indicates that the distributed framework is used. Execution plans are generated on CNs and then sent to DNs for execution.

**Default value**: **on**

enable_trigger_shipping
-----------------------

**Parameter description**: Specifies whether the trigger can be pushed to DNs for execution.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the trigger can be pushed to DNs for execution.
-  **off** indicates that the trigger cannot be pushed to DNs. It must be executed on the CN.

**Default value**: **on**

enable_remotejoin
-----------------

**Parameter description:** Specifies whether JOIN operation plans can be delivered to DNs for execution.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that JOIN operation plans can be delivered to DNs for execution.
-  **off** indicates that JOIN operation plans cannot be delivered to DNs for execution.

**Default value**: **on**

enable_remotegroup
------------------

**Parameter description:** Specifies whether the execution plans of **GROUP BY** and **AGGREGATE** can be delivered to DNs for execution.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the execution plans of **GROUP BY** and **AGGREGATE** can be delivered to DNs for execution.
-  **off** indicates that the execution plans of **GROUP BY** and **AGGREGATE** cannot be delivered to DNs for execution.

**Default value**: **on**

enable_remotelimit
------------------

**Parameter description**: Specifies whether the execution plan specified in the LIMIT clause can be pushed down to DNs for execution.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the execution plan specified in the LIMIT clause can be pushed down to DNs for execution.
-  **off** indicates that the execution plan specified in the LIMIT clause cannot be delivered to DNs for execution.

**Default value**: **on**

enable_remotesort
-----------------

**Parameter description:** Specifies whether the execution plan of the ORDER BY clause can be delivered to DNs for execution.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the execution plan of the ORDER BY clause can be delivered to DNs for execution.
-  **off** indicates that the execution plan of the ORDER BY clause cannot be delivered to DNs for execution.

**Default value**: **on**

enable_join_pseudoconst
-----------------------

**Parameter description**: Specifies whether joining with the pseudo constant is allowed. A pseudo constant indicates that the variables on both sides of a join are identical to the same constant.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that joining with the pseudo constant is allowed.
-  **off** indicates that joining with the pseudo constant is not allowed.

**Default value**: **off**

cost_model_version
------------------

**Parameter description**: Specifies the model used for cost estimation in the application scenario. This parameter affects the distinct estimation of the expression, HashJoin cost model, estimation of the number of rows, distribution key selection during redistribution, and estimation of the number of aggregate rows.

**Type**: USERSET

**Value range**: **0**, **1**, or **2**

-  **0** indicates that the original cost estimation model is used.
-  **1** indicates that the enhanced distinct estimation of the expression, HashJoin cost estimation model, estimation of the number of rows, distribution key selection during redistribution, and estimation of the number of aggregate rows are used on the basis of **0**.
-  **2** indicates that the ANALYZE sampling algorithm with better randomicity is used on the basis of **1** to improve the accuracy of statistics collection.

**Default value**: **1**

debug_assertions
----------------

**Parameter description**: Specifies whether to enable various assertion checks. This parameter assists in debugging. If you are experiencing strange problems or crashes, set this parameter to **on** to identify programming defects. To use this parameter, the macro USE_ASSERT_CHECKING must be defined (through the configure option **--enable-cassert**) during the GaussDB(DWS) compilation.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that various assertion checks are enabled.
-  **off** indicates that various assertion checks are disabled.

.. note::

   This parameter is set to **on** by default if GaussDB(DWS) is compiled with various assertion checks enabled.

**Default value**: **off**

distribute_test_param
---------------------

**Parameter description**: Specifies whether the embedded test stubs for testing the distribution framework take effect. In most cases, developers embed some test stubs in the code during fault injection tests. Each test stub is identified by a unique name. The value of this parameter is a triplet that includes three values: thread level, test stub name, and error level of the injected fault. The three values are separated by commas (,).

**Type**: USERSET

**Value range**: a string indicating the name of any embedded test stub.

**Default value**: **-1, default, default**

ignore_checksum_failure
-----------------------

**Parameter description**: Sets whether to ignore check failures (but still generates an alarm) and continues reading data. This parameter is valid only when enable_crc_check is set to **on**. Continuing reading data may result in breakdown, damaged data being transferred or hidden, failure of data recovery from remote nodes, or other serious problems. You are not advised to modify the settings.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that data check errors are ignored.
-  **off** indicates that data check errors are reported.

**Default value**: **off**

enable_colstore
---------------

**Parameter description**: Specifies whether to create a table as a column-store table by default when no storage method is specified. The value for each node must be the same. This parameter is used for tests. Users are not allowed to enable it.

**Type**: SUSET

**Value range**: Boolean

**Default value**: **off**

enable_force_vector_engine
--------------------------

**Parameter description**: Specifies whether to forcibly generate vectorized execution plans for a vectorized execution operator if the operator's child node is a non-vectorized operator. When this parameter is set to **on**, vectorized execution plans are forcibly generated. When **enable_force_vector_engine** is enabled, no matter it is a row-store table, column-store table, or hybrid row-column store table, if the plantree does not contain scenarios that do not support vectorization, the vectorized executor is forcibly used.

**Type**: USERSET

**Value range**: Boolean

**Default value**: **off**

enable_csqual_pushdown
----------------------

**Parameter description**: Specifies whether to deliver filter criteria for a rough check during query.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that a rough check is performed with filter criteria delivered during query.
-  **off** indicates that a rough check is performed without filter criteria delivered during query.

**Default value**: **on**

explain_dna_file
----------------

**Parameter description**: Specifies the name of a CSV file exported when :ref:`explain_perf_mode <en-us_topic_0000001145894431__s16fe71bb07ef45c4b3119ee670eac7d1>` is set to **run**.

**Type**: USERSET

.. important::

   The value of this parameter must be an absolute path plus a file name with the extension **.csv**.

**Value range**: a string

**Default value**: **NULL**

.. _en-us_topic_0000001145894431__s16fe71bb07ef45c4b3119ee670eac7d1:

explain_perf_mode
-----------------

**Parameter description**: Specifies the display format of the **explain** command.

**Type**: USERSET

**Value range**: **normal**, **pretty**, **summary**, and **run**

-  **normal** indicates that the default printing format is used.
-  **pretty** indicates that the optimized display mode of GaussDB(DWS) is used. A new format contains a plan node ID, directly and effectively analyzing performance.
-  **summary** indicates that the analysis result based on such information is printed in addition to the printed information in the format specified by **pretty**.
-  **run** indicates that in addition to the printed information specified by **summary**, the database exports the information as a CSV file.

**Default value**: **pretty**

join_num_distinct
-----------------

**Parameter description**: Controls the default distinct value of the join column or expression in application scenarios.

**Type**: USERSET

**Value range**: a double-precision floating point number greater than or equal to **-100**. Decimals may be truncated when displayed on clients.

-  If the value is greater than **0**, the value is used as the default distinct value.
-  If the value is greater than or equal to **-100** and less than **0**, it means the percentage used to estimate the default distinct value.
-  If the value is **0**, the default distinct value is **200**.

**Default value**: **-20**

qual_num_distinct
-----------------

**Parameter description**: Controls the default distinct value of the filter column or expression in application scenarios.

**Type**: USERSET

**Value range**: a double-precision floating point number greater than or equal to **-100**. Decimals may be truncated when displayed on clients.

-  If the value is greater than **0**, the value is used as the default distinct value.
-  If the value is greater than or equal to **-100** and less than **0**, it means the percentage used to estimate the default distinct value.
-  If the value is **0**, the default distinct value is **200**.

**Default value**: **200**

trace_notify
------------

**Parameter description**: Specifies whether to generate a large amount of debugging output for the **LISTEN** and **NOTIFY** commands. :ref:`client_min_messages <en-us_topic_0000001098655026__sbd8ad9bb6b9b48ba97f998f060dc56f3>` or :ref:`log_min_messages <en-us_topic_0000001098655026__s1ffb0797361d413d875381200fed970b>` must be **DEBUG1** or lower so that such output can be recorded in the logs on the client or server separately.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the function is enabled.
-  **off** indicates that the function is disabled.

**Default value**: **off**

trace_recovery_messages
-----------------------

**Parameter description**: Specifies whether to enable logging of recovery-related debugging output. This parameter allows users to overwrite the normal setting of :ref:`log_min_messages <en-us_topic_0000001098655026__s1ffb0797361d413d875381200fed970b>`, but only for specific messages. This is intended for use in debugging the standby server.

**Type**: SIGHUP

**Value range**: enumerated values. Valid values include **debug5**, **debug4**, **debug3**, **debug2**, **debug1**, and **log**. For details about the parameter values, see :ref:`log_min_messages <en-us_topic_0000001098655026__s1ffb0797361d413d875381200fed970b>`.

**Default value**: **log**

.. note::

   -  **log** indicates that recovery-related debugging information will not be logged.
   -  Except the default value **log**, each of the other values indicates that recovery-related debugging information at the specified level will also be logged. Common settings of **log_min_messages** will unconditionally record information into server logs.

trace_sort
----------

**Parameter description**: Specifies whether to display information about resource usage during sorting operations in logs. This parameter is available only when the macro TRACE_SORT is defined during the GaussDB(DWS) compilation. However, TRACE_SORT is currently defined by default.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the function is enabled.
-  **off** indicates that the function is disabled.

**Default value**: **off**

zero_damaged_pages
------------------

**Parameter description**: Specifies whether to detect a damaged page header that causes GaussDB(DWS) to report an error, aborting the current transaction.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the function is enabled.
-  **off** indicates that the function is disabled.

.. note::

   -  Setting this parameter to **on** causes the system to report a warning, pad the damaged page with zeros, and then continue with subsequent processing. This behavior will damage data, that is, all rows on the damaged page. However, it allows you to bypass the error and retrieve rows from any undamaged pages that are present in the table. Therefore, it is useful for restoring data that is damaged due to a hardware or software error. In most cases, you are not advised to set this parameter to **on** unless you do not want to restore data from the damaged pages of a table.
   -  For a column-store table, the system will skip the entire CU and then continue processing. The supported scenarios include the CRC check failure, magic check failure, and incorrect CU length.

**Default value**: **off**

string_hash_compatible
----------------------

**Parameter description**: Specifies whether to use the same method to calculate char-type hash values and varchar- or text-type hash values. Based on the setting of this parameter, you can determine whether a redistribution is required when a distribution column is converted from a char-type data distribution into a varchar- or text-type data distribution.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** indicates that the same calculation method is used and a redistribution is not required.
-  **off** indicates that different calculation methods are used and a redistribution is required.

.. note::

   Calculation methods differ in the length of input strings used for calculating hash values. (For a char-type hash value, spaces following a string are not counted as the length. For a text- or varchar-type hash value, the spaces are counted.) The hash value affects the calculation result of queries. To avoid query errors, do not modify this parameter during database running once it is set.

**Default value**: **off**

replication_test
----------------

**Parameter description**: Specifies whether to enable internal testing on the data replication function.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that internal testing on the data replication function is enabled.
-  **off** indicates that internal testing on the data replication function is disabled.

**Default value**: **off**

cost_param
----------

**Parameter description**: Controls use of different estimation methods in specific customer scenarios, allowing estimated values approximating to onsite values. This parameter can control various methods simultaneously by performing AND (&) operations on the bit for each method. A method is selected if its value is not **0**.

If **cost_param & 1** is not set to **0**, an improvement mechanism is selected for calculating a non-equi join selection rate, which is more accurate in estimation of self-join (join between two same tables). In V300R002C00 and later, **cost_param & 1=0** is not used. That is, an optimized formula is selected for calculation.

When **cost_param & 2** is set to a value other than **0**, the selection rate is estimated based on multiple filter criteria. The lowest selection rate among all filter criteria, but not the product of the selection rates for two tables under a specific filter criterion, is used as the total selection rate. This method is more accurate when a close correlation exists between the columns to be filtered.

When **cost_param & 4** is not **0**, the selected debugging model is not recommended when the stream node is evaluated.

When **cost_param & 16** is not **0**, the model between fully correlated and fully uncorrelated models is used to calculate the comprehensive selection rate of two or more filtering conditions or join conditions. If there are many filtering conditions, the strongly-correlated model is preferred.

**Type**: USERSET

**Value range**: an integer ranging from 1 to INT_MAX

**Default value**: **16**

convert_string_to_digit
-----------------------

**Parameter description**: Specifies the implicit conversion priority, which determines whether to preferentially convert strings into numbers.

In MySQL-compatible mode, this parameter has no impact.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that strings are preferentially converted into numbers.
-  **off** indicates that strings are not preferentially converted into numbers.

**Default value**: **on**

.. important::

   Modify this parameter only when absolutely necessary because the modification will change the rule for converting internal data types and may cause unexpected results.

nls_timestamp_format
--------------------

**Parameter description**: Specifies the default timestamp format.

**Type**: USERSET

**Value range**: a string

**Default value**: **DD-Mon-YYYY HH:MI:SS.FF AM**

enable_partitionwise
--------------------

**Parameter description**: Specifies whether to select an intelligent algorithm for joining partitioned tables.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that an intelligent algorithm is selected.
-  **off** indicates that an intelligent algorithm is not selected.

**Default value**: **off**

enable_partition_dynamic_pruning
--------------------------------

**Parameter description**: Specifies whether dynamic pruning is enabled during partition table scanning.

**Type**: USERSET

**Value range**: Boolean

-  **on**: enable
-  **off**: disable

**Default value**: **on**

max_user_defined_exception
--------------------------

**Parameter description**: Specifies the maximum number of exceptions. The default value cannot be changed.

**Type**: USERSET

**Value range**: an integer

**Default value**: **1000**

datanode_strong_sync
--------------------

**Parameter description**: This parameter no longer takes effect.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that forcible synchronization between stream nodes is enabled.
-  **off** indicates that forcible synchronization between stream nodes is disabled.

**Default value**: **off**

enable_debug_vacuum
-------------------

**Parameter description**: Specifies whether to allow output of some VACUUM-related logs for problem locating. This parameter is used only by developers. Common users are advised not to use it.

**Type**: SIGHUP

**Value range**: Boolean

-  **on/true** indicates that output of vacuum-related logs is allowed.
-  **off/false** indicates that output of vacuum-related logs is disallowed.

**Default value**: **off**

enable_global_stats
-------------------

**Parameter description**: Specifies the current statistics mode. This parameter is used to compare global statistics generation plans and the statistics generation plans for a single DN. This parameter is used for tests. Users are not allowed to enable it.

**Type**: SUSET

**Value range**: Boolean

-  **on** or **true** indicates the global statistics mode.
-  **off** or **false** indicates the single-DN statistics mode.

**Default value**: **on**

enable_fast_numeric
-------------------

**Parameter description**: Specifies whether to enable optimization for numeric data calculation. Calculation of numeric data is time-consuming. Numeric data is converted into int64- or int128-type data to improve numeric data calculation performance.

**Type**: USERSET

**Value range**: Boolean

-  **on/true** indicates that optimization for numeric data calculation is enabled.
-  **off/false** indicates that optimization for numeric data calculation is disabled.

**Default value**: **on**

enable_row_fast_numeric
-----------------------

**Parameter description**: Specifies the format in which numeric data in a row-store table is spilled to disks.

**Type**: USERSET

**Value range**: Boolean

-  **on/true** indicates that numeric data in a row-store table is spilled to disks in bigint format.
-  **off/false** indicates that numeric data in a row-store table is spilled to disks in the original format.

.. important::

   If this parameter is set to **on**, you are advised to enable **enable_force_vector_engine** to improve the query performance of large data sets. However, compared with the original format, there is a high probability that the bigint format occupies more disk space. For example, the TPC-H test set occupies about 7% more space (reference value, may vary depending on the environment).

**Default value**: **off**

rewrite_rule
------------

**Parameter description**: Specifies the rewriting rule for enabled optional queries. Some query rewriting rules are optional. Enabling them cannot always improve query efficiency. In a specific customer scenario, you can set the query rewriting rules through the GUC parameter to achieve optimal query efficiency.

This parameter can control the combination of query rewriting rules, for example, there are multiple rewriting rules: rule1, rule2, rule3, and rule4. To set the parameters, you can perform the following operations:

.. code-block:: text

   set rewrite_rule=rule1;          --Enable query rewriting rule rule1.
   set rewrite_rule=rule2,rule3;    --Enable query rewriting rules rule2 and rule3.
   set rewrite_rule=none;           --Disable all optional query rewriting rules.

**Type**: USERSET

**Value range**: a string

-  **none**: Does not use any optional query rewriting rules.
-  **lazyagg**: Uses the Lazy Agg query rewriting rules for eliminating aggregation operations in subqueries.
-  **magicset**: Uses the Magic Set query rewriting rules (from the main query to subqueries).
-  **uniquecheck**: Uses the Unique Check rewriting rules. (The scenario where the target column does not contain the expression sublink of the aggregate function can be improved. The function can be enabled only when the value of the target column is unique after the sublink is aggregated based on the associated column. This function is recommended to be used by optimization engineers.)
-  **disablerep**: Uses the function that prohibit pulling up sublinks of the replication table. (Disable sublink pull-up for the replication table.)
-  **projection_pushdown**: Uses the Projection Pushdown rewriting rules.

**Default value**: **magicset**

enable_compress_spill
---------------------

**Parameter description**: Specifies whether to enable the compression function of writing data to a disk.

**Type**: USERSET

**Value range**: Boolean

-  **on/true** indicates that optimization for writing data to a disk is enabled.
-  **off/false** indicates that optimization for writing data to a disk is disabled.

**Default value**: **on**

analysis_options
----------------

**Parameter description**: Specifies whether to enable function options in the corresponding options to use the corresponding location functions, including data verification and performance statistics. For details, see the options in the value range.

**Type**: USERSET

**Value range**: a string

-  **LLVM_COMPILE** indicates that the codegen compilation time of each thread is displayed on the explain performance page.
-  **HASH_CONFLICT** indicates that the log file in the **pg_log** directory of the DN process displays the hash table statistics, including the hash table size, hash chain length, and hash conflict information.
-  **STREAM_DATA_CHECK** indicates that a CRC check is performed on data before and after network data transmission.

**Default value**: **off(ALL)**, which indicates that no location function is enabled.

resource_track_log
------------------

**Parameter description**: Specifies the log level of self-diagnosis. Currently, this parameter takes effect only in multi-column statistics.

**Type**: USERSET

**Value range**: a string

-  **summary**: Brief diagnosis information is displayed.
-  **detail**: Detailed diagnosis information is displayed.

Currently, the two parameter values differ only when there is an alarm about multi-column statistics not collected. If the parameter is set to **summary**, such an alarm will not be displayed. If it is set to **detail**, such an alarm will be displayed.

**Default value**: **summary**

hll_default_log2m
-----------------

**Parameter description**: Specifies the number of buckets for HLL data. The number of buckets affects the precision of distinct values calculated by HLL. The more buckets there are, the smaller the deviation is. The deviation range is as follows: [-1.04/2\ :sup:`log2m*1/2`, +1.04/2\ :sup:`log2m*1/2`]

**Type**: USERSET

**Value range**: an integer ranging from 10 to 16

**Default value**: **11**

hll_default_regwidth
--------------------

**Parameter description**: Specifies the number of bits in each bucket for HLL data. A larger value indicates more memory occupied by HLL. **hll_default_regwidth** and **hll_default_log2m** determine the maximum number of distinct values that can be calculated by HLL. For details, see :ref:`Table 1 <en-us_topic_0000001145894431__table05450516616>`.

**Type**: USERSET

**Value range**: an integer ranging from 1 to 5

**Default value**: **5**

.. _en-us_topic_0000001145894431__table05450516616:

.. table:: **Table 1** Maximum number of calculated distinct values determined by hll_default_log2m and hll_default_regwidth

   ===== ============ ============ ============ ============ ============
   log2m regwidth = 1 regwidth = 2 regwidth = 3 regwidth = 4 regwidth = 5
   ===== ============ ============ ============ ============ ============
   10    7.4e+02      3.0e+03      4.7e+04      1.2e+07      7.9e+11
   11    1.5e+03      5.9e+03      9.5e+04      2.4e+07      1.6e+12
   12    3.0e+03      1.2e+04      1.9e+05      4.8e+07      3.2e+12
   13    5.9e+03      2.4e+04      3.8e+05      9.7e+07      6.3e+12
   14    1.2e+04      4.7e+04      7.6e+05      1.9e+08      1.3e+13
   15    2.4e+04      9.5e+04      1.5e+06      3.9e+08      2.5e+13
   ===== ============ ============ ============ ============ ============

hll_default_expthresh
---------------------

**Parameter description**: Specifies the default threshold for switching from the **explicit** mode to the **sparse** mode.

**Type**: USERSET

**Value range**: an integer ranging from -1 to 7 **-1** indicates the auto mode; **0** indicates that the **explicit** mode is skipped; a value from 1 to 7 indicates that the mode is switched when the number of distinct values reaches 2\ :sup:`hll_default_expthresh`.

**Default value**: **-1**

hll_default_sparseon
--------------------

**Parameter description**: Specifies whether to enable the **sparse** mode by default.

**Type**: USERSET

**Valid value**: **0** and **1** **0** indicates that the **sparse** mode is disabled by default. **1** indicates that the **sparse** mode is enabled by default.

**Default value**: **1**

hll_max_sparse
--------------

**Parameter description**: Specifies the size of **max_sparse**.

**Type**: USERSET

**Value range**: an integer ranging from -1 to **INT_MAX**

**Default value**: **-1**

enable_compress_hll
-------------------

**Parameter description**: Specifies whether to enable memory optimization for HLL.

**Type**: USERSET

**Value range**: Boolean

-  **on** or **true** indicates that memory optimization is enabled.
-  **off** or **false** indicates that memory optimization is disabled.

**Default value**: **off**

.. _en-us_topic_0000001145894431__section1765913299426:

udf_memory_limit
----------------

**Parameter description**: Controls the maximum physical memory that can be used when each CN or DN executes UDFs.

**Type**: POSTMASTER

**Value range**: an integer. The value range is from 200 x 1024 to the value of :ref:`max_process_memory <en-us_topic_0000001145694783__sadc1e0e8c1c246a4a6cad3967deebaad>` and the unit is KB.

**Default value**: **200 MB**

FencedUDFMemoryLimit
--------------------

**Parameter description**: Controls the virtual memory used by each fenced udf worker process.

**Type**: USERSET

**Suggestion**: You are not advised to set this parameter. You can set :ref:`udf_memory_limit <en-us_topic_0000001145894431__section1765913299426>` instead.

**Value range**: an integer. The unit can be KB, MB, or GB. **0** indicates that the memory is not limited.

**Default value**: **0**

UDFWorkerMemHardLimit
---------------------

**Parameter description**: Specifies the maximum value of **fencedUDFMemoryLimit**.

**Type**: POSTMASTER

**Suggestion**: You are not advised to set this parameter. You can set :ref:`udf_memory_limit <en-us_topic_0000001145894431__section1765913299426>` instead.

**Value range**: an integer. The unit can be KB, MB, or GB.

**Default value**: **1 GB**

pljava_vmoptions
----------------

**Parameter description**: Specifies the startup parameters for JVMs used by the PL/Java function.

**Type**: SUSET

**Value range**: a string, supporting:

-  JDK8 JVM startup parameters.
-  JDK8 JVM system attributes (starting with **-D**, for example, **-Djava.ext.dirs**).
-  User-defined parameters (starting with **-D**, for example, **-Duser.defined.option**).

.. important::

   If **pljava_vmoptions** is set to a value beyond the value range, an error will be reported when PL/Java functions are used.

**Default value**: empty

javaudf_disable_feature
-----------------------

**Parameter description**: Specifies the granularity of Java UDF actions.

**Type**: SIGHUP

**Value range**: a string

-  **none** indicates that any action specified in other fine-grained parameters is enabled. When this parameter is set together with other parameters, **none** is invalid.
-  **all** indicates that all Java UDF functions are disabled. This option has the highest priority.
-  **extdir** indicates that the function of storing dependency JAR packages in a third-party path is disabled.
-  **hadoop** indicates that Hadoop functions are disabled.
-  **reflection** indicates that the reflection permission (**ReflectPermission**) is disabled during the execution of Java UDF functions.
-  **loadlibrary** indicates that the dynamic library loading permission (**loadLibrary**) is disabled during the execution of Java UDF functions.
-  **net** indicates that the network permission (**NetPermission**) is disabled during the execution of Java UDF functions.
-  **socket** indicates that the socket permission (**SocketPermission**) is disabled during the execution of Java UDF functions.
-  **security** indicates that the security configuration permission (**SecurityPermission**) is disabled during the execution of Java UDF functions.
-  **classloader** indicates that the **classLoder** creation permission (**createClassLoader**) is disabled during the execution of Java UDF functions.
-  **access_declared_members** indicates that the permission of accessing other declared members (**accessDeclaredMembers**) is disabled during the execution of Java UDF functions.

**Default value**: **extdir,hadoop,reflection,loadlibrary,net,socket,security,classloader,access_declared_members**

enable_pbe_optimization
-----------------------

**Parameter description**: Specifies whether the optimizer optimizes the query plan for statements executed in Parse Bind Execute (PBE) mode.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the optimizer optimizes the query plan.
-  **off** indicates that the optimization does not optimize the query plan.

**Default value**: **on**

enable_light_proxy
------------------

**Parameter description**: Specifies whether the optimizer optimizes the execution of simple queries on CNs.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the optimizer optimizes the execution.
-  **off** indicates that the optimization does not optimize the execution.

**Default value**: **on**

checkpoint_flush_after
----------------------

**Parameter description**: Specifies the number of consecutive disk pages that the checkpointer writer thread writes before asynchronous flush. In GaussDB(DWS), the size of a disk page is 8 KB.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 256. **0** indicates that the asynchronous flush function is disabled. For example, if the value is **32**, the checkpointer thread continuously writes 32 disk pages (that is, 32 x 8 = 256 KB) before asynchronous flush.

**Default value**: **32**

enable_parallel_ddl
-------------------

**Parameter description**: Controls whether multiple CNs can concurrently perform DDL operations on the same database object.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that DDL operations can be performed safely and that no distributed deadlock occurs.
-  **off** indicates that DDL operations cannot be performed safely and that distributed deadlocks may occur.

**Default value**: **on**

show_acce_estimate_detail
-------------------------

**Parameter description**: When the GaussDB(DWS) cluster is accelerated (:ref:`acceleration_with_compute_pool <en-us_topic_0000001099134808__section13787157164412>` is set to **on**), specifies whether the **EXPLAIN** statement displays the evaluation information about execution plan pushdown to computing Node Groups. The evaluation information is generally used by O&M personnel during maintenance, and it may affect the output display of the **EXPLAIN** statement. Therefore, this parameter is disabled by default. The evaluation information is displayed only if the **verbose** option of the **EXPLAIN** statement is enabled.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the evaluation information is displayed in the output of the **EXPLAIN** statement.
-  **off** indicates that the evaluation information is not displayed in the output of the **EXPLAIN** statement.

**Default value**: **off**

support_batch_bind
------------------

**Parameter description**: Specifies whether to batch bind and execute PBE statements through interfaces such as JDBC, ODBC, and Libpq.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that batch binding and execution are used.
-  **off** indicates that batch binding and execution are not used.

**Default value**: **on**

enable_immediate_interrupt
--------------------------

**Parameter description**: Specifies whether the execution of the current statement or session can be immediately interrupted in the signal processing function.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the execution of the current statement or session can be immediately interrupted in the signal processing function.
-  **off** indicates that the execution of the current statement or session cannot be immediately interrupted in the signal processing function.

**Default value**: **off**

.. note::

   Exercise caution when setting this parameter to **on**. If the execution of the current statement or session can be immediately interrupted in the signal processing function, the execution of some key processes may be interrupted, causing the failure to release the global lock in the system. It is recommended that this parameter be set to **on** only during system debugging or fault prevention.
