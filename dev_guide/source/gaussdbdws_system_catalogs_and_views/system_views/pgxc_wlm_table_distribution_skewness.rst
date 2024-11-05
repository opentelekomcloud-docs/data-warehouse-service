:original_name: dws_04_1008.html

.. _dws_04_1008:

PGXC_WLM_TABLE_DISTRIBUTION_SKEWNESS
====================================

PGXC_WLM_TABLE_DISTRIBUTION_SKEWNESS displays data skews of tables in the current database. You can quickly query the storage space skew of all tables in the current database on each node. This view is supported only by clusters of version 8.2.1 or later.

The formula for calculating the skew rate is as follows: Skew rate (SKEW_PERCENT) = (Maximum value - Average value) x 100/Maximum value

.. table:: **Table 1** PGXC_WLM_TABLE_DISTRIBUTION_SKEWNESS columns

   +--------------+-----------------+------------------------------------------------------------------------------------------------+
   | Column       | Type            | Description                                                                                    |
   +==============+=================+================================================================================================+
   | schema_name  | name            | Name of the schema where a table is                                                            |
   +--------------+-----------------+------------------------------------------------------------------------------------------------+
   | table_name   | name            | Table name                                                                                     |
   +--------------+-----------------+------------------------------------------------------------------------------------------------+
   | total_size   | numeric         | Total storage space of a table on all nodes, in bytes                                          |
   +--------------+-----------------+------------------------------------------------------------------------------------------------+
   | avg_size     | numeric(1000,0) | Average storage space of a table on each node, in bytes                                        |
   +--------------+-----------------+------------------------------------------------------------------------------------------------+
   | max_percent  | numeric         | Percentage (%) of the maximum storage space of a table on each node to the total storage space |
   +--------------+-----------------+------------------------------------------------------------------------------------------------+
   | min_percent  | numeric         | Percentage (%) of the minimum storage space of a table on each node to the total storage space |
   +--------------+-----------------+------------------------------------------------------------------------------------------------+
   | skew_percent | numeric         | Skew rate (%) of a table                                                                       |
   +--------------+-----------------+------------------------------------------------------------------------------------------------+

.. note::

   -  To use this view to query the storage distribution information of a specified table, you must have the **SELECT** permission on the table.
   -  This function is based on the physical file storage space recorded in the :ref:`PG_RELFILENODE_SIZE <dws_04_1611>` system catalog. Ensure that the GUC parameters **use_workload_manager** and **enable_perm_space** are enabled.
   -  When you analyze the disk space skew of each table in a database in a large cluster with a large amount of data, the **PGXC_WLM_TABLE_DISTRIBUTION_SKEWNESS** view delivers better query performance than the **gs_table_distribution()** function and the **PGXC_GET_TABLE_SKEWNESS** view. You are advised to use the **PGXC_WLM_TABLE_DISTRIBUTION_SKEWNESS** view to query the table skew status overview, and then use the **gs_table_distribution(schemaname text, tablename text)** function to obtain the disk space distribution of a specified table on each node.

Example
-------

You can use the **PGXC_WLM_TABLE_DISTRIBUTION_SKEWNESS** view to query the table skew status overview, and then use the **gs_table_distribution(schemaname text, tablename text)** function to obtain the disk space distribution of a specified table on each node.

#. Use the **PGXC_WLM_TABLE_DISTRIBUTION_SKEWNESS** view to query the table skew status overview.

   .. code-block::

      tpcds_col=# select * from pgxc_wlm_table_distribution_skewness;

   The query result is as follows:

   |image1|

   The data skew of the **dbgen_version** table is severe.

#. Use the **gs_table_distribution(schemaname text, tablename text)** function to query the disk space distribution of the **dbgen_version** table on each node.

   .. code-block::

      tpcds_col=# select * from gs_table_distribution('public','dbgen_version');

   The query result is as follows:

   |image2|

   According to the preceding information, data skew occurs in the disk space occupied by the table on DNs. Most data is stored on **dn_6005_6006**.

.. |image1| image:: /_static/images/en-us_image_0000001481220582.png
.. |image2| image:: /_static/images/en-us_image_0000001532145549.png
