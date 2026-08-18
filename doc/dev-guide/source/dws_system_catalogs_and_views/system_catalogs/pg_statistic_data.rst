:original_name: dws_04_1083.html

.. _dws_04_1083:

PG_STATISTIC_DATA
=================

Records extended statistics of tables in the current cluster, including expression statistics. This system catalog is supported only by clusters of version 8.2.1.210 or later.

.. table:: **Table 1** PG_STATISTIC_DATA columns

   +-----------------------+-----------------------+------------------------------------------------------------------------------------------+
   | Column                | Type                  | Description                                                                              |
   +=======================+=======================+==========================================================================================+
   | statoid               | OID                   | OID of the object for which extended statistics are collected.                           |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------+
   | inherit               | Boolean               | Whether to collect statistics for objects that have inheritance relationship.            |
   |                       |                       |                                                                                          |
   |                       |                       | -  **true** indicates that the table has child tables.                                   |
   |                       |                       | -  **false** indicates that the table does not have child tables.                        |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------+
   | ndistinct             | pg_ndistinct          | N-distinct count, which is serialized as the pg_ndistinct type.                          |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------+
   | dependencies          | pg_dependencies       | Function dependency statistics, which are serialized as the pg_dependencies type.        |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------+
   | mcv                   | pg_mcv_list           | Most common value (MCV) statistics, which are serialized as the pg_mcv_list type.        |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------+
   | expr                  | pg_statistic[]        | Statistics of each expression, which is serialized as an array of the pg_statistic type. |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------+
