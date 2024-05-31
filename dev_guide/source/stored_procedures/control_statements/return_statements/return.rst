:original_name: dws_04_0534.html

.. _dws_04_0534:

RETURN
======

Syntax
------

:ref:`Figure 1 <en-us_topic_0000001188482314__fa40a1c0c0b6f4aba8952d533d6f111a5>` shows the syntax diagram for a return statement.

.. _en-us_topic_0000001188482314__fa40a1c0c0b6f4aba8952d533d6f111a5:

.. figure:: /_static/images/en-us_image_0000001188642286.jpg
   :alt: **Figure 1** return_clause::=

   **Figure 1** return_clause::=

The syntax details are as follows:

This statement returns control from a stored procedure or function to a caller.

Example
-------

::

   -- Create the stored procedure proc_staffs:
   CREATE OR REPLACE PROCEDURE proc_staffs
   (
   section     NUMBER(6),
   salary_sum out NUMBER(8,2),
   staffs_count out INTEGER
   )
   IS
   BEGIN
   SELECT sum(salary), count(*) INTO salary_sum, staffs_count FROM staffs where section_id = section;
   END;
   /

   -- Create the stored procedure proc_return:
   CREATE OR REPLACE PROCEDURE proc_return
   AS
   v_num NUMBER(8,2);
   v_sum INTEGER;
   BEGIN
   proc_staffs(30, v_sum, v_num);  --Invoke a statement
   dbms_output.put_line(v_sum||'#'||v_num);
   RETURN;   --Return a statement
   END;
   /

   -- Invoke a stored procedure proc_return:
   CALL proc_return();

   -- Delete a stored procedure:
   DROP PROCEDURE proc_staffs;
   DROP PROCEDURE proc_return;

   --Create the function func_return.
   CREATE OR REPLACE FUNCTION func_return returns void
   language plpgsql
   AS $$
   DECLARE
   v_num INTEGER := 1;
   BEGIN
   dbms_output.put_line(v_num);
   RETURN;   --Return a statement
   END $$;


   -- Invoke the function func_return.
   CALL func_return();
   1

   -- Delete the function:
   DROP FUNCTION func_return;
