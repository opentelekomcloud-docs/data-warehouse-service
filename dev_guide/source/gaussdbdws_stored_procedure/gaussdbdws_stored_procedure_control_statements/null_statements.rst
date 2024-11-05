:original_name: dws_04_0539.html

.. _dws_04_0539:

NULL Statements
===============

In PL/SQL programs, **NULL** statements are used to indicate "nothing should be done", equal to placeholders. They grant meanings to some statements and improve program readability.

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
