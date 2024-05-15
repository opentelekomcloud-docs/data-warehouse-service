:original_name: dws_04_0484.html

.. _dws_04_0484:

Case: Reconstructing Partition Tables
=====================================

Partitioning refers to splitting what is logically one large table into smaller physical pieces based on specific schemes. The table based on the logic is called a partitioned table, and a physical piece is called a partition. Generally, partitioning is applied to tables that have obvious ranges. Partitions on such tables allow scanning on a small part of data, improving the query performance.

During query, partition pruning is used to minimize bottom-layer data scanning to narrow down the overall scope of scanning in a table. Partition pruning means that the optimizer can automatically extract partitions to be scanned based on the partition key specified in the **FROM** and **WHERE** statements. This avoids full table scanning, reduces the number of data blocks to be scanned, and improves performance.

Before Optimization
-------------------

Create a non-partition table **orders_no_part**. The table definition is as follows:

|image1|

Run the following SQL statement to query the execution plan of the non-partition table:

::

   EXPLAIN PERFORMANCE
   SELECT count(*) FROM orders_no_part WHERE
   o_orderdate >= '1996-01-01 00:00:00'::timestamp(0);

As shown in the following figure, the execution time is 73 milliseconds, and the full table scanning time is 44 to 45 milliseconds.

|image2|

After Optimization
------------------

Create a partitioned table **orders**. The table is defined as follows:

|image3|

Run the SQL statement again to query the execution plan of the partitioned table. The execution time is 40 ms, in which the table scanning time is only 13 ms. The smaller the value of **Iterations**, the better the partition pruning effect.

::

   EXPLAIN PERFORMANCE
   SELECT count(*) FROM orders_no_part WHERE
   o_orderdate >= '1996-01-01 00:00:00'::timestamp(0);

As shown in the following figure, the execution time is 40 milliseconds, and the table scanning time is only 13 milliseconds. A smaller **Iterations** value indicates a better partition pruning effect.

|image4|

.. |image1| image:: /_static/images/en-us_image_0000001601785621.png
.. |image2| image:: /_static/images/en-us_image_0000001550427578.png
.. |image3| image:: /_static/images/en-us_image_0000001601788033.png
.. |image4| image:: /_static/images/en-us_image_0000001550748802.png
