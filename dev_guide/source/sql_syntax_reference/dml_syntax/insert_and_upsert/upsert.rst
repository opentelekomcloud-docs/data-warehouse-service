:original_name: dws_06_0237.html

.. _dws_06_0237:

UPSERT
======

Function
--------

**UPSERT** inserts rows into a table. When a row duplicates an existing primary key or unique key value, the row will be ignored or updated.

.. important::

   The **UPSERT** syntax is supported only in 8.1.1 and later.

Syntax
------

For details, see :ref:`Syntax <en-us_topic_0000001098990904__se26969fe97994814b5f45a6173164204>` of **INSERT**. The following table describes the syntax of **UPSERT**.

.. _en-us_topic_0000001145910983__table663035101813:

.. table:: **Table 1** UPSERT syntax

   +--------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+--------------------------------------------------------------+
   | Syntax                                                                                                 | Update Data Upon Conflict                                       | Ignore Data Upon Conflict                                    |
   +========================================================================================================+=================================================================+==============================================================+
   | Syntax 1: No index is specified.                                                                       | .. code-block::                                                 | .. code-block::                                              |
   |                                                                                                        |                                                                 |                                                              |
   |                                                                                                        |    INSERT INTO ON DUPLICATE KEY UPDATE                          |    INSERT IGNORE                                             |
   |                                                                                                        |                                                                 |    INSERT INTO ON CONFLICT DO NOTHING                        |
   +--------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+--------------------------------------------------------------+
   | Syntax 2: The unique key constraint can be inferred from the specified column name or constraint name. | .. code-block::                                                 | .. code-block::                                              |
   |                                                                                                        |                                                                 |                                                              |
   |                                                                                                        |    INSERT INTO ON CONFLICT(...) DO UPDATE SET                   |    INSERT INTO ON CONFLICT(...) DO NOTHING                   |
   |                                                                                                        |    INSERT INTO ON CONFLICT ON CONSTRAINT con_name DO UPDATE SET |    INSERT INTO ON CONFLICT ON CONSTRAINT con_name DO NOTHING |
   +--------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+--------------------------------------------------------------+

In syntax 1, no index is specified. The system checks for conflicts on all primary keys or unique indexes. If a conflict exists, the system ignores or updates the corresponding data.

In syntax 2, a specified index is used for conflict check. The primary key or unique index is inferred from the column name, the expression that contains column names, or the constraint name specified in the **ON CONFLICT** clause.

-  Unique index inference

   Syntax 2 infers the primary key or unique index by specifying the column name or constraint name. You can specify a single column name or multiple column names by using an expression, for example, **(column1, column2, column3)**.

   **collation** and **opclass** can be specified when you create an index. Therefore, you can also specify them after the column name for index inference.

   **COLLATE collation** specifies the collation of a column, and **opclass** specifies the name of the operator class. For details, see :ref:`CREATE INDEX <dws_06_0165>`.

   When inferring the unique index from an expression that includes multiple column names, the system checks whether there is a unique index that exactly contains all the column names specified by **conflict_target**.

   -  If **collation** and **opclass** are not specified, a match is considered found as long as a column has the same name as the specified single column or multiple columns have the same names as those specified by the column expression (regardless of the values of **collation** and **opclass** specified for the index column).
   -  If **collation** and **opclass** are specified, their values must also match the **collation** and **opclass** of the index.

-  **UPDATE** clause

The **UPDATE** clause can use **VALUES(colname)** or **EXCLUDED.colname** to reference inserted data. **EXCLUDED** indicates the rows that should be excluded due to conflicts. An example is as follows:

::

   CREATE TABLE t1(id int PRIMARY KEY, a int, b int);
   INSERT INTO t1 VALUES(1,1,1);
   -- Upon a conflicting row, change the value in column a to the value in column a of the target table plus 1, which, in this example, is (1,2,1).
   INSERT INTO t1 VALUES(1,10,20) ON CONFLICT(id) DO UPDATE SET a = a + 1;
   -- EXCLUDED.a is used to reference the value of column a that is originally proposed for insertion. In this example, the value is 10.
   -- Upon a conflicting row, change the value of column a to that of the referenced column plus 1. In this example, the value is updated to (1,11,1).
   INSERT INTO t1 VALUES(1,10,20) ON CONFLICT(id) DO UPDATE SET a = EXCLUDED.a + 1;

