:original_name: dws_06_0237.html

.. _dws_06_0237:

UPSERT
======

Function
--------

**UPSERT** inserts rows into a table. When a primary key or unique constraint conflict occurs, it can choose to update the conflicting data or ignore the conflict.

.. important::

   The **UPSERT** syntax is supported only in 8.1.1 and later.

Precautions
-----------

.. warning::

   -  Do not concurrently update the same column-store table using the **UPDATE** and **UPSERT** statements.
   -  For more information about development and design specifications, see "DWS Development and Design Proposal" in the *Developer Guide*.

-  Permission requirements: Only users with the **INSERT** or **UPDATE** permission on a table can run the **UPSERT** statement to insert data to or update data in the table.

-  Index requirements: The **UPSERT** statement of updating data upon conflict can be executed only when the target table contains a primary key or unique index. The **UPSERT** statement of updating data upon conflict cannot be executed if no unique indexes are available. You can execute the statement only after the indexes are rebuilt.

-  Other requirements:

   -  The **WITH** clause cannot be used at the same time.

   -  **INSERT OVERWRITE** cannot be used at the same time.

   -  The **UPDATE** clause and its **WHERE** clause do not support subqueries.

   -  **VALUES(colname)** in the **UPDATE** clause does not support outer nested functions. That is, the usage similar to **sqrt(VALUES(colname))** is not supported. To support this function, use the **EXCLUDED.colname** syntax.

   -  **INSERT INTO ON CONFLICT(...) DO UPDATE** must contain **conflict_target**. That is, a column or constraint name must be specified.

   -  The distribution column cannot be updated. (Exception: Update is allowed if the distribution key is the same as the updated value.)

      ::

         CREATE TABLE t1(dist_key int PRIMARY KEY, a int, b int);
         INSERT INTO t1 VALUES(1,2,3) ON CONFLICT(dist_key) DO UPDATE SET dist_key = EXCLUDED.dist_key, a = EXCLUDED.a + 1;
         INSERT INTO t1 VALUES(1,2,3) ON CONFLICT(dist_key) DO UPDATE SET dist_key = dist_key, a = EXCLUDED.a + 1;

   -  The **UPSERT** statement cannot be executed on the target table that contains a trigger (with the **INSERT** or **UPDATE** trigger event).

   -  The **UPSERT** statement is not supported for updatable views.

   -  The **UPDATE** clause, the **WHERE** clause of **UPDATE**, and the index condition expression should not contain :ref:`functions that cannot be pushed down <en-us_topic_0000001811634697__section1365845133718>`.

   -  Unique indexes cannot be deferred.

   -  When performing the update operation of **UPSERT** using **INSERT INTO SELECT**, pay attention to the query result sequence of **SELECT**. In a distributed environment, if the **ORDER BY** statement is not used, the sequence of returned results may be different each time the same **SELECT** statement is executed. As a result, the execution result of the **UPSERT** statement does not meet the expectation.

   -  Multiple updates are not supported. When conflicting data groups exist, you will see an error like "INSERT ON CONFLICT DO UPDATE command cannot affect row a second time." This does not happen if the query uses a PGXC plan. For details, see :ref:`FAQs <en-us_topic_0000001811515753__section20849161583318>`.

-  Impact on performance:

   -  When UPSERT is executed on column-store tables, enable DELTA tables to avoid small CUs from being generated. A large number of small CUs may cause tables to bloat or lead to poor query performance.
   -  **UPSERT**, **UPDATE**, and **DELETE** operations cannot be concurrently performed because they need to wait for the CU lock. This problem cannot be solved even if the DELTA table is enabled. To execute **UPSERT**, **UPDATE**, and **DELETE** operations concurrently on column-store tables, use HStore tables.

