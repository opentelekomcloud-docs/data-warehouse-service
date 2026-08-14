:original_name: dws_04_1422.html

.. _dws_04_1422:

Overview
========

To quickly update materialized views by partition, you can create partition tables for the views and establish mappings between the views and the base tables. In this way, when certain partitions of the base table change, the refresh process can target only the affected partitions, bringing in the latest data to the corresponding partitions of the materialized view.

-  **View creation**

   Designating the partition key of the queried base table as the partition key of the materialized view can implement the automatic partition mapping between the materialized view and the base table.

-  **Partition invalidation**

   -  If data in a partition of the base table changes, the corresponding partition in the materialized view is automatically marked as invalid.
   -  Similarly, when data in the base table is modified concurrently, the materialized view flags are invalidated concurrently to prevent repeated marking.

-  **Incremental update**

   -  During a refresh of a materialized view, only invalid partitions are automatically identified and refreshed.
   -  If a partition is currently being refreshed concurrently, the system will skip the refresh to avoid repeated refresh.
   -  Updating materialized views does not interfere with data modifications in the base table.

-  **Partition DDL**

   When a partition is added or deleted in the base table, the corresponding partition in the materialized view is also added or deleted according to the established mapping rule.
