:original_name: dws_04_0691.html

.. _dws_04_0691:

GS_REL_IOSTAT
=============

**GS_REL_IOSTAT** displays disk I/O statistics on the current node. In the current version, only one page is read or written in each read or write operation. Therefore, the number of read/write times is the same as the number of pages.

.. table:: **Table 1** GS_REL_IOSTAT columns

   ========= ====== =======================
   Name      Type   Description
   ========= ====== =======================
   phyrds    bigint Number of disk reads
   phywrts   bigint Number of disk writes
   phyblkrd  bigint Number of read pages
   phyblkwrt bigint Number of written pages
   ========= ====== =======================
