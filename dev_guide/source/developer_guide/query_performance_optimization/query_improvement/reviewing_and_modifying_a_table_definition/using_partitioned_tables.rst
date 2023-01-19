:original_name: dws_04_0443.html

.. _dws_04_0443:

Using Partitioned Tables
========================

Partitioning refers to splitting what is logically one large table into smaller physical pieces based on specific schemes. The table based on the logic is called a partitioned table, and a physical piece is called a partition. Data is stored on these smaller physical pieces, namely, partitions, instead of the larger logical partitioned table. A partitioned table has the following advantages over an ordinary table:

#. High query performance: The system queries only the concerned partitions rather than the whole table, improving the query efficiency.
#. High availability: If a partition is faulty, data in the other partitions is still available.
#. Easy maintenance: You only need to fix the faulty partition.

GaussDB(DWS) supports range-partitioned tables.

Range-partitioned table: Data within a specific range is mapped onto each partition. The range is determined by the partition key specified during the partitioned table creation. The partition key is usually a date. For example, sales data is partitioned by month.
