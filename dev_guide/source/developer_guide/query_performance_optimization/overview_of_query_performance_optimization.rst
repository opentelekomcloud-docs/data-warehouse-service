:original_name: dws_04_0402.html

.. _dws_04_0402:

Overview of Query Performance Optimization
==========================================

The aim of SQL optimization is to maximize the utilization of resources, including CPU, memory, disk I/O, and network I/O. To maximize resource utilization is to run SQL statements as efficiently as possible to achieve the highest performance at a lower cost. For example, when performing a typical point query, you can use the seqscan and filter (that is, read every tuple and query conditions for match). You can also use an index scan, which can be implemented at a lower cost but achieve the same effect.

This chapter describes how to analyze and improve query performance, and provides common cases and troubleshooting methods.
