:original_name: dws_04_1084.html

.. _dws_04_1084:

PG_STATISTIC_INFO
=================

Records extended statistics objects of tables in the current cluster, including expression statistics objects. This system catalog is supported only by clusters of version 8.2.1.210 or later.

.. table:: **Table 1** PG_STATISTIC_INFO columns

   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------+
   | Column                | Type                  | Description                                                                                  |
   +=======================+=======================+==============================================================================================+
   | relid                 | OID                   | Table used in the extended statistics object.                                                |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------+
   | statname              | Name                  | Name of the extended statistics object.                                                      |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------+
   | statnamespace         | OID                   | OID of the namespace that contains the extended statistics object.                           |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------+
   | statowner             | OID                   | OID of the owner of the extended statistics object.                                          |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------+
   | stattarget            | Integer               | Controls the detail level of statistics.                                                     |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------+
   | keys                  | int2vector            | Array of column numbers described, indicating the columns included in the statistics object. |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------+
   | kind                  | "char"[]              | Array containing statistics types. The possible values are as follows:                       |
   |                       |                       |                                                                                              |
   |                       |                       | **d**: N-distinct statistics                                                                 |
   |                       |                       |                                                                                              |
   |                       |                       | **f**: function dependency statistics                                                        |
   |                       |                       |                                                                                              |
   |                       |                       | **m**: most common value (MCV) statistics                                                    |
   |                       |                       |                                                                                              |
   |                       |                       | **e**: expression statistics                                                                 |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------+
   | exprs                 | pg_node_tree          | Expression corresponding to the extended statistics information.                             |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------+
