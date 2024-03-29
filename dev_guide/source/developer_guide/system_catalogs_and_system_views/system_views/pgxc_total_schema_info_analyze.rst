:original_name: dws_04_0829.html

.. _dws_04_0829:

PGXC_TOTAL_SCHEMA_INFO_ANALYZE
==============================

**PGXC_TOTAL_SCHEMA_INFO_ANALYZE** displays the overall schema space information of the cluster, including the total cluster space, average space of instances, skew ratio, maximum space of a single instance, minimum space of a single instance, and names of the instances with the maximum space and minimum space. It provides visibility into the schema space usage of the entire cluster. This view can be queried only on CNs.

.. table:: **Table 1** PGXC_TOTAL_SCHEMA_INFO_ANALYZE columns

   +--------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name         | Type    | Description                                                                                                                                                                     |
   +==============+=========+=================================================================================================================================================================================+
   | schemaname   | text    | Schema name                                                                                                                                                                     |
   +--------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | databasename | text    | Database name                                                                                                                                                                   |
   +--------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | nodegroup    | text    | Name of the node group                                                                                                                                                          |
   +--------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | total_value  | bigint  | Total cluster space in the current schema                                                                                                                                       |
   +--------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avg_value    | bigint  | Average space of instances in the current schema                                                                                                                                |
   +--------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | skew_percent | integer | Skew ratio                                                                                                                                                                      |
   +--------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | extend_info  | text    | Extended information, including the maximum space of a single instance, minimum space of a single instance, and names of the instances with the maximum sapce and minimum space |
   +--------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
