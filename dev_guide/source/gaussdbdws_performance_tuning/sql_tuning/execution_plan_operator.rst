:original_name: dws_04_0402.html

.. _dws_04_0402:

Execution Plan Operator
=======================

Operator Introduction
---------------------

In an SQL execution plan, each step indicates a database operator, also called an execution operator. In GaussDB(DWS), operators are the building blocks of data processing. By combining them effectively and optimizing their sequence and execution, you can significantly improve data processing efficiency.

GaussDB(DWS) operators are classified into scan operators, control operators, materialization operators, join operators, and other operators.

Scan Operators
--------------

A scan operator scans data in a table, processing one tuple at a time for the upper-layer node. It operates at the leaf node of the query plan tree and can scan tables, result sets, linked lists, and subquery results. The following table lists common scan operators.

.. table:: **Table 1** Scan operators

   +---------------------------------------------+----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | Operator                                    | Description                      | Scenario                                                                                                                                |
   +=============================================+==================================+=========================================================================================================================================+
   | SeqScan                                     | Sequential scanning              | It is a basic operator used to scan physical tables in sequence, not an index-assisted scan.                                            |
   +---------------------------------------------+----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | IndexScan                                   | Index scanning                   | Indexes are created for the attributes involved in selection conditions.                                                                |
   +---------------------------------------------+----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | IndexOnlyScan                               | Obtaining a tuple from an index  | The index column completely overwrites the result set column.                                                                           |
   +---------------------------------------------+----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | BitmapScan(BitmapIndexScan, BitmapHeapScan) | Obtaining a tuple using a bitmap | BitmapIndexScan uses indexes for attributes to scan data and returns a bitmap. BitmapHeapScan then uses this bitmap to retrieve tuples. |
   +---------------------------------------------+----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | TidScan                                     | Obtaining a tuple by tuple tid   | #. WHERE conditions(like CTID = tid or CTID IN (tid1, tid2, ...)) ;                                                                     |
   |                                             |                                  | #. UPDATE/DELETE ... WHERE CURRENT OF cursor;                                                                                           |
   +---------------------------------------------+----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | SubqueryScan                                | Subquery scanning                | Another query plan tree (subplan) is used as the scanning object to scan tuples.                                                        |
   +---------------------------------------------+----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | FunctionScan                                | Function scanning                | FROM function_name                                                                                                                      |
   +---------------------------------------------+----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | ValuesScan                                  | Values linked list scanning      | It scans the given tuple set in VALUES clauses.                                                                                         |
   +---------------------------------------------+----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | ForeignScan                                 | External table scanning          | It queries external tables.                                                                                                             |
   +---------------------------------------------+----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | CteScan                                     | CTE table scanning               | It scans the subquery defined by the WITH clause in the SELECT query.                                                                   |
   +---------------------------------------------+----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+

Join Operators
--------------

In relational algebra, a join operation is equivalent to a join operator. Take a simple example: joining two tables, t1 and t2. There are several types of joins, including inner join, left join, right join, full join, semi join, and anti join. These joins can be implemented using three methods: Nestloop, HashJoin, and MergeJoin.

.. table:: **Table 2** Join operators

   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operator          | Description                                                                                                                                                                                                                 | Scenario                                                                             | Implementation Feature                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   +===================+=============================================================================================================================================================================================================================+======================================================================================+====================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | NestLoop          | Nested loop join, which is a brute force approach. It scans the inner table for each row.                                                                                                                                   | Inner Join, Left Outer Join, Semi Join, Anti Join                                    | It is used for queries that have a smaller subset connected. In a nested loop, the foreign table drives the internal table. Each row returned by the foreign table is retrieved from the internal table to find the matched row. Therefore, the result set returned by the entire query cannot be greater than 10,000. The table with a smaller subset returned is used as the foreign table. It is recommended that indexes be created for the join fields in the internal table. |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | MergeJoin         | A merge join on ordered input sorts both the inner and outer tables, identifies the first and last matching rows, and then joins tuples at a time. Equi-join.                                                               | Inner Join, Left Outer Join, Right Outer Join, Full Outer Join, Semi Join, Anti Join | In a merge join, data in the two joined tables is sorted by join columns. Then, data is extracted from the two tables to a sorted table for matching.                                                                                                                                                                                                                                                                                                                              |
   |                   |                                                                                                                                                                                                                             |                                                                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   |                   |                                                                                                                                                                                                                             |                                                                                      | A merge join requires more resources for sorting and its performance is lower than that of a hash join. However, if the source data has been pre-sorted and no more sorting is needed during the merge join, its performance excels.                                                                                                                                                                                                                                               |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | (Sonic) Hash Join | Hash join: The inner and outer tables use the join column's hash value to create a hash table. Matching values are then stored in the same bucket. The two ends of an equal join must be of the same type and support hash. | Inner Join, Left Outer Join, Right Outer Join, Full Outer Join, Semi Join, Anti Join | A hash Join is used for large tables. The optimizer creates a hash table in memory using the join key and the smaller table. It then scans the larger table and uses the hash table to quickly identify matching rows. While Sonic and non-Sonic hash joins have different internal structures, this does not impact the final result set.                                                                                                                                         |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Materialization Operators
-------------------------

Materialization operators are a class of nodes that can cache tuples. During execution, many extended physical operations can be performed only after all tuples are obtained, such as aggregation function operations and sorting without indexes. Materialization operators can cache all the tuples.

