:original_name: dws_04_1003.html

.. _dws_04_1003:

GaussDB(DWS) Multi-Table Join Query
===================================

Join Types
----------

Multiple joins are necessary for accomplishing complex queries. Joins are classified into inner joins and outer joins. Each type of joins have their subtypes.

-  Inner join: inner join, cross join, and natural join.
-  Outer join: left outer join, right outer join, and full join.

To better illustrate the differences between these joins, the following provides some examples.

Create the sample tables **student** and **math_score** and insert data into them. Set **enable_fast_query_shipping** to **off** (**on** by default), that is, the query optimizer uses the distributed framework. Set **explain_perf_mode** to **pretty** (default value) to specify the **EXPLAIN** display format.

::

   CREATE TABLE student(
     id INTEGER,
     name varchar(50)
   );

   CREATE TABLE math_score(
     id INTEGER,
     score INTEGER
   );

   INSERT INTO student VALUES(1, 'Tom');
   INSERT INTO student VALUES(2, 'Lily');
   INSERT INTO student VALUES(3, 'Tina');
   INSERT INTO student VALUES(4, 'Perry');

   INSERT INTO math_score VALUES(1, 80);
   INSERT INTO math_score VALUES(2, 75);
   INSERT INTO math_score VALUES(4, 95);
   INSERT INTO math_score VALUES(6, NULL);

   SET enable_fast_query_shipping = off;
   SET explain_perf_mode = pretty;

Inner Join
----------

-  Inner join

   Syntax:

   ::

      left_table [INNER] JOIN right_table [ ON join_condition | USING ( join_column )]

   Description: Rows that meet **join_condition** in both the left and right tables are joined and output. Tuples that do not meet **join_condition** are not output.

   Example 1: Query students' math scores.

   ::

      SELECT s.id, s.name, ms.score FROM student s JOIN math_score ms on s.id = ms.id;
       id | name  | score
      ----+-------+-------
        2 | Lily  |    75
        1 | Tom   |    80
        4 | Perry |    95
      (3 rows)


      EXPLAIN SELECT s.id, s.name, ms.score FROM student s JOIN math_score ms on s.id = ms.id;
                                             QUERY PLAN
      ----------------------------------------------------------------------------------------
        id |                operation                | E-rows | E-memory | E-width | E-costs
       ----+-----------------------------------------+--------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)            |      4 |          |      13 | 19.47
         2 |    ->  Hash Join (3,4)                  |      4 | 1MB      |      13 | 11.47
         3 |       ->  Seq Scan on math_score ms     |     30 | 1MB      |       8 | 10.10
         4 |       ->  Hash                          |     12 | 16MB     |       9 | 1.28
         5 |          ->  Streaming(type: BROADCAST) |     12 | 2MB      |       9 | 1.28
         6 |             ->  Seq Scan on student s   |      4 | 1MB      |       9 | 1.01

       Predicate Information (identified by plan id)
       ---------------------------------------------
         2 --Hash Join (3,4)
               Hash Cond: (ms.id = s.id)

         ====== Query Summary =====
       -------------------------------
       System available mem: 1761280KB
       Query Max mem: 1761280KB
       Query estimated mem: 4400KB
      (19 rows)

-  Cross join

   Syntax:

   ::

      left_table CROSS JOIN right_table

   Description: Each row in the left table is joined with each row in the right table. The number of final rows is the product of the number of rows on both sides. The product is also called Cartesian product.

   Example 2: Cross join of student tables and math score tables.

   ::

      SELECT s.id, s.name, ms.score FROM student s CROSS JOIN math_score ms;
       id | name  | score
      ----+-------+-------
        3 | Tina  |    80
        2 | Lily  |    80
        1 | Tom   |    80
        4 | Perry |    80
        3 | Tina  |
        2 | Lily  |
        1 | Tom   |
        4 | Perry |
        3 | Tina  |    95
        2 | Lily  |    95
        1 | Tom   |    95
        4 | Perry |    95
        2 | Lily  |    75
        3 | Tina  |    75
        1 | Tom   |    75
        4 | Perry |    75
      (16 rows)

      EXPLAIN SELECT s.id, s.name, ms.score FROM student s CROSS JOIN math_score ms;
                                             QUERY PLAN
      ----------------------------------------------------------------------------------------
        id |                operation                | E-rows | E-memory | E-width | E-costs
       ----+-----------------------------------------+--------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)            |    120 |          |      13 | 19.89
         2 |    ->  Nested Loop (3,4)                |    120 | 1MB      |      13 | 11.89
         3 |       ->  Seq Scan on math_score ms     |     30 | 1MB      |       4 | 10.10
         4 |       ->  Materialize                   |     12 | 16MB     |       9 | 1.30
         5 |          ->  Streaming(type: BROADCAST) |     12 | 2MB      |       9 | 1.28
         6 |             ->  Seq Scan on student s   |      4 | 1MB      |       9 | 1.01

         ====== Query Summary =====
       -------------------------------
       System available mem: 1761280KB
       Query Max mem: 1761280KB
       Query estimated mem: 4144KB
      (14 rows)

