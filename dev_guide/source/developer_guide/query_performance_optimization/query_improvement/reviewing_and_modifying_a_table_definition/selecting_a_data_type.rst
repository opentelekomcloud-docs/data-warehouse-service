:original_name: dws_04_0444.html

.. _dws_04_0444:

Selecting a Data Type
=====================

Use the following principles to obtain efficient data types:

#. **Using the data type that can be efficiently executed**

   Generally, calculation of integers (including common comparison calculations, such as =, >, <, >=, <=, and ≠ and group by) is more efficient than that of strings and floating point numbers. For example, if you need to filter data in a column containing numeric data for a column-store table where point query is performed, the execution takes over 10s. However, the execution time is reduced to 1.8s when you change the data type from NUMERIC to INT.

#. **Using the data type of short length column**

   Using the data type with a shorter length reduces both the data file size and the memory used for computing, improving the I/O and computing performance. For example, use SMALLINT instead of INT, and INT instead of BIGINT.

#. **Using the same data type for associated columns**

   Use the same data type for associated columns. If columns having different data types are associated, the database must dynamically convert the different data types into the same ones for comparison. The conversion results in performance overheads.