.. table:: **Table 3** Materialization operators

   +-----------------------+----------------------------------------------+-------------------------------------------------------------------------------------------+
   | Operator              | Description                                  | Scenario                                                                                  |
   +=======================+==============================================+===========================================================================================+
   | Material              | Materialization                              | Caches the subnode result.                                                                |
   +-----------------------+----------------------------------------------+-------------------------------------------------------------------------------------------+
   | Sort                  | Sorting                                      | ORDER BY clause, which is used for join, group, and set operations and works with Unique. |
   +-----------------------+----------------------------------------------+-------------------------------------------------------------------------------------------+
   | Group                 | Grouping                                     | GROUP BY clause.                                                                          |
   +-----------------------+----------------------------------------------+-------------------------------------------------------------------------------------------+
   | Agg                   | Executes aggregate functions.                | #. Aggregate functions such as COUNT, SUM, AVG, MAX, and MIN.                             |
   |                       |                                              | #. DISTINCT clause.                                                                       |
   |                       |                                              | #. UNION deduplication.                                                                   |
   |                       |                                              | #. GROUP BY clause.                                                                       |
   +-----------------------+----------------------------------------------+-------------------------------------------------------------------------------------------+
   | WindowAgg             | Window functions                             | WINDOW clause.                                                                            |
   +-----------------------+----------------------------------------------+-------------------------------------------------------------------------------------------+
   | Unique                | Deduplication (with sorted lower-layer data) | #. DISTINCT clause.                                                                       |
   |                       |                                              | #. UNION deduplication.                                                                   |
   +-----------------------+----------------------------------------------+-------------------------------------------------------------------------------------------+
   | Hash                  | HashJoin auxiliary node                      | Constructs a hash table and use it together with HashJoin.                                |
   +-----------------------+----------------------------------------------+-------------------------------------------------------------------------------------------+
   | SetOp                 | Processing set operations                    | INTERSECT/INTERSECT ALL, EXCEPT/EXCEPT ALL                                                |
   +-----------------------+----------------------------------------------+-------------------------------------------------------------------------------------------+
   | LockRows              | Processing row-level locks                   | SELECT ... FOR SHARE/UPDATE                                                               |
   +-----------------------+----------------------------------------------+-------------------------------------------------------------------------------------------+

Control Operators
-----------------

Control operators are a type of node that handles exceptional scenarios and executes custom workflows.

.. table:: **Table 4** Control operators

   +-----------------------+----------------------------------------------------------------------+------------------------------------------------------------------+
   | Operator              | Description                                                          | Scenario                                                         |
   +=======================+======================================================================+==================================================================+
   | Result                | Performing calculation directly                                      | #. Table scanning is not included.                               |
   |                       |                                                                      | #. The **INSERT** statement contains only one **VALUES** clause. |
   +-----------------------+----------------------------------------------------------------------+------------------------------------------------------------------+
   | ModifyTable           | INSERT/UPDATE/DELETE upper-layer node                                | **INSERT**, **UPDATE**, and **DELETE**                           |
   +-----------------------+----------------------------------------------------------------------+------------------------------------------------------------------+
   | Append                | Appending                                                            | #. **UNION(ALL)**                                                |
   |                       |                                                                      | #. Table inheritance                                             |
   +-----------------------+----------------------------------------------------------------------+------------------------------------------------------------------+
   | MergeAppend           | Appending (ordered input)                                            | #. **UNION(ALL)**                                                |
   |                       |                                                                      | #. Table inheritance                                             |
   +-----------------------+----------------------------------------------------------------------+------------------------------------------------------------------+
   | RecursiveUnion        | Processing the UNION subquery defined recursively in the WITH clause | **WITH RECURSIVE... SELECT...** statement                        |
   +-----------------------+----------------------------------------------------------------------+------------------------------------------------------------------+
   | BitmapAnd             | Bitmap logical AND operation                                         | BitmapScan for multi-dimensional index scanning                  |
   +-----------------------+----------------------------------------------------------------------+------------------------------------------------------------------+
   | BitmapOr              | Bitmap logical OR operation                                          | BitmapScan for multi-dimensional index scanning                  |
   +-----------------------+----------------------------------------------------------------------+------------------------------------------------------------------+
   | Limit                 | Processing the LIMIT clause                                          | **OFFSET ... LIMIT ...**                                         |
   +-----------------------+----------------------------------------------------------------------+------------------------------------------------------------------+

Other Operators
---------------

Other operators include Stream and RemoteQuery. There are three types of Stream operators: Gather stream, Broadcast stream, and Redistribute stream.

-  Gather stream: Each source node sends its data to the target node for aggregation.
-  Broadcast stream: A source node sends its data to N target nodes for calculation.
-  Redistribute stream: Each source node calculates the hash value of its data based on the join condition, distributes the data based on the hash value, and sends the data to the corresponding target node.

.. table:: **Table 5** Other Operators

   +------------------------+-----------------------------+-----------------------------------------------------------------------------+
   | Operator               | Description                 | Scenario                                                                    |
   +========================+=============================+=============================================================================+
   | Stream                 | Multi-node data exchange    | When a distributed query plan is executed, data is exchanged between nodes. |
   +------------------------+-----------------------------+-----------------------------------------------------------------------------+
   | Partition Iterator     | Partition iterator          | Scans partitioned tables and iteratively scans each partition.              |
   +------------------------+-----------------------------+-----------------------------------------------------------------------------+
   | RowToVec               | Rows-to-column conversion   | Hybrid row-column.                                                          |
   +------------------------+-----------------------------+-----------------------------------------------------------------------------+
   | DfsScan / DfsIndexScan | HDFS table (index) scanning | HDFS table scanning.                                                        |
   +------------------------+-----------------------------+-----------------------------------------------------------------------------+
