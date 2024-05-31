:original_name: dws_16_0196.html

.. _dws_16_0196:

.. _en-us_topic_0000001819416285:

LIMIT
=====

UPDATE LIMIT syntax can be used to limit the scope. A clause is a limit on row matching. As long as the rows that satisfy the clause are found, the statements will stop, regardless of whether they have actually changed.

**Input**

.. code-block::

   # LIMIT
   UPDATE  employees SET department_id=department_id+1   LIMIT 3 ;
   UPDATE  employees SET department_id=department_id+1   LIMIT 3 , 10 ;

   # LIMIT + OFFSET
   UPDATE  employees SET department_id=department_id+1   LIMIT 3   OFFSET 2;

   # LIMIT + ORDER BY
   UPDATE  employees SET department_id=department_id+1 ORDER BY fname  LIMIT 3 ;

   # LIMIT + WHERE + ORDER BY
   UPDATE  employees SET department_id=department_id+1 WHERE id<5 ORDER BY  fname  LIMIT 3 ;

   # LIMIT + WHERE + ORDER BY + OFFSET
   UPDATE  employees SET department_id=department_id+1 WHERE id<5 ORDER BY  fname  LIMIT 3 OFFSET 2 ;

**Output**

.. code-block::

   -- LIMIT
   UPDATE "public"."employees" SET "department_id" = department_id+1;
   UPDATE "public"."employees" SET "department_id" = department_id+1;

   -- LIMIT + OFFSET
   UPDATE "public"."employees" SET "department_id" = department_id+1;

   -- LIMIT + ORDER BY
   UPDATE "public"."employees" SET "department_id" = department_id+1;

   -- LIMIT + WHERE + ORDER BY
   UPDATE "public"."employees" SET "department_id" = department_id+1 WHERE id<5;

   -- LIMIT + WHERE + ORDER BY + OFFSET
   UPDATE "public"."employees" SET "department_id" = department_id+1 WHERE id<5;