-  **WHERE** clause

   -  The **WHERE** clause is used to determine whether a specified condition is met when data conflict occurs. If yes, update the conflict data. Otherwise, ignore it.
   -  Only syntax 2 of **Update Data Upon Conflict** can specify the **WHERE** clause, that is, **INSERT INTO ON CONFLICT(...) DO UPDATE SET WHERE**.

Note the following when using the syntax:

-  Syntax 1 and syntax 2 described in :ref:`Table 1 <en-us_topic_0000001145910983__table663035101813>` cannot be used together in the same statement.
-  The **WITH** clause cannot be used at the same time.
-  **INSERT OVERWRITE** cannot be used at the same time.
-  The **UPDATE** clause and its **WHERE** clause do not support subqueries.
-  **VALUES(colname)** in the **UPDATE** clause does not support outer nested functions. That is, the usage similar to **sqrt(VALUES(colname))** is not supported. To support this function, use the **EXCLUDED.colname** syntax.
-  **INSERT INTO ON CONFLICT(...) DO UPDATE** must contain **conflict_target**. That is, a column or constraint name must be specified.

Precautions
-----------

-  When running **UPSERT** on a column-store table, you are advised to enable the **DELTA** table. If the **DELTA** table is disabled, concurrency will be affected and the space will be incurred.

-  Only users with the **INSERT** or **UPDATE** permission on a table can run the **UPSERT** statement to insert data to or update data in the table.

-  The **UPSERT** statement of updating data upon conflict can be executed only when the target table contains a primary key or unique index.

-  The **UPSERT** statement of updating data upon conflict cannot be executed if no unique indexes are available. You can execute the statement only after the indexes are rebuilt.

-  A distributed deadlock may occur, resulting in query hanging.

   .. note::

      For example, multiple **UPSERT** statements are executed in batches in a transaction or through JDBC (**setAutoCommit(false)**). Multiple similar tasks are executed at the same time.

      Possible result: The update sequences of different threads may vary depending on nodes. As a result, a deadlock may occur when the same row is concurrently updated.

      Solution:

      #. Decrease the value of the GUC parameter **lockwait_timeout**. The default value is 20 minutes. A distributed deadlock error will be reported after waiting for *the value of* **lockwait_timeout**. You can decrease the value of this parameter to reduce the service waiting time caused by a deadlock.
      #. Ensure that data with the same primary key is imported from only one database to the database. **UPSERT** statements can be executed concurrently.
      #. Only one **UPSERT** statement is executed in each transaction. **UPSERT** statements can be executed concurrently.
      #. Multiple **UPSERT** statements can be executed in a single thread. **UPSERT** statements cannot be executed concurrently.

      In the preceding solution, method 1 can only reduce the waiting time but cannot solve the deadlock problem. If there are **UPSERT** statements in the service, you are advised to decrease the value of this parameter. Methods 2, 3, and 4 can solve the deadlock problem, but method 2 is recommended because its performance is better than another two methods.

-  The distribution column cannot be updated. (Exception: Update is allowed if the distribution key is the same as the updated value.)

   ::

      CREATE TABLE t1(dist_key int PRIMARY KEY, a int, b int);
      INSERT INTO t1 VALUES(1,2,3) ON CONFLICT(dist_key) DO UPDATE SET dist_key = EXCLUDED.dist_key, a = EXCLUDED.a + 1;
      INSERT INTO t1 VALUES(1,2,3) ON CONFLICT(dist_key) DO UPDATE SET dist_key = dist_key, a = EXCLUDED.a + 1;

-  The **UPSERT** statement cannot be executed on the target table that contains a trigger (with the **INSERT** or **UPDATE** trigger event).

-  The **UPSERT** statement is not supported for updatable views.

-  The **UPDATE** clause, the **WHERE** clause of **UPDATE**, and the index condition expression should not contain functions that cannot be pushed down.

-  Unique indexes cannot be deferred.

-  The update data upon conflict statement of **UPSERT** cannot be executed on column-store replication tables.

-  When performing the update operation of **UPSERT** using **INSERT INTO SELECT**, pay attention to the query result sequence of **SELECT**. In a distributed environment, if the **ORDER BY** statement is not used, the sequence of returned results may be different each time the same **SELECT** statement is executed. As a result, the execution result of the **UPSERT** statement does not meet the expectation.

