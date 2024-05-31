:original_name: dws_04_0444.html

.. _dws_04_0444:

Selecting a Data type
=====================

You can use data types with the following features to improve efficiency:

#. **Data types that boost execution efficiency**

   Generally, the calculation of integers (including common comparison calculations, such as =, >, <, >=, <=, and ≠ and **GROUP BY**) is more efficient than that of strings and floating point numbers. For example, if you need to perform a point query on a column-store table whose **NUMERIC** column is used as a filter criterion, the query will take over 10 seconds. If you change the data type from **NUMERIC** to **INT**, the query takes only about 1.8 seconds.

#. **Data types with a short length**

   Data types with short length reduce both the data file size and the memory used for computing, improving the I/O and computing performance. For example, use **SMALLINT** instead of **INT**, and **INT** instead of **BIGINT**.

#. **Same data type for a join**

   You are advised to use the same data type for a join. To join columns with different data types, the database needs to convert them to the same type, which leads to additional performance overheads.
