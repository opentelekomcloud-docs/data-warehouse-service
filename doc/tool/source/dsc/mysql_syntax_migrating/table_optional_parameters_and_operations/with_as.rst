:original_name: dws_16_0153.html

.. _dws_16_0153:

WITH AS
=======

WITH AS is used in GaussDB(DWS) to declare one or more subqueries that can be referenced by name in the main query. It is equivalent to a temporary table. DSC supports this keyword, which is retained in the migration.

**Input**

::

   WITH e AS (
     SELECT city, sum(population) FROM t1 group by city
     ),
     d AS (
     SELECT max(music) as max_music, min(music) as min_music from student
     ),
     s AS (
     SELECT * FROM subject
     )
   SELECT * FROM e;

**Output**

::

   WITH e AS (
     SELECT
       city, sum(population)
     FROM
       t1
     GROUP BY
       city
   ),
   d AS (
     SELECT
       max(music) AS "max_music", min(music) AS "min_music"
     FROM
       student
   ),
   s AS (
     SELECT
       *
     FROM
       subject
   )
   SELECT
     *
   FROM
     e;
