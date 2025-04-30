:original_name: dws_04_0816.html

.. _dws_04_0816:

PGXC_REL_IOSTAT
===============

**PGXC_REL_IOSTAT** displays disk read/write statistics on each node in the cluster. This view is accessible only to users with system administrators rights.

.. table:: **Table 1** PGXC_REL_IOSTAT columns

   ========= ====== =======================
   Column    Type   Description
   ========= ====== =======================
   node_name Text   Node name
   phyrds    Bigint Number of disk reads
   phywrts   Bigint Number of disk writes
   phyblkrd  Bigint Number of read pages
   phyblkwrt Bigint Number of written pages
   ========= ====== =======================
