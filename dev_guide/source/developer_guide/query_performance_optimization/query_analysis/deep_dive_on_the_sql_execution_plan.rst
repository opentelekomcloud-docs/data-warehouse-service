:original_name: dws_04_0411.html

.. _dws_04_0411:

Deep Dive on the SQL Execution Plan
===================================

As described in :ref:`Overview of the SQL Execution Plan <en-us_topic_0000001147351103>`, **EXPLAIN** displays the execution plan, but will not actually run SQL statements. **EXPLAIN ANALYZE** and **EXPLAIN PERFORMANCE** both will actually run SQL statements and return the execution information. In this section, detailed execution plan and execution information are described.

.. _en-us_topic_0000001147151251__en-us_topic_0000001145814375_s94ac9b9e142d4be28d0288e0f3ba4a64:

Execution Plan
--------------

The following SQL statement is used as an example:

::

   select
       cjxh,
       count(1)
   from dwcjk
   group by cjxh;

Run the **EXPLAIN** command and the output is as follows:

|image1|

**Interpretation of the execution plan column (horizontal)**:

-  Id: execution operator node ID

-  operation: execution node operator name

   The operator of the Vector prefix refers to a vectorized execution engine operator, which exists in a query containing a column-store table.

   Streaming is a special operator. It implements the core data shuffle function of the distributed architecture. Streaming has three types, which correspond to different data shuffle functions in the distributed architecture:

   -  Streaming (type: GATHER): The CN collects data from DNs.
   -  Streaming(type: REDISTRIBUTE): Data is redistributed to all the DNs based on selected columns.
   -  Streaming(type: BROADCAST): Data on the current DN is broadcast to other DNs.

-  E-rows: indicates the number of rows estimated by each operator.

-  E-memory: indicates the estimated memory used by each operator on a DN. Only operators executed on DNs are displayed. In certain scenarios, the memory upper limit enclosed in parentheses will be displayed following the estimated memory usage.

-  E-width: indicates the estimated width of tuples provided by each operator.

-  E-costs: indicates the execution cost estimated by each operator.

   -  E-costs are defined by the optimizer based on cost parameters, habitually grasping disk page as a unit. Other overhead parameters are set by referring to E-costs.
   -  The cost of each node (the E-costs value) includes the cost of all of its child nodes.
   -  Overhead reflects only what the optimizer is concerned about, but does not consider the time that the result row passed to the client. Although the time may play a very important role in the actual total time, it is ignored by the optimizer, because it cannot be changed by modifying the plan.

**Interpretation of the execution plan level (vertical)**:

#. Layer 1: CStore Scan on dwcjk

   The table scan operator scans the table **dwcjk** using Cstore Scan. The function of this layer is to read data in the table dwcjk from the buffer or disks, or transfers it to the upper-layer node to participate in the calculation.

#. Layer 2: Vector Hash Aggregate

   Aggregation operators are used to perform aggregation operations (group by) on operators calculated from the lower layer.

#. Layer 3: Vector Streaming (type: GATHER)

   The GATHER-typed Shuffle operator aggregates data from DNs to the CN.

#. Layer 4: Row Adapter

   Storage format conversion operator is used to convert data in columns of the memory to data in rows for client display.

It should be noted that when operators in the top layer are Data Node Scan, you need to set **enable_fast_query_shipping** to **off** to view detailed execution plan as follows:

|image2|

After **enable_fast_query_shipping** is set, the execution plan is displayed as follows:

|image3|

**Keywords in the execution plan**:

#. Table access modes

   -  Seq Scan

      Scans all rows of the table in sequence.

   -  Index Scan

      The optimizer uses a two-step plan: the child plan node visits an index to find the locations of rows matching the index condition, and then the upper plan node actually fetches those rows from the table itself. Fetching rows separately is much more expensive than reading them sequentially, but because not all pages of the table have to be visited, this is still cheaper than a sequential scan. The upper-layer planning node first sort the location of index identifier rows based on physical locations before reading them. This minimizes the independent capturing overhead.

      If there are separate indexes on multiple columns referenced in **WHERE**, the optimizer might choose to use an **AND** or **OR** combination of the indexes. However, this requires the visiting of both indexes, so it is not necessarily a win compared to using just one index and treating the other condition as a filter.

      The following Index scans featured with different sorting mechanisms are involved:

      -  Bitmap Index Scan

         Fetches data pages using a bitmap.

      -  Index Scan using index_name

         Fetches table rows in index order, which makes them even more expensive to read. However, there are so few rows that the extra cost of sorting the row locations is unnecessary. This plan type is used mainly for queries fetching just a single row and queries having an **ORDER BY** condition that matches the index order, because no extra sorting step is needed to satisfy **ORDER BY**.

#. Table connection modes

   -  Nested Loop

      Nested-loop is used for queries that have a smaller data set connected. In a Nested-loop join, the foreign table drives the internal table and each row returned from the foreign table should have a matching row in the internal table. The returned result set of all queries should be less than 10,000. The table that returns a smaller subset will work as a foreign table, and indexes are recommended for connection fields of the internal table.

   -  (Sonic) Hash Join

      A Hash join is used for large tables. The optimizer uses a hash join, in which rows of one table are entered into an in-memory hash table, after which the other table is scanned and the hash table is probed for matches to each row. Sonic and non-Sonic hash joins differ in their hash table structures, which do not affect the execution result set.

   -  Merge Join

      In a merge join, data in the two joined tables is sorted by join columns. Then, data is extracted from the two tables to a sorted table for matching.

      Merge join requires more resources for sorting and its performance is lower than that of hash join. If the source data has been sorted, it does not need to be sorted again when merge join is performed. In this case, the performance of merge join is better than that of hash join.