-  Natural join

   Syntax:

   ::

      left_table NATURAL JOIN right_table

   Description: Columns with the same name in left table and right table are joined by equi-join, and the columns with the same name are merged into one column.

   Example 3: Natural join between the **student** table and the **math_score** table. The columns with the same name in the two tables are the **id** columns, therefore equivalent join is performed based on the **id** columns.

   ::

      SELECT * FROM student s NATURAL JOIN math_score ms;
       id | name  | score
      ----+-------+-------
        1 | Tom   |    80
        4 | Perry |    95
        2 | Lily  |    75
      (3 rows)

      EXPLAIN SELECT * FROM student s NATURAL JOIN math_score ms;
                                             QUERY PLAN
      ----------------------------------------------------------------------------------------
        id |                operation                | E-rows | E-memory | E-width | E-costs
       ----+-----------------------------------------+--------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)            |      4 |          |      13 | 19.47
         2 |    ->  Hash Join (3,4)                  |      4 | 1MB      |      13 | 11.47
         3 |       ->  Seq Scan on math_score ms     |     30 | 1MB      |       8 | 10.10
         4 |       ->  Hash                          |     12 | 16MB     |       9 | 1.28
         5 |          ->  Streaming(type: BROADCAST) |     12 | 2MB      |       9 | 1.28
         6 |             ->  Seq Scan on student s   |      4 | 1MB      |       9 | 1.01

       Predicate Information (identified by plan id)
       ---------------------------------------------
         2 --Hash Join (3,4)
               Hash Cond: (ms.id = s.id)

         ====== Query Summary =====
       -------------------------------
       System available mem: 1761280KB
       Query Max mem: 1761280KB
       Query estimated mem: 4400KB
      (19 rows)

Outer Join
----------

-  Left Join

   Syntax:

   ::

      left_table LEFT [OUTER] JOIN right_table [ ON join_condition | USING ( join_column )]

   Description: The result set of a left outer join includes all rows of left table, not only the joined rows. If a row in the left table does not match any row in right table, the row will be **NULL** in the result set.

   Example 4: Perform left join on the **student** table and **math_score** table. The right table data corresponding to the row where ID is 3 in the **student** table is filled with **NULL** in the result set.

   ::

      SELECT s.id, s.name, ms.score FROM student s LEFT JOIN math_score ms on (s.id = ms.id);
       id | name  | score
      ----+-------+-------
        3 | Tina  |
        1 | Tom   |    80
        2 | Lily  |    75
        4 | Perry |    95
      (4 rows)

      EXPLAIN SELECT s.id, s.name, ms.score FROM student s LEFT JOIN math_score ms on (s.id = ms.id);
                                              QUERY PLAN
      -------------------------------------------------------------------------------------------
        id |                 operation                  | E-rows | E-memory | E-width | E-costs
       ----+--------------------------------------------+--------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)               |      4 |          |      13 | 10.26
         2 |    ->  Hash Left Join (3, 5)               |      4 | 1MB      |      13 | 2.26
         3 |       ->  Streaming(type: REDISTRIBUTE)    |      4 | 2MB      |       9 | 1.11
         4 |          ->  Seq Scan on student s         |      4 | 1MB      |       9 | 1.01
         5 |       ->  Hash                             |      4 | 16MB     |       8 | 1.11
         6 |          ->  Streaming(type: REDISTRIBUTE) |      4 | 2MB      |       8 | 1.11
         7 |             ->  Seq Scan on math_score ms  |      4 | 1MB      |       8 | 1.01

       Predicate Information (identified by plan id)
       ---------------------------------------------
         2 --Hash Left Join (3, 5)
               Hash Cond: (s.id = ms.id)

         ====== Query Summary =====
       ------------------------------
       System available mem: 901120KB
       Query Max mem: 901120KB
       Query estimated mem: 7520KB
      (20 rows)

