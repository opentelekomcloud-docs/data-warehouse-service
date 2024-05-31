:original_name: dws_04_0526.html

.. _dws_04_0526:

Call Statement
==============

Syntax
------

:ref:`Figure 1 <en-us_topic_0000001188163580__f299fb9795429468ea45fe86a41dbca6e>` shows the syntax diagram for calling a clause.

.. _en-us_topic_0000001188163580__f299fb9795429468ea45fe86a41dbca6e:

.. figure:: /_static/images/en-us_image_0000001233883409.png
   :alt: **Figure 1** call_clause::=

   **Figure 1** call_clause::=

The above syntax diagram is explained as follows:

-  **procedure_name** specifies the name of a stored procedure.
-  **parameter** specifies the parameters for the stored procedure. You can set no parameter or multiple parameters.

Examples
--------

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
