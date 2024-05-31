:original_name: dws_04_0476.html

.. _dws_04_0476:

Case: Creating an Appropriate Index
===================================

Creating a proper index can accelerate the retrieval of data rows in a table. Indexes occupy disk space and reduce the speed of adding, deleting, and updating rows. If data needs to be updated very frequently or disk space is limited, you need to limit the number of indexes. Create indexes for large tables. Because the more data in the table, the more effective the index is. You are advised to create indexes on:

-  Columns that need to be queried frequently
-  Joined columns. For a query on joined columns, you are advised to create a composite index on the joined columns. For example, if the join condition is **select \* from t1 join t2 on t1.a=t2.a and t1.b=t2.b**. You can create a composite index on the **a** and **b** columns of table **t1**.
-  Columns having filter criteria (especially scope criteria) of a **where** clause
-  Columns that appear after **order by**, **group by**, and **distinct**

Before optimization
-------------------

The column-store partitioned table **orders** is defined as follows:

|image1|

Run the SQL statement to query the execution plan when no index is created. It is found that the execution time is 48 milliseconds.

::

   EXPLAIN PERFORMANCE SELECT * FROM orders WHERE o_custkey = '1106459';

|image2|

After optimization
------------------

The filtering condition column of the **where** clause is **o_custkey**. Add an index to the **o_custkey** column.

::

   CREATE INDEX idx_o_custkey ON orders (o_custkey) LOCAL;

Run the SQL statement to query the execution plan after the index is created. It is found that the execution time is 18 milliseconds.

|image3|

.. |image1| image:: /_static/images/en-us_image_0000001551160526.png
.. |image2| image:: /_static/images/en-us_image_0000001601476601.png
.. |image3| image:: /_static/images/en-us_image_0000001602017897.png
