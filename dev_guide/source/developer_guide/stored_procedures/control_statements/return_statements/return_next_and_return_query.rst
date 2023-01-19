:original_name: dws_04_0535.html

.. _dws_04_0535:

RETURN NEXT and RETURN QUERY
============================

Syntax
------

When creating a function, specify **SETOF datatype** for the return values.

return_next_clause::=

|image1|

return_query_clause::=

|image2|

The syntax details are as follows:

If a function needs to return a result set, use **RETURN NEXT** or **RETURN QUERY** to add results to the result set, and then continue to execute the next statement of the function. As the **RETURN NEXT** or **RETURN QUERY** statement is executed repeatedly, more and more results will be added to the result set. After the function is executed, all results are returned.

**RETURN NEXT** can be used for scalar and compound data types.

**RETURN QUERY** has a variant **RETURN QUERY EXECUTE**. You can add dynamic queries and add parameters to the queries by using **USING**.

Examples
--------

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

.. |image1| image:: /_static/images/en-us_image_0000001099135202.png
.. |image2| image:: /_static/images/en-us_image_0000001098975214.png
