:original_name: dws_04_0541.html

.. _dws_04_0541:

GOTO Statements
===============

The **GOTO** statement unconditionally transfers the control from the current statement to a labeled statement. The **GOTO** statement changes the execution logic. Therefore, use this statement only when necessary. Alternatively, you can use the **EXCEPTION** statement to handle issues in special scenarios. To run the **GOTO** statement, the labeled statement must be unique.

Syntax
------

label declaration ::=

|image1|

goto statement ::=

|image2|

Examples
--------

::

   CREATE OR REPLACE PROCEDURE GOTO_test()
   AS
   DECLARE
       v1  int;
   BEGIN
       v1  := 0;
           LOOP
           EXIT WHEN v1 > 100;
                   v1 := v1 + 2;
                   if v1 > 25 THEN
                           GOTO pos1;
                   END IF;
           END LOOP;
   <<pos1>>
   v1 := v1 + 10;
   raise info 'v1 is %. ', v1;
   END;
   /

   call GOTO_test();
   DROP PROCEDURE GOTO_test();

Constraints
-----------

The **GOTO** statement has the following constraints:

-  The **GOTO** statement does not allow multiple labeled statements even if they are in different blocks.

   ::

      BEGIN
        GOTO pos1;
        <<pos1>>
        SELECT * FROM ...
        <<pos1>>
        UPDATE t1 SET ...
      END;

-  The **GOTO** statement cannot transfer control to the **IF**, **CASE**, or **LOOP** statement.

   ::

      BEGIN
         GOTO pos1;
         IF valid THEN
           <<pos1>>
           SELECT * FROM ...
         END IF;
       END;

-  The **GOTO** statement cannot transfer control from one **IF** clause to another, or from one **WHEN** clause in the **CASE** statement to another.

   ::

      BEGIN
         IF valid THEN
           GOTO pos1;
           SELECT * FROM ...
         ELSE
           <<pos1>>
           UPDATE t1 SET ...
         END IF;
       END;

-  The **GOTO** statement cannot transfer control from an outer block to an inner **BEGIN-END** block.

   ::

      BEGIN
         GOTO pos1;
         BEGIN
           <<pos1>>
           UPDATE t1 SET ...
         END;
       END;

-  The **GOTO** statement cannot transfer control from an **EXCEPTION** block to the current **BEGIN-END** block but can transfer to an outer **BEGIN-END** block.

   ::

      BEGIN
         <<pos1>>
         UPDATE t1 SET ...
         EXCEPTION
           WHEN condition THEN
              GOTO pos1;
       END;

-  If the labeled statement in the **GOTO** statement does not exist, you need to add the **NULL** statement.

   ::

      DECLARE
         done  BOOLEAN;
      BEGIN
         FOR i IN 1..50 LOOP
            IF done THEN
               GOTO end_loop;
            END IF;
            <<end_loop>>  -- not allowed unless an executable statement follows
            NULL; -- add NULL statement to avoid error
         END LOOP;  -- raises an error without the previous NULL
      END;
      /

.. |image1| image:: /_static/images/en-us_image_0000001811610621.png
.. |image2| image:: /_static/images/en-us_image_0000001811491541.png
