:original_name: dws_03_0003.html

.. _dws_03_0003:

Why Do I Need to Use a Data Warehouse?
======================================

Background
----------

Large amounts of data (orders, inventories, materials, and payments) are being generated in enterprises' business operation systems and transactional databases everyday.

Decision makers need to find ways to better utilize and mine such data in order to gain the insights needed to understand the performance of their organizations and make better informed decisions.

Challenges
----------

A data classification and analysis task usually involves simultaneous access to data in multiple database tables, which means multiple tables that are possibly being updated by different transactions need to be locked at the same time. This can be quite difficult for a busy database system.

-  Locking multiple tables increases the latency of a complex query.
-  Blocking transactions that are updating the database tables causes increased latencies or even interruptions for these transactions.

Solution
--------

Data warehouses excel in data aggregation and association, facilitating large-scale data mining for better decision-making support. Data mining requires complex queries that involve multiple tables.

The ETL process copies data from dedicated business databases to a data warehouse for centralized analysis and computing. Data of multiple systems can be aggregated to one data warehouse for the extraction of more valuable data insights.

Data warehouses are designed differently from standard transaction-oriented databases, such as Oracle, Microsoft SQL Server, and MySQL. Data warehouses are optimized in terms of data aggregation and association, but certain features of standard databases, such as transactional properties and data manipulation operations (add, delete, modify) may be compromised or become unavailable. This is why data warehouses and databases are usually used for different purposes. Transactional databases focus on online transaction processing, while data warehouses are better at complex queries and analysis. You could also say databases are for data updates whereas data warehouses are for data analysis.
