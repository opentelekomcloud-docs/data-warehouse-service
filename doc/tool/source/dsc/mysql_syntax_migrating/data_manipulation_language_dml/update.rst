:original_name: dws_16_0193.html

.. _dws_16_0193:

.. _en-us_topic_0000001813438744:

UPDATE
======

In MySQL, **UPDATE** allows the following keywords: **LOW_PRIORITY**, **ORDER BY**, **LIMIT**, and **IGNORE**.

.. _en-us_topic_0000001813438744__en-us_topic_0000001432327713_section66763463919:

LOW_PRIORITY
------------

With the **LOW_PRIORITY** modifier, execution of **UPDATE** is delayed.

**Input**

::

   # LOW_PRIORITY
   UPDATE LOW_PRIORITY employees SET department_id=2;

**Output**

::

   -- LOW_PRIORITY
   UPDATE "public"."employees" SET "department_id" = 2;

.. _en-us_topic_0000001813438744__en-us_topic_0000001432327713_section2770201933915:

ORDER BY
--------

In MySQL, if an **UPDATE** statement includes an **ORDER BY** clause, the rows will be updated in the order specified by the clause.

**Input**

::

   # ORDER BY
   UPDATE employees SET department_id=department_id+1 ORDER BY id;

**Output**

::

   -- ORDER BY
   UPDATE "public"."employees" SET "department_id" = department_id+1;

.. _en-us_topic_0000001813438744__en-us_topic_0000001432327713_section109124408391:

LIMIT
-----

**UPDATE LIMIT** syntax can be used to limit the scope. A clause is a limit on row matching. As long as the rows that satisfy the clause are found, the statements will stop, regardless of whether they have actually changed.

**Input**

::

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

::

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

.. _en-us_topic_0000001813438744__en-us_topic_0000001432327713_section48891264401:

IGNORE
------

With the **IGNORE** modifier, the **UPDATE** statement does not abort even if errors occur during execution.

**Input**

::

   # IGNORE
   UPDATE IGNORE employees SET department_id=3;

**Output**

::

   -- IGNORE
   UPDATE "public"."employees" SET "department_id" = 3;
