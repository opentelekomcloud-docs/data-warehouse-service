:original_name: dws_04_1122.html

.. _dws_04_1122:

GaussDB(DWS) Subquery Expressions
=================================

A subquery allows you to nest one query within another, enabling more complex data query and analysis.

Subquery Expressions
--------------------

-  EXISTS/NOT EXISTS

   Before the main query runs, the subquery runs and its result determines if the main query continues. EXISTS returns **true** if the subquery returns at least one row. **NOT EXISTS** returns **true** if the subquery returns no rows.

   |image1|

   Syntax:

   ::

      WHERE column_name EXISTS/NOT EXISTS (subquery)

-  IN/NOT IN

   IN and NOT IN are operators that check if a value is in a set of values. **IN** returns **true** when the outer query row matches a subquery row. **NOT IN** returns **true** when the outer query row does not match any subquery row.

   |image2|

   Syntax:

   ::

      WHERE column_name IN/NOT IN (subquery)

-  ANY/SOME

   **ANY** indicates that any value in a subquery can match a value in an outer query. **SOME** is the same as **ANY**, but the syntax is different.

   The subquery can return only one column. The expression on the left uses operators (``=, <>, <, <=, >, >=``) to compare the value with each subquery row. The result must be a Boolean value. The result of **ANY** is **true** if any true result is obtained. The result is **false** if no true result is found (including the case where the subquery returns no rows).

   |image3|

   Syntax:

   ::

      WHERE column_name operator ANY/SOME (subquery)

-  ALL

   The subquery on the right must return only one field. The expression on the left uses operators (``=, <>, <, <=, >, >=``) to compare the value with each subquery row. The result must be a Boolean value. The result of **ALL** is **true** if all rows yield true (including the case where the subquery returns no rows). The result is **false** if any false result is found.

   |image4|

   Syntax:

   ::

      WHERE column_name operator ALL (subquery)

   .. table:: **Table 1** ALL conditions

      +-------------------------+---------------------------------------------------------------------------------------------------+
      | Condition               | Description                                                                                       |
      +=========================+===================================================================================================+
      | column_name > ALL(...)  | The **column_name** value must be greater than the maximum value of a set to be true.             |
      +-------------------------+---------------------------------------------------------------------------------------------------+
      | column_name >= ALL(...) | The **column_name** value must be greater than or equal to the maximum value of a set to be true. |
      +-------------------------+---------------------------------------------------------------------------------------------------+
      | column_name < ALL(...)  | The **column_name** value must be smaller than the minimum value of a set to be true.             |
      +-------------------------+---------------------------------------------------------------------------------------------------+
      | column_name <= ALL(...) | The **column_name** value must be smaller than or equal to the minimum value of a set to be true. |
      +-------------------------+---------------------------------------------------------------------------------------------------+
      | column_name <> ALL(...) | The **column_name** value cannot be equal to any value in a set to be true.                       |
      +-------------------------+---------------------------------------------------------------------------------------------------+
      | column_name = ALL(...)  | The **column_name** value must be equal to any value in a set to be true.                         |
      +-------------------------+---------------------------------------------------------------------------------------------------+

Example
-------

Create the **course** table and insert data into the table.

::

   CREATE TABLE course(cid VARCHAR(10) COMMENT 'No.course',cname VARCHAR(10) COMMENT 'course name',teid VARCHAR(10) COMMENT 'No.teacher');

   INSERT INTO course VALUES('01' , 'course1' , '02');
   INSERT INTO course VALUES('02' , 'course2' , '01');
   INSERT INTO course VALUES('03' , 'course3' , '03');

Create the **teacher** table and insert data into the table.

::

   CREATE TABLE teacher(teid VARCHAR(10) COMMENT 'Teacher ID',tname VARCHAR(10) COMMENT'Teacher name');

   INSERT INTO teacher VALUES('01' , 'teacher1');
   INSERT INTO teacher VALUES('02' , 'teacher2');
   INSERT INTO teacher VALUES('03' , 'teacher3');
   INSERT INTO teacher VALUES('04' , 'teacher4');

-  EXISTS/NOT EXISTS example

Query the teacher records in the course table.

::

   SELECT * FROM teacher WHERE EXISTS (SELECT * FROM course WHERE course.teid = teacher.teid);

|image5|

Query the teacher records that are not in the **course** table.

::

   SELECT * FROM teacher WHERE NOT EXISTS (SELECT * FROM course WHERE course.teid = teacher.teid);

|image6|

-  IN/NOT IN example

Query the **course** table for teacher information based on the teacher ID.

::

   SELECT * FROM course WHERE teid IN (SELECT teid FROM teacher );

|image7|

Query the information about teachers who are not in the **course** table.

::

   SELECT * FROM teacher WHERE teid NOT IN (SELECT teid FROM course );

|image8|

-  ANY/SOME example

Compare the main query fields on the left with the subquery fields on the right to obtain the required result set.

::

   SELECT * FROM course WHERE teid < ANY (SELECT teid FROM teacher where teid<>'04');

or

::

   SELECT * FROM course WHERE teid < some (SELECT teid FROM teacher where teid<>'04');

|image9|

-  ALL example

The value in the **teid** column must be smaller than the minimum value in the set to be true.

::

   SELECT * FROM course WHERE teid < ALL(SELECT teid FROM teacher WHERE teid<>'01');

|image10|

Important Notes
---------------

-  Duplicate subquery statements are not allowed in an SQL statement.
-  Avoid scalar sub-queries whenever possible. A scalar subquery is a subquery whose result is one value and whose condition expression uses an equal operator.
-  Do not use subqueries in the SELECT target columns. Otherwise, the plan cannot be pushed down, affecting the execution performance.
-  It is recommended that the nested subqueries cannot exceed two layers. Subqueries cause temporary table overhead. Therefore, complex queries must be optimized based on service logic.

A subquery can be nested in the SELECT statement to implement a more complex query. A subquery can also use the results of other queries in the WHERE clause to better filter data. However, subqueries may cause query performance problems and make code difficult to read and understand. Therefore, when using SQL subqueries in databases such as GaussDB, use them based on the site requirements.

.. |image1| image:: /_static/images/en-us_image_0000001787832492.png
.. |image2| image:: /_static/images/en-us_image_0000001834631725.png
.. |image3| image:: /_static/images/en-us_image_0000001787831724.png
.. |image4| image:: /_static/images/en-us_image_0000001834631305.png
.. |image5| image:: /_static/images/en-us_image_0000001814324420.png
.. |image6| image:: /_static/images/en-us_image_0000001861124593.png
.. |image7| image:: /_static/images/en-us_image_0000001814165292.png
.. |image8| image:: /_static/images/en-us_image_0000001861005333.png
.. |image9| image:: /_static/images/en-us_image_0000001814165600.png
.. |image10| image:: /_static/images/en-us_image_0000001814325508.png
