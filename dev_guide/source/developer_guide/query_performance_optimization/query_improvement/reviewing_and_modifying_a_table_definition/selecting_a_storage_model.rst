:original_name: dws_04_0439.html

.. _dws_04_0439:

Selecting a Storage Model
=========================

During database design, some key factors about table design will greatly affect the subsequent query performance of the database. Table design affects data storage as well. Scientific table design reduces I/O operations and minimizes memory usage, improving the query performance.

Selecting a model for table storage is the first step of table definition. Select a proper storage model for your service based on the following table.

+-----------------------------------+---------------------------------------------------------------------------------------------------+
| Storage Model                     | Application Scenario                                                                              |
+===================================+===================================================================================================+
| Row storage                       | Point query (simple index-based query that returns only a few records).                           |
|                                   |                                                                                                   |
|                                   | Query involving many **INSERT**, **UPDATE**, and **DELETE** operations.                           |
+-----------------------------------+---------------------------------------------------------------------------------------------------+
| Column storage                    | Statistics analysis query, in which operations, such as group and join, are performed many times. |
+-----------------------------------+---------------------------------------------------------------------------------------------------+
