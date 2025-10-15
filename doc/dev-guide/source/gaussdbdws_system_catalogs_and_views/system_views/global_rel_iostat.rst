:original_name: dws_04_0683.html

.. _dws_04_0683:

GLOBAL_REL_IOSTAT
=================

**GLOBAL_REL_IOSTAT** displays the total disk I/O statistics of all nodes in a cluster. The name of each column in this view is the same as that in the :ref:`GS_REL_IOSTAT <dws_04_0691>` view, but the column meaning is the sum of the value of the same column in the **GS_REL_IOSTAT** view on each node.

.. table:: **Table 1** GLOBAL_REL_IOSTAT columns

   ========= ====== ===============================================
   Column    Type   Description
   ========= ====== ===============================================
   phyrds    Bigint Total number of disk read times of all nodes
   phywrts   Bigint Total number of disk write times of all nodes
   phyblkrd  Bigint Total number of disk pages read by all nodes
   phyblkwrt Bigint Total number of disk pages written by all nodes
   ========= ====== ===============================================

.. note::

   This view is accessible only to users with system administrator rights.
