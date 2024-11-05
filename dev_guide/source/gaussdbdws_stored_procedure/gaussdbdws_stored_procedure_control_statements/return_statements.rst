:original_name: dws_04_0533.html

.. _dws_04_0533:

RETURN Statements
=================

In GaussDB(DWS), data can be returned in either of the following ways: **RETURN**, **RETURN NEXT**, or **RETURN QUERY**. **RETURN NEXT** and **RETURN QUERY** are used only for functions and cannot be used for stored procedures.

RETURN
------

**Syntax**

:ref:`Figure 1 <en-us_topic_0000001510162929__fa40a1c0c0b6f4aba8952d533d6f111a5>` shows the syntax diagram for a return statement.

.. _en-us_topic_0000001510162929__fa40a1c0c0b6f4aba8952d533d6f111a5:

.. figure:: /_static/images/en-us_image_0000002040174922.jpg
   :alt: **Figure 1** return_clause::=

   **Figure 1** return_clause::=

The syntax details are as follows:

This statement returns control from a stored procedure or function to a caller.

**Examples**

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
   proc_staffs(30, v_sum, v_num);  --Invoke a statement.
   dbms_output.put_line(v_sum||'#'||v_num);
   RETURN;   --Return a statement.
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
   RETURN;   --Return a statement.
   END $$;


   -- Invoke the function func_return.
   CALL func_return();
   1

   -- Delete the function.
   DROP FUNCTION func_return;

RETURN NEXT and RETURN QUERY
----------------------------

**Syntax**

When creating a function, specify **SETOF datatype** for the return values.

return_next_clause::=

|image1|

return_query_clause::=

|image2|

The syntax details are as follows:

If a function needs to return a result set, use **RETURN NEXT** or **RETURN QUERY** to add results to the result set, and then continue to execute the next statement of the function. As the **RETURN NEXT** or **RETURN QUERY** statement is executed repeatedly, more and more results will be added to the result set. After the function is executed, all results are returned.

**RETURN NEXT** can be used for scalar and compound data types.

**RETURN QUERY** has a variant **RETURN QUERY EXECUTE**. You can add dynamic queries and add parameters to the queries by using **USING**.

**Examples**

::

   CREATE TABLE t1(a int);
   INSERT INTO t1 VALUES(1),(10);

   --RETURN NEXT
   CREATE OR REPLACE FUNCTION fun_for_return_next() RETURNS SETOF t1 AS $$
   DECLARE
      r t1%ROWTYPE;
   BEGIN
      FOR r IN select * from t1
      LOOP
         RETURN NEXT r;
      END LOOP;
      RETURN;
   END;
   $$ LANGUAGE PLPGSQL;
   call fun_for_return_next();
    a
   ---
    1
    10
   (2 rows)

   -- RETURN QUERY
   CREATE OR REPLACE FUNCTION fun_for_return_query() RETURNS SETOF t1 AS $$
   DECLARE
      r t1%ROWTYPE;
   BEGIN
      RETURN QUERY select * from t1;
   END;
   $$
   language plpgsql;
   call fun_for_return_next();
    a
   ---
    1
    10
   (2 rows)

.. |image1| image:: /_static/images/en-us_image_0000002076212665.png
.. |image2| image:: /_static/images/en-us_image_0000002076334057.png