-  Right join

   Syntax:

   ::

      left_table RIGHT [OUTER] JOIN right_table [ ON join_condition | USING ( join_column )]

   Description: Contrary to the left join, the result set of a right join includes all rows of the right table, not just the joined rows. If a row in the right table does not match any row in right table, the row will be **NULL** in the result set.

   Example 5: Perform right join on the **student** table and **math_score** table. The right table data corresponding to the row where ID is 6 in the **math_score** table is filled with **NULL** in the result set.

   ::

      SELECT ms.id, s.name, ms.score FROM student s RIGHT JOIN math_score ms on (s.id = ms.id);
       id | name  | score
      ----+-------+-------
        1 | Tom   |    80
        6 |       |
        4 | Perry |    95
        2 | Lily  |    75

      EXPLAIN SELECT ms.id, s.name, ms.score FROM student s RIGHT JOIN math_score ms on (s.id = ms.id);
                                             QUERY PLAN
      ----------------------------------------------------------------------------------------
        id |                operation                | E-rows | E-memory | E-width | E-costs
       ----+-----------------------------------------+--------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)            |     30 |          |      13 | 19.47
         2 |    ->  Hash Left Join (3, 4)            |     30 | 1MB      |      13 | 11.47
         3 |       ->  Seq Scan on math_score ms     |     30 | 1MB      |       8 | 10.10
         4 |       ->  Hash                          |     12 | 16MB     |       9 | 1.28
         5 |          ->  Streaming(type: BROADCAST) |     12 | 2MB      |       9 | 1.28
         6 |             ->  Seq Scan on student s   |      4 | 1MB      |       9 | 1.01

       Predicate Information (identified by plan id)
       ---------------------------------------------
         2 --Hash Left Join (3, 4)
               Hash Cond: (ms.id = s.id)

         ====== Query Summary =====
       -------------------------------
       System available mem: 1761280KB
       Query Max mem: 1761280KB
       Query estimated mem: 5424KB
      (19 rows)

   In a right join, **Left** is displayed in the join operator. This is because a right join is actually the process replacing the left table with the right table then performing left join.

-  Full join

   Syntax:

   ::

      left_table FULL [OUTER] JOIN right_table [ ON join_condition | USING ( join_column )]

   Description: A full join is a combination of a left outer join and a right outer join. The result set of a full outer join includes all rows of the left table and the right table, not just the joined rows. If a row in the left table does not match any row in the right table, the row will be **NULL** in the result set. If a row in the right table does not match any row in right table, the row will be **NULL** in the result set.

   Example 6: Perform full outer join on the **student** table and **math_score** table. The right table data corresponding to the row where ID is 3 is filled with **NULL** in the result set. The left table data corresponding to the row where ID is 6 is filled with **NULL** in the result set.

   ::

      SELECT s.id, s.name, ms.id, ms.score FROM student s FULL JOIN math_score ms ON (s.id = ms.id);
       id | name  | id | score
      ----+-------+----+-------
        2 | Lily  |  2 |    75
        4 | Perry |  4 |    95
        1 | Tom   |  1 |    80
        3 | Tina  |    |
          |       |  6 |
      (5 rows)

      EXPLAIN SELECT s.id, s.name, ms.id, ms.score FROM student s FULL JOIN math_score ms ON (s.id = ms.id);
                                              QUERY PLAN
      -------------------------------------------------------------------------------------------
        id |                 operation                  | E-rows | E-memory | E-width | E-costs
       ----+--------------------------------------------+--------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)               |     30 |          |      17 | 20.24
         2 |    ->  Hash Full Join (3, 5)               |     30 | 1MB      |      17 | 12.24
         3 |       ->  Streaming(type: REDISTRIBUTE)    |     30 | 2MB      |       8 | 11.06
         4 |          ->  Seq Scan on math_score ms     |     30 | 1MB      |       8 | 10.10
         5 |       ->  Hash                             |      4 | 16MB     |       9 | 1.11
         6 |          ->  Streaming(type: REDISTRIBUTE) |      4 | 2MB      |       9 | 1.11
         7 |             ->  Seq Scan on student s      |      4 | 1MB      |       9 | 1.01

       Predicate Information (identified by plan id)
       ---------------------------------------------
         2 --Hash Full Join (3, 5)
               Hash Cond: (ms.id = s.id)

         ====== Query Summary =====
       -------------------------------
       System available mem: 1761280KB
       Query Max mem: 1761280KB
       Query estimated mem: 6496KB
      (20 rows)

Differences Between the ON Condition and the WHERE Condition in Multi-Table Query
---------------------------------------------------------------------------------

According to the preceding join syntax, except natural join and cross join, the **ON** condition (**USING** is converted to the **ON** condition during query parsing) is used on the join result of both the two tables. Generally, the **WHERE** condition is used in the query statement to restrict the query result. The **ON** join condition and **WHERE** filter condition do not contain conditions that can be pushed down to tables. The differences between **ON** and **WHERE** are as follows:

-  The **ON** condition is used for joining two tables.
-  **WHERE** is used to filter the result set.

To sum up, the **ON** condition is used when two tables are joined. After the join result set of two tables is generated, the **WHERE** condition is used.
