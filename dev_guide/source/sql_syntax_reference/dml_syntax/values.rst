:original_name: dws_06_0241.html

.. _dws_06_0241:

VALUES
======

Function
--------

**VALUES** computes a row or a set of rows based on given values. It is most commonly used to generate a constant table within a large command.

Precautions
-----------

-  **VALUES** lists with large numbers of rows should be avoided, as you might encounter out-of-memory failures or poor performance. **VALUES** appearing within **INSERT** is a special case, because the desired column types are known from the **INSERT**'s target table, and need not be inferred by scanning the **VALUES** list. In this case, **VALUE** can handle larger lists than are practical in other contexts.
-  If more than one row is specified, all the rows must have the same number of elements.

Syntax
------

::

   VALUES {( expression [, ...] )} [, ...]
       [ ORDER BY { sort_expression [ ASC | DESC | USING operator ] } [, ...] ]
       [ { [ LIMIT { count | ALL } ] [ OFFSET start [ ROW | ROWS ] ] } | { LIMIT start, { count | ALL } } ]
       [ FETCH { FIRST | NEXT } [ count ] { ROW | ROWS } ONLY ];

Parameter Description
---------------------

-  **expression**

   Specifies a constant or expression to compute and insert at the indicated place in the resulting table or set of rows.

   In a **VALUES** list appearing at the top level of an **INSERT**, an expression can be replaced by **DEFAULT** to indicate that the destination column's default value should be inserted. **DEFAULT** cannot be used when **VALUES** appears in other contexts.

-  **sort_expression**

   Specifies an expression or integer constant indicating how to sort the result rows.

-  **ASC**

   Indicates ascending sort order.

-  **DESC**

   Indicates descending sort order.

-  **operator**

   Specifies a sorting operator.

-  **count**

   Specifies the maximum number of rows to return.

-  **start**

   Specifies the number of rows to skip before starting to return rows.

-  **FETCH { FIRST \| NEXT } [ count ] { ROW \| ROWS } ONLY**

   The **FETCH** clause restricts the total number of rows starting from the first row of the return query result, and the default value of **count** is **1**.

Examples
--------

Create the **reason_t1** table.

::

   CREATE TABLE reason_t1
   (
       TABLE_SK          INTEGER               ,
       TABLE_ID          VARCHAR(20)           ,
       TABLE_NA          VARCHAR(20)
   );

Insert a record into a table.

::

   INSERT INTO reason_t1(TABLE_SK, TABLE_ID, TABLE_NA) VALUES (1, 'S01', 'StudentA');

Insert a record into a table. This command is equivalent to the last one.

::

   INSERT INTO reason_t1 VALUES (1, 'S01', 'StudentA');

Insert records whose **TABLE_SK** is less than **1** into the table.

::

   INSERT INTO reason_t1 SELECT * FROM reason_t1 WHERE TABLE_SK < 1;

Insert records into the table.

::

   INSERT INTO reason_t1 VALUES (1, 'S01', 'StudentA'),(2, 'T01', 'TeacherA'),(3, 'T02', 'TeacherB');
   SELECT * FROM reason_t1 ORDER BY 1;
    TABLE_SK | TABLE_ID | TABLE_NAME
   ----------+----------+------------
           1 |      S01 |   StudentA
           2 |      T01 |   TeacherA
           3 |      T02 |   TeacherB
   (3 rows)

Use **INSERT OVERWRITE** to update data in a table, that is, insert data to overwrite the old data.

::

   INSERT OVERWRITE INTO reason_t1 values (4, 'S02', 'StudentB');
   SELECT * FROM reason_t1 ORDER BY 1;
    TABLE_SK | TABLE_ID | TABLE_NAME
   ----------+----------+------------
           4 |      S02 |   StudentB
   (1 rows)

Insert data back into the **reason_t1** table.

.. code-block::

   INSERT INTO reason_t1 SELECT * FROM reason_t1;

Specify default values for independent columns.

.. code-block::

   INSERT INTO reason_t1 VALUES (5, 'S03', DEFAULT);

Insert some data in a table to another table: Use the **WITH** subquery to obtain a temporary table **temp_t**, and then insert all data in **temp_t** to another table **reason_t1**.

.. code-block::

   WITH temp_t AS (SELECT * FROM reason_t1) INSERT INTO reason_t1 SELECT * FROM temp_t ORDER BY 1;

Insert data into a partition of a partitioned table:

::

   CREATE TABLE test_range_row(a int, d int)
   DISTRIBUTE BY hash(a) PARTITION BY RANGE(d)
   (
       PARTITION p1 values LESS THAN (60),
       PARTITION p2 values LESS THAN (75),
       PARTITION p3 values LESS THAN (90),
       PARTITION p4 VALUES LESS THAN (maxvalue)
   );
   INSERT OVERWRITE INTO test_range_row PARTITION(p1) VALUES(55,51);
   INSERT OVERWRITE INTO test_range_row PARTITION(p3) VALUES(85,80);

   DELETE FROM test_range_row PARTITION(p1);
