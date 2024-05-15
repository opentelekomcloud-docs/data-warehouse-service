:original_name: dws_04_1000.html

.. _dws_04_1000:

Case: Converting from NOT IN to NOT EXISTS
==========================================

**nestloop anti join** must be used to implement **NOT IN**, while you can use **Hash anti join** to implement **NOT EXISTS**. If no **NULL** value exists in the **JOIN** column, **NOT IN** is equivalent to **NOT EXISTS**. Therefore, if you are sure that no **NULL** value exists, you can convert **NOT IN** to **NOT EXISTS** to generate **hash joins** and to improve the query performance.

Before Optimization
-------------------

Create two base tables **t1** and **t2**.

::

   CREATE TABLE t1(a int, b int, c int not null) WITH(orientation=row);
   CREATE TABLE t2(a int, b int, c int not null) WITH(orientation=row);

Run the following SQL statement to query the **NOT IN** execution plan:

::

   EXPLAIN VERBOSE SELECT * FROM t1 WHERE t1.c NOT IN (SELECT t2.c FROM t2);

The following figure shows the statement output.

|image1|

According to the returned result, nest loops are used. As the OR operation result of NULL and any value is NULL,

::

   t1.c NOT IN (SELECT t2.c FROM t2)

the preceding condition expression is equivalent to:

::

   t1.c <> ANY(t2.c) AND t1.c IS NOT NULL AND ANY(t2.c) IS NOT NULL

After Optimization
------------------

The query can be modified as follows:

::

   SELECT * FROM t1 WHERE NOT EXISTS (SELECT * FROM t2 WHERE t2.c = t1.c);

Run the following statement to query the execution plan of **NOT EXISTS**:

::

   EXPLAIN VERBOSE SELECT * FROM t1 WHERE NOT EXISTS (SELECT 1 FROM t2 WHERE t2.c = t1.c);

|image2|

.. |image1| image:: /_static/images/en-us_image_0000001555962590.png
.. |image2| image:: /_static/images/en-us_image_0000001605933889.png
