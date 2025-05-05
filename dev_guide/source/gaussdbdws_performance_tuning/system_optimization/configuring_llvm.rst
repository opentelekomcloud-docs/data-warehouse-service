:original_name: dws_04_0426.html

.. _dws_04_0426:

Configuring LLVM
================

LLVM dynamic compilation can be used to generate customized machine code for each query to replace original common functions. The query performance is improved by reducing redundant judgment condition and virtual function invocation, and make local data more accurate during actual queries.

LLVM needs to consume extra time to pre-generate intermediate representation (IR) and compile it into code. Therefore, if the data volume is small or if a query itself consumes little time, LLVM actually does more harm than good.

LLVM Application Scenarios and Constraints
------------------------------------------

**Applicable Scenarios**

-  Expressions supporting LLVM. The query statements that contain the following expressions support LLVM optimization:

   #. CASE...WHEN...
   #. IN
   #. Bool (AND/OR/NOT)
   #. BooleanTest (IS_NOT_KNOWN/IS_UNKNOWN/IS_TRUE/IS_NOT_TRUE/IS_FALSE/IS_NOT_FALSE)
   #. NullTest (IS_NOT_NULL/IS_NULL)
   #. Operators
   #. Functions (lpad, substring, btrim, rtrim, and length)
   #. Nullif

   The following data types are supported for expression calculation: bool, tinyint, smallint, int, bigint, float4, float8, numeric, date, time, timetz, timestamp, timestamptz, interval, bpchar, varchar, text, and oid.

   Consider using LLVM dynamic compilation and optimization only when expressions are used in the following scenarios:

   -  **filter** on the **Scan** node in the case of a vectorized executor.
   -  **complicate hash condition**, **hash join filter**, and **hash join target** in the **Hash Join** node.
   -  **filter** and **join filter** in the **Nested Loop** node.
   -  **merge join filter** and **merge join target** in the **Merge Join** node.
   -  **filter** in the Group node.

-  Operators that can use LLVM:

   #. Join: HashJoin
   #. Agg: HashAgg
   #. Sort

   Among them:

   -  HashJoin supports only Hash Inner Join, and the corresponding hash cond supports comparisons between int4, bigint, and bpchar.
   -  HashAgg supports sum and avg operations of bigint and numeric data types. Group By statements support int4, bigint, bpchar, text, varchar, timestamp, and the count(*) aggregation operation.
   -  Sort supports only comparisons between int4, bigint, numeric, bpchar, text, and varchar data types.

   With the exception of the operations above, LLVM dynamic compilation and optimization cannot be used. To further confirm, use the explain performance tool to check.

**Non-Applicable Scenarios**

-  LLVM dynamic compilation and optimization are not supported on CNs.
-  Tables that have small amounts of data cannot be dynamically compiled using LLVM.
-  Query jobs with a non-vectorized execution path cannot be generated.

Other Factors Impacting LLVM Performance
----------------------------------------

The result of LLVM optimization depends not only on operations and computation in the database, but also on the hardware environment.

-  Number of C- functions invoked by query statements

   CodeGen cannot be used in all expressions in an entire expression, that is, some expressions use CodeGen while others invoke original C codes for computation. In an entire expression, if more expressions invoke original C codes, LLVM dynamic compilation and optimization may reduce the computational performance. By setting **log_min_messages** to **DEBUG1**, you can check expressions that directly invoke C codes.

-  Memory resources

   One of the key LLVM features is to ensure the locality of data, that is, data should be stored in registers whenever possible. Data loading should be reduced at the same time. Therefore, when using LLVM optimization, the value of work_mem must be set as large as required to ensure that the code is processed in the memory using LLVM. Otherwise, performance may deteriorate.

-  Optimizer cost estimation

   The LLVM feature realizes a simple cost estimation model. You can determine whether to use LLVM dynamic compilation and optimization for the current node based on the sizes of tables involved in node computation. If the optimizer understates the actual number of rows involved, the expected performance gains may not be realized. An overestimation will have the same effect.

Recommended Usage of LLVM
-------------------------

LLVM is enabled in the database kernel by default, and users can configure it based on the analysis above. The overall suggestions are as follows:

#. Set an appropriate value for **work_mem** and set it as large as possible. If much data is flushed to disks, you are advised to disable LLVM dynamic compilation and optimization by setting **enable_codegen** to **off**.
#. Set an appropriate value for **codegen_cost_threshold** (The default value is 10,000). Ensure that LLVM dynamic compilation and optimization is not used when the data volume is small. After the value is set, if the database performance deteriorates due to the use of LLVM dynamic compilation and optimization, increase the value.
#. If a large number of C- functions are invoked, you are advised to disable LLVM dynamic compilation and optimization.
#. The constants following the **In** expression cannot exceed 10. Otherwise, LLVM compilation and optimization cannot be performed.

   .. note::

      If resources are sufficient, the database performance will improve as the data volume increases.