#. Operators

   -  sort

      Sorts the result set.

   -  filter

      The **EXPLAIN** output shows the **WHERE** clause being applied as a **Filter** condition attached to the **Seq Scan** plan node. This means that the plan node checks the condition for each row it scans, and returns only the ones that meet the condition. The estimated number of output rows has been reduced because of the **WHERE** clause. However, the scan will still have to visit all 10000 rows. As a result, the cost is not decreased. It increases a bit (by 10000 x **cpu_operator_cost**) to reflect the extra CPU time spent on checking the **WHERE** condition.

   -  LIMIT

      **LIMIT** limits the number of output execution results. If a **LIMIT** condition is added, not all rows are retrieved.

Task Execution
--------------

You can use **EXPLAIN ANALYZE** or **EXPLAIN PERFORMANCE** to check the SQL statement execution information and compare the actual execution and the optimizer's estimation to find what to optimize. **EXPLAIN PERFORMANCE** provides the execution information on each DN, whereas **EXPLAIN ANALYZE** does not.

The following SQL statement is used as an example:

::

   select count(1) from tb1;

The output of running **EXPLAIN PERFORMANCE** is as follows:

|image4|

|image5|

|image6|

|image7|

This figure shows that the execution information can be classified into the following 7 aspects.

#. The plan is displayed as a table, which contains 11 columns: **id**, **operation**, **A-time**, **A-rows**, **E-rows**, **E-distinct**, **Peak Memory**, **E-memory**, **A-width**, **E-width**, and **E-costs**. The definition of the plan-type columns (columns started with id, operation, or some started with E) is the same as that of running **EXPLAIN**. For details, see :ref:`Execution Plan <en-us_topic_0000001147151251__en-us_topic_0000001145814375_s94ac9b9e142d4be28d0288e0f3ba4a64>` (execution plan) in the section. The definition of A-time, A-rows, E-distinct, Peak Memory, and A-width are described as follows:

   -  A-time: indicates the execution completion time of the current operator. Generally, the A-time of the operator executed on the DN is two values enclosed by square brackets ([]), indicating the shortest time and longest time for completing the operator on all DNs, respectively.
   -  A-rows: indicates the number of tuples provided by the current operator
   -  E-distinct: indicates the estimated distinct value of the hashjoin operator.
   -  Peak Memory: indicates the peak memory usage of an operator on each DN.
   -  A-width: indicates that the current operator tuple actual width of each line. This parameter is valid only for the heavy memory operator is displayed, including: (Vec)HashJoin, (Vec)HashAgg, (Vec) HashSetOp, (Vec)Sort, and (Vec)Materialize operator. The (Vec)HashJoin calculation of width is the width of the right subtree operator, it will be displayed in the right subtree.

#. Predicate Information (identified by plan id):

   This part displays the static information that does not change during the plan execution process, such as some join conditions and filter information.

#. Memory Information (identified by plan id):

   This part displays the memory usage information printed by certain operators (mainly Hash and Sort), including **peak memory**, **control memory**, **operator memory**, **width**, **auto spread num**, and **early spilled**; and spill details, including **spill Time(s)**, **inner/outer partition spill num**, **temp file num**, split data volume, and **written disk IO [**\ *min, max*\ **]**. The Sort operator does not display the number of files written to disks, and displays disks only when displaying sorting methods.

#. Targetlist Information (identified by plan id):

   This part displays the target columns provided by each operator.

#. DataNode Information (identified by plan id):

   This part displays the execution time of each operator (including the execution time of filtering and projection, if any), CPU usage, and buffer usage.

#. User Define Profiling:

   This part displays CNs and DNs, DN and DN connection time, and some execution information in the storage layer.

#. ====== Query Summary =====:

   The total execution time and network traffic, including the maximum and minimum execution time in the initialization and end phases on each DN, initialization, execution, and time in the end phase on each CN, and the system available memory during the current statement execution, and statement estimation memory information.

.. important::

   -  The difference between A-rows and E-rows shows the deviation between the optimizer estimation and actual execution. Generally, if the deviation is large, the plan generated by the optimizer is untrusted, and you need to modify the deviation value.
   -  If the difference of the A-time values is large, it indicates that the operator computing skew (difference between execution time on DNs) is large and that manual performance tuning is required. Generally, for two adjacent operators, the execution time of the upper-layer operator includes that of the lower-layer operator. However, if the upper-layer operator is a stream operator, its execution time may be less than that of the lower-layer operator, as there is no driving relationship between threads.
   -  **Max Query Peak Memory** is often used to estimate the consumed memory of SQL statements, and is also used as an important basis for setting a memory parameter during SQL statement optimization. Generally, the output from **EXPLAIN ANALYZE** or **EXPLAIN PERFORMANCE** is provided for the input for further optimization.

.. |image1| image:: /_static/images/en-us_image_0000001145814971.png
.. |image2| image:: /_static/images/en-us_image_0000001098975108.png
.. |image3| image:: /_static/images/en-us_image_0000001145695045.png
.. |image4| image:: /_static/images/en-us_image_0000001099135094.png
.. |image5| image:: /_static/images/en-us_image_0000001098655292.png
.. |image6| image:: /_static/images/en-us_image_0000001145895093.png
.. |image7| image:: /_static/images/en-us_image_0000001098815108.png
