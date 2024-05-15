:original_name: dws_04_0402.html

.. _dws_04_0402:

Overview of Query Performance Optimization
==========================================

Performance optimization is a key step in database application development and migration and is a large part in the entire project implementation process. Performance optimization improves database resource utilization, reduces service costs, reduces application system running risks, improves system stability, and brings more values to customers.

The aim of SQL optimization is to maximize the utilization of resources, including CPU, memory, disk I/O, and network I/O. To maximize resource utilization is to run SQL statements as efficiently as possible to achieve the highest performance at a lower cost. For example, when performing a typical point query, you can use the seqscan and filter (that is, read every tuple and query conditions for match). You can also use an index scan, which can be implemented at a lower cost but achieve the same effect.

This chapter describes the basic database commands **analyze** and **explain** for performance optimization, and describes the explained database execution plans. This helps you understand the database execution process, identify performance bottlenecks, and optimize the database through execution plans. In addition, this document describes performance parameters, typical application scenarios, SQL diagnosis, SQL performance optimization, and SQL rewriting cases, which provide you comprehensive reference for database performance optimization.