-  Deadlock: A distributed deadlock may occur, resulting in query hanging. When many **UPSERT** commands run together in a transaction or batch via JDBC with **setAutoCommit(false)**, deadlocks can happen if several tasks try to update the same row simultaneously. This occurs because different threads on various nodes might update rows in varying orders. To solve this issue, you can:

   #. Decrease the value of the GUC parameter **lockwait_timeout** (default value: 20 minutes). A distributed deadlock error will be reported after waiting for *the value of* **lockwait_timeout**. You can decrease the value of this parameter to reduce the service waiting time caused by a deadlock.
   #. Only import data with the same primary key through one database connection. You can run **UPSERT** statements at the same time.
   #. Ensure that only one UPSERT statement is executed in each transaction. In this way, **UPSERT** statements can be executed concurrently.
   #. Run all **UPSERT** operations one at a time in a single thread; avoid running them simultaneously.

   Method 1 reduces waiting time but does not fix deadlocks. When using the **UPSERT** statement in services, lower this parameter's value. Methods 2, 3, and 4 resolve deadlocks, with method 2 being preferred due to its superior performance.

Syntax
------

For details, see :ref:`Syntax <en-us_topic_0000001764516334__se26969fe97994814b5f45a6173164204>` of **INSERT**. The following table describes the syntax of **UPSERT**.

The first syntax does not specify an index. The system checks for conflicts on all primary keys or unique indexes. If a conflict exists, the system ignores or updates the corresponding data.

The second syntax is to specify an index. The primary key or unique index is inferred from the column name, the expression that contains column names, or the constraint name specified in the **ON CONFLICT** clause.

-  Scenario 1: If inserting data causes a conflict, the system updates the current data with the new information.

   -  Conflict update without specifying an index:

      ::

         INSERT INTO ON DUPLICATE KEY UPDATE

   -  Conflict update with an index specified:

      .. code-block::

         INSERT INTO ON CONFLICT(...) DO UPDATE SET
         INSERT INTO ON CONFLICT ON CONSTRAINT con_name DO UPDATE SET

-  Scenario 2: If a data insertion causes a conflict, the system skips the operation.

   -  Conflict ignorance without specifying an index:

      .. code-block::

         INSERT IGNORE
         INSERT INTO ON CONFLICT DO NOTHING

   -  Conflict ignorance with an index specified:

      .. code-block::

         INSERT INTO ON CONFLICT(...) DO NOTHING
         INSERT INTO ON CONFLICT ON CONSTRAINT con_name DO NOTHING

.. table:: **Table 1** UPSERT parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | Parameter             | Description                                                                                                                                                                                                                                                                                                                             | Value Range or Example |
   +=======================+=========================================================================================================================================================================================================================================================================================================================================+========================+
   | ON CONFLICT (...)     | The system determines the primary key or unique index by analyzing the given column or constraint names for a specific index's syntax. You can specify a single column name or multiple column names by using an expression, for example, **(column1, column2, column3)**.                                                              | ``-``                  |
   |                       |                                                                                                                                                                                                                                                                                                                                         |                        |
   |                       | **collation** and **opclass** can be specified when you create an index. Therefore, you can also specify them after the column name for index inference.                                                                                                                                                                                |                        |
   |                       |                                                                                                                                                                                                                                                                                                                                         |                        |
   |                       | **COLLATE collation** specifies the collation of a column, and **opclass** specifies the name of the operator class. For details, see :ref:`CREATE INDEX <dws_06_0165>`.                                                                                                                                                                |                        |
   |                       |                                                                                                                                                                                                                                                                                                                                         |                        |
   |                       | When inferring the unique index from an expression that includes multiple column names, the system checks whether there is a unique index that exactly contains all the column names specified by **conflict_target**.                                                                                                                  |                        |
   |                       |                                                                                                                                                                                                                                                                                                                                         |                        |
   |                       | -  If **collation** and **opclass** are not specified, a match is considered found as long as a column has the same name as the specified single column or multiple columns have the same names as those specified by the column expression (regardless of the values of **collation** and **opclass** specified for the index column). |                        |
   |                       | -  If **collation** and **opclass** are specified, their values must also match the **collation** and **opclass** of the index.                                                                                                                                                                                                         |                        |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | **UPDATE** clause     | The **UPDATE** clause can use **VALUES(colname)** or **EXCLUDED.colname** to reference inserted data. **EXCLUDED** indicates the data row that should be excluded due to conflict. For details about the syntax example, see :ref:`Examples <en-us_topic_0000001811515753__section414485520537>`.                                       | ``-``                  |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
   | **WHERE** clause      | -  The **WHERE** clause is used to determine whether a specified condition is met when data conflict occurs. If yes, update the conflicting data. Otherwise, ignore the conflict.                                                                                                                                                       | ``-``                  |
   |                       | -  Only syntax 2 of **Update Data Upon Conflict** can specify the **WHERE** clause, that is, **INSERT INTO ON CONFLICT(...) DO UPDATE SET WHERE**.                                                                                                                                                                                      |                        |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+

