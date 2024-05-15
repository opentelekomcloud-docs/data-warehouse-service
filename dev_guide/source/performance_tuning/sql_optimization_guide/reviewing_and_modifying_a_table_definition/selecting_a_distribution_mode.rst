:original_name: dws_04_0440.html

.. _dws_04_0440:

Selecting a Distribution Mode
=============================

In replication mode, full data in a table is copied to each DN in the cluster. This mode is used for tables containing a small volume of data. Full data in a table stored on each DN avoids data redistribution during the **JOIN** operation. This reduces network costs and plan segments (each with a thread), but generates much redundant data. Generally, replication is only used for small dimension tables.

In hash mode, hash values are generated for one or more columns. You can obtain the storage location of a tuple based on the mapping between DNs and the hash values. In a hash table, I/O resources on each node can be used for data read/write, which greatly accelerates the read/write of a table. Generally, a table containing a large amount of data is defined as a hash table.

The round-robin table sends each row of the table to each DN in turn, so that data is evenly distributed on each DN. This storage mode prevents data skew, improving the cluster space utilization. However, the optimization operation cannot be performed on one storage location, and the query performance is inferior to that of hash tables. If a proper distribution column can be found for a large table, use the hash distribution mode with better performance. Otherwise, define the table as a round-robin table.

+-------------+-----------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| Policy      | Description                                                                                         | Scenario                                                                                                  |
+=============+=====================================================================================================+===========================================================================================================+
| Hash        | Table data is distributed on all DNs in the cluster.                                                | Fact tables containing a large amount of data                                                             |
+-------------+-----------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| Replication | Full data in a table is stored on each DN in the cluster.                                           | Small tables and dimension tables                                                                         |
+-------------+-----------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| Round-robin | Each row of the table is sent to each DN in turn. Therefore, data is evenly distributed on each DN. | Fact tables that contain a large amount of data and cannot find a proper distribution column in hash mode |
+-------------+-----------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+

As shown in :ref:`Figure 1 <en-us_topic_0000001233681703__fig50793519161135>`, **T1** is a replication table and **T2** is a hash table.

.. _en-us_topic_0000001233681703__fig50793519161135:

.. figure:: /_static/images/en-us_image_0000001188323792.png
   :alt: **Figure 1** Replication table and hash table

   **Figure 1** Replication table and hash table
