:original_name: dws_03_2102.html

.. _dws_03_2102:

How Do I Replace the Null Results with 0 in a GaussDB(DWS) Join Query?
======================================================================

When OUTER JOIN (LEFT JOIN, RIGHT JOIN, and FULL JOIN) is executed, the match failure in the outer join generates a large number of NULL values. You can replace these null values with 0.

You can use the **COALESCE** function to do that. This function returns the first non-null parameter value in the parameter list. For example:

::

   SELECT coalesce(NULL,'hello');
    coalesce
   ----------
    hello
   (1 row)

Use left join to join the tables **course1** and **course2**.

::

   SELECT * FROM course1;
     stu_id  |  stu_name  |     cour_name
   ----------+------------+--------------------
    20110103 | ALLEN      | Math
    20110102 | JACK       | Programming Design
    20110101 | MAX        | Science
   (3 rows)

   SELECT * FROM course2;
    cour_id |     cour_name      | teacher_name
   ---------+--------------------+--------------
       1002 | Programming Design | Mark
       1001 | Science            | Anne
   (2 rows)

   SELECT course1.stu_name,course2.cour_id,course2.cour_name,course2.teacher_name FROM course1 LEFT JOIN course2 ON course1.cour_name = course2.cour_name ORDER BY 1;
     stu_name  | cour_id |     cour_name      | teacher_name
   ------------+---------+--------------------+--------------
    ALLEN      |         |                    |
    JACK       |    1002 | Programming Design | Mark
    MAX        |    1001 | Science            | Anne
   (3 rows)

Use the **COALESCE** function to replace null values in the query result with 0 or other non-zero values:

::

   SELECT course1.stu_name,
    coalesce(course2.cour_id,0) AS cour_id,
    coalesce(course2.cour_name,'NA') AS cour_name,
    coalesce(course2.teacher_name,'NA') AS teacher_name
    FROM course1
    LEFT JOIN course2 ON course1.cour_name = course2.cour_name
    ORDER BY 1;
     stu_name  | cour_id |     cour_name      | teacher_name
   ------------+---------+--------------------+--------------
    ALLEN      |       0 | NA                 | NA
    JACK       |    1002 | Programming Design | Mark
    MAX        |    1001 | Science            | Anne
   (3 rows)