.. _en-us_topic_0000001811515753__section414485520537:

Examples
--------

Create table **reason_t2** and insert data into it.

::

   DROP TABLE IF EXISTS reason_t2;
   CREATE TABLE reason_t2
   (
     a    int primary key,
     b    int,
     c    int
   );

   INSERT INTO reason_t2 VALUES (1, 2, 3);
   SELECT * FROM reason_t2 ORDER BY 1;

|image1|

Insert two data records into the table **reason_t2**. One data record conflicts and the other does not. Conflicting data is ignored, and non-conflicting data is inserted.

::

   INSERT INTO reason_t2 VALUES (1, 4, 5),(2, 6, 7) ON CONFLICT(a) DO NOTHING;
   SELECT * FROM reason_t2 ORDER BY 1;

|image2|

Insert two data records into the table **reason_t2**. One data record conflicts and the other does not. Update conflicting information and add new data where there is no conflict. **EXCLUDED.b** and **EXCLUDED.c** refer to columns **b** and **c** for the inserted values, which are 4 and 5 in this case.

::

   INSERT INTO reason_t2 VALUES (1, 4, 5),(3, 8, 9) ON CONFLICT(a) DO UPDATE SET b = EXCLUDED.b, c = EXCLUDED.c;
   SELECT * FROM reason_t2 ORDER BY 1;

|image3|

Filter the updated rows.

::

   INSERT INTO reason_t2 VALUES (2, 7, 8) ON CONFLICT (a) DO UPDATE SET b = excluded.b, c = excluded.c  WHERE reason_t2.c = 7;
   SELECT * FROM reason_t2 ORDER BY 1;

|image4|

Insert data into the table **reason_t**. Update the conflicting data and adjust the mapping. That is, update column c to column b and column b to column c.

::

   INSERT INTO reason_t2 VALUES (1, 2, 3) ON CONFLICT (a) DO UPDATE SET b = excluded.c, c = excluded.b;
   SELECT * FROM reason_t2 ORDER BY 1;

|image5|

.. _en-us_topic_0000001811515753__section20849161583318:

FAQs
----

Multiple updates are not supported. When conflicting data groups exist, you will see an error like "INSERT ON CONFLICT DO UPDATE command cannot affect row a second time." This does not happen if the query uses a PGXC plan. Below is an example.

::

   CREATE TABLE t1(id int PRIMARY KEY, a int, b int);
   SET explain_perf_mode=pretty;
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
   ERROR:  dn_xxxx: INSERT ON CONFLICT DO UPDATE command cannot affect row a second time
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

.. |image1| image:: /_static/images/en-us_image_0000002624830876.png
.. |image2| image:: /_static/images/en-us_image_0000002655190287.png
.. |image3| image:: /_static/images/en-us_image_0000002655270363.png
.. |image4| image:: /_static/images/en-us_image_0000002624831032.png
.. |image5| image:: /_static/images/en-us_image_0000002655190371.png
