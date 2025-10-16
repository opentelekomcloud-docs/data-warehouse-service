:original_name: dws_04_0539.html

.. _dws_04_0539:

NULL Statements
===============

In PL/SQL programs, a **NULL** statement can be used to indicate "do nothing", which is also known as an empty statement.

A NULL statement acts as a placeholder and can give meaning to certain statements, improving the readability of the program.

Syntax
------

The following shows example use of NULL statements.

::

   DECLARE
       ...
   BEGIN
       ...
       IF v_num IS NULL THEN
           NULL; --No data needs to be processed.
       END IF;
   END;
   /
