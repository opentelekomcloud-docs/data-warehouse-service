:original_name: dws_04_1068.html

.. _dws_04_1068:

PG_MATVIEW_DEPEND
=================

**PG_MATVIEW_DEPEND** records the base tables used in materialized views, along with the corresponding materialized views for each base table. This view is supported only by clusters of version 8.2.1.100 or later.

.. table:: **Table 1** PG_MATVIEW_DEPEND columns

   +--------------------+-----------+------------------------------------------------------------------+
   | Column             | Type      | Description                                                      |
   +====================+===========+==================================================================+
   | matviewid          | OID       | OID of the materialized view.                                    |
   +--------------------+-----------+------------------------------------------------------------------+
   | relid              | OID       | OID of the materialized view base table.                         |
   +--------------------+-----------+------------------------------------------------------------------+
   | partids            | oidvector | OID of the base table partition when the partition is specified. |
   +--------------------+-----------+------------------------------------------------------------------+
   | contain_entire_rel | Boolean   | Whether to include the entire base table.                        |
   +--------------------+-----------+------------------------------------------------------------------+
   | matpartid          | OID       | Partition OID of the mapped materialized view.                   |
   +--------------------+-----------+------------------------------------------------------------------+
   | relpartid          | OID       | Partition OID of the mapped base table.                          |
   +--------------------+-----------+------------------------------------------------------------------+
