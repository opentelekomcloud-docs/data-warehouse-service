:original_name: dws_03_0064.html

.. _dws_03_0064:

What Are Partitioned Tables, Partitions, and Partition Keys?
============================================================

Partitioned table: Partitioning refers to splitting what is logically one large table into smaller physical pieces based on specific schemes. The table based on the logic is called a partitioned table, and a physical piece is called a partition. Data is stored on these smaller physical pieces, namely, partitions, instead of the larger logical partitioned table.

Partition: In the GaussDB(DWS) distributed system, data partitioning is to horizontally partition table data within a node based on a specified policy. The table is divided into partitions that do not overlap within a specific range.

Partition key: A partition key is an ordered set of one or more table columns. The values in the table partition keys are used to determine the data partition that a row belongs to.
