:original_name: dws_04_0440.html

.. _dws_04_0440:

Selecting a Distribution Mode
=============================

In replication mode, full data in a table is copied to each DN in the cluster. This mode is used for tables containing a small volume of data. Full data in a table stored on each DN avoids data redistribution during the **JOIN** operation. This reduces network costs and plan segments (each with a thread), but generates much redundant data. Generally, replication is only used for small dimension tables.

In hash mode, hash values are generated for one or more columns. You can obtain the storage location of a tuple based on the mapping between DNs and the hash values. In a hash table, I/O resources on each node can be used for data read/write, which greatly accelerates the read/write of a table. Generally, a table containing a large amount of data is defined as a hash table.

+-------------+-----------------------------------------------------------+-----------------------------------------------+
| Policy      | Description                                               | Scenario                                      |
+=============+===========================================================+===============================================+
| Hash        | Table data is distributed on all DNs in the cluster.      | Fact tables containing a large amount of data |
+-------------+-----------------------------------------------------------+-----------------------------------------------+
| Replication | Full data in a table is stored on each DN in the cluster. | Small tables and dimension tables             |
+-------------+-----------------------------------------------------------+-----------------------------------------------+

As shown in :ref:`Figure 1 <en-us_topic_0000001098654758__fig50793519161135>`, **T1** is a replication table and **T2** is a hash table.

.. _en-us_topic_0000001098654758__fig50793519161135:

.. figure:: /_static/images/en-us_image_0000001145495133.png
   :alt: **Figure 1** Replication table and hash table

   **Figure 1** Replication table and hash table
