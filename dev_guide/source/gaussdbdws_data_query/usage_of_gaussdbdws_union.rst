:original_name: dws_04_1121.html

.. _dws_04_1121:

Usage of GaussDB(DWS) UNION
===========================

**UNION** is a powerful SQL operator that combines the result sets of two or more SELECT statements into one. During combination, the number of columns and data types in the two tables must be the same and correspond to each other. Use the **UNION** or **UNION** ALL keyword between SELECT statements.

UNION removes duplicate rows, while UNION ALL keeps them. Deduplication is time-consuming, so UNION ALL can be faster than UNION if the data sets are already distinct by logic.

|image1|

Syntax
------

::

   SELECT column,... FROM table1 UNION [ALL]SELECT column,... FROM table2

Example
-------

#. Create the student information table **student** (ID, name, gender, and school).

   ::

      SET current_schema=public;
      DROP TABLE IF EXISTS student;
      CREATE table student(
      sId VARCHAR(10) NOT NULL,
      sname VARCHAR(10) NOT NULL,
      sgender VARCHAR(10) NOT NULL,
      sschool VARCHAR(10) NOT NULL);

#. Insert data into the **student** table.

   ::

      INSERT INTO student VALUES('s01' , 'ZhaoLei' , 'male', 'NENU');
      INSERT INTO student VALUES('s02' , 'QianDian' , 'male', 'SJTU');
      INSERT INTO student VALUES('s03' , 'SunFenng' , 'male', 'Tongji');
      INSERT INTO student VALUES('s04' , 'LIYun' , 'male', 'CCOM');
      INSERT INTO student VALUES('s05' , 'ZhouMei' , 'female', 'FuDan');
      INSERT INTO student VALUES('s06' , 'WuLan' , 'female', 'WHU');
      INSERT INTO student VALUES('s07' , 'ZhengZhu' , 'female', 'NWAFU');
      INSERT INTO student VALUES('s08' , 'ZhangShan' , 'female', 'Tongji');

#. View the student table.

   ::

      SELECT * FROM student;

   Information similar to the following is displayed.

   |image2|

#. Create the teacher information table **teacher** (ID, name, gender, and school).

   ::

      DROP TABLE IF EXISTS teacher;
      CREATE table teacher(
      tid VARCHAR(10) NOT NULL,
      tname VARCHAR(10) NOT NULL,
      tgender VARCHAR(10) NOT NULL,
      tschool VARCHAR(10) NOT NULL);

#. Insert data to the **teacher** table.

   ::

      INSERT INTO teacher VALUES('t01' , 'ZhangLei', 'male', 'FuDan');
      INSERT INTO teacher VALUES('t02' , 'LiLiang', 'male', 'WHU');
      INSERT INTO teacher VALUES('t03' , 'WangGang', 'male', 'Tongji');

#. Query the **teacher** table.

   ::

      SELECT * FROM teacher;

   |image3|

#. Use **UNION** (combine and deduplicate) to obtain the schools of students and teachers and sort the schools in ascending order by initial letter of the school name.

   ::

      SELECT t.school  FROM (
           SELECT sschool AS school
            FROM student
            UNION
            SELECT tschool AS school
            FROM teacher
        ) t
        ORDER BY t.school ASC;

   Information similar to the following is displayed.

   |image4|

#. Use **UNION ALL** (combine without deduplication) to obtain the schools of all students and teachers and sort the schools by initial letter of the school name in ascending order.

   ::

      SELECT t.school  FROM (
           SELECT sschool AS school
            FROM student
            UNION ALL
            SELECT tschool AS school
            FROM teacher
        ) t
        ORDER BY t.school ASC;

   |image5|

#. Use **UNION ALL** (combine the result sets of SQL statements with **WHERE** clause) to get all information about students and teachers from "Tongji' and sort by student and teacher number in ascending order.

   ::

      SELECT t.*  FROM  (
        SELECT Sid AS id,Sname AS name,Sgender AS gender,Sschool AS school
        FROM student
        WHERE Sschool='Tongji'
        UNION ALL
        SELECT Tid AS id,Tname AS name,Tgender AS gender,Tschool AS school
        FROM teacher
        WHERE Tschool='Tongji'
      ) t
        ORDER BY t.id ASC;

Summary
-------

In actual service scenarios, pay attention to the following points when using **UNION** and **UNION ALL**:

-  The number of SQL fields and field types on the left and right sides must be the same.
-  Check whether data deduplication (deduplication before combination or during combination) is needed based on service requirements.
-  Based on the data volume, valuate the SQL execution efficiency and determine whether to use temporary tables.
-  Select **UNION** or **UNION ALL** wisely and consider the complexity when writing SQL statements.

.. |image1| image:: /_static/images/en-us_image_0000001775288212.png
.. |image2| image:: /_static/images/en-us_image_0000002050424178.png
.. |image3| image:: /_static/images/en-us_image_0000002086543721.png
.. |image4| image:: /_static/images/en-us_image_0000001820529137.png
.. |image5| image:: /_static/images/en-us_image_0000001773729626.png
