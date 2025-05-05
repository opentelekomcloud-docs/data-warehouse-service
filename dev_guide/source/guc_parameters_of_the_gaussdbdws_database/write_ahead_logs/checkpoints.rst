:original_name: dws_04_0902.html

.. _dws_04_0902:

Checkpoints
===========

checkpoint_segments
-------------------

**Parameter description**: minimum number of WAL segment files in the period specified by **checkpoint_timeout**. The size of each log file is 16 MB.

**Type**: SIGHUP

**Value range**: an integer. The minimum value is **1**.

**Default value**: **64**

.. important::

   Increasing the value of this parameter speeds up the import of big data. Set this parameter based on **checkpoint_timeout** and **shared_buffers**. This parameter affects the number of WAL log segment files that can be reused. Generally, the maximum number of reused files in the **pg_xlog** folder is twice the number of checkpoint segments. The reused files are not deleted and are renamed to the WAL log segment files which will be later used.