-  Multiple updates are not supported. An error will be reported if the inserted multiple groups of data conflict with each other. (Exception: No error will be reported if the query plan is a PGXC plan.)

   ::

      CREATE TABLE t1(id int PRIMARY KEY, a int, b int);
      -- Use the stream query plan:
      EXPLAIN (COSTS OFF) INSERT INTO t1 VALUES(1,2,3),(1,5,6) ON CONFLICT(id) DO UPDATE SET a = EXCLUDED.a + 1;
                         QUERY PLAN
      ------------------------------------------------
        id |                operation
       ----+-----------------------------------------
         1 | ->  Streaming (type: GATHER)
         2 |    ->  Insert on t1
         3 |       ->  Streaming(type: REDISTRIBUTE)
         4 |          ->  Values Scan on "*VALUES*"
       Predicate Information (identified by plan id)
       ---------------------------------------------
         2 --Insert on t1
               Conflict Resolution: UPDATE
               Conflict Arbiter Indexes: t1_pkey
         ====== Query Summary =====
       ------------------------------
       System available mem: 819200KB
       Query Max mem: 819200KB
       Query estimated mem: 3104KB
      (18 rows)
      INSERT INTO t1 VALUES(1,2,3),(1,5,6) ON CONFLICT(id) DO UPDATE SET a = EXCLUDED.a + 1;
      ERROR:  INSERT ON CONFLICT DO UPDATE command cannot affect row a second time
      HINT:  Ensure that no rows proposed for insertion within the same command have duplicate constrained values.
      -- Disable the stream plan and generate a PGXC plan:
      set enable_stream_operator = off;
      EXPLAIN (COSTS OFF) INSERT INTO t1 VALUES(1,2,3),(1,5,6) ON CONFLICT(id) DO UPDATE SET a = EXCLUDED.a + 1;
                        QUERY PLAN
      -----------------------------------------------
        id |            operation
       ----+----------------------------------
         1 | ->  Insert on t1
         2 |    ->  Values Scan on "*VALUES*"
       Predicate Information (identified by plan id)
       ---------------------------------------------
         1 --Insert on t1
               Conflict Resolution: UPDATE
               Conflict Arbiter Indexes: t1_pkey
               Node expr: id
      (11 rows)
      INSERT INTO t1 VALUES(1,2,3),(1,5,6) ON CONFLICT(id) DO UPDATE SET a = EXCLUDED.a + 1;
      INSERT 0 2

Examples
--------

Create table **reason_t2** and insert data into it.

::

   CREATE TABLE reason_t2
   (
     a    int primary key,
     b    int,
     c    int
   );

   INSERT INTO reason_t2 VALUES (1, 2, 3);
   SELECT * FROM reason_t2 ORDER BY 1;
    a | b | c
   ---+---+---
    1 | 2 | 3
    (1 rows)

Insert two data records into the table **reason_t2**. One data record conflicts and the other does not. Conflicting data is ignored, and non-conflicting data is inserted.

::

   INSERT INTO reason_t2 VALUES (1, 4, 5),(2, 6, 7) ON CONFLICT(a) DO NOTHING;
   SELECT * FROM reason_t2 ORDER BY 1;
    a | b | c
   ---+---+----
    1 | 2 | 3
    2 | 6 | 7
   (2 rows)

Insert two data records into the table **reason_t2**. One data record conflicts and the other does not. Conflicting data is updated, and non-conflicting data is inserted.

::

   INSERT INTO reason_t2 VALUES (1, 4, 5),(3, 8, 9) ON CONFLICT(a) DO UPDATE SET b = EXCLUDED.b, c = EXCLUDED.c;
   SELECT * FROM reason_t2 ORDER BY 1;
    a | b | c
   ---+---+----
    1 | 4 | 5
    2 | 6 | 7
    3 | 8 | 9
    (3 rows)

Filter the updated rows.

::

   INSERT INTO reason_t2 VALUES (2, 7, 8) ON CONFLICT (a) DO UPDATE SET b = excluded.b, c = excluded.c  WHERE reason_t2.c = 7;
   SELECT * FROM reason_t2 ORDER BY 1;
    a | b | c
   ---+---+---
    1 | 4 | 5
    2 | 7 | 8
    3 | 8 | 9
   (3 rows)

Insert data into the table **reason_t**. Update the conflicting data and adjust the mapping. That is, update column c to column b and column b to column c.

::

   INSERT INTO reason_t2 VALUES (1, 2, 3) ON CONFLICT (a) DO UPDATE SET b = excluded.c, c = excluded.b;
   SELECT * FROM reason_t2 ORDER BY 1;
    a | b | c
   ---+---+---
    1 | 3 | 2
    2 | 7 | 8
    3 | 8 | 9
   (3 rows)
