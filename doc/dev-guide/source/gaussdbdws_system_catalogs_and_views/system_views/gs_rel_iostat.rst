:original_name: dws_04_0691.html

.. _dws_04_0691:

GS_REL_IOSTAT
=============

**GS_REL_IOSTAT** displays disk I/O statistics on the current node. In the current version, only one page is read or written in each read or write operation. Therefore, the number of read/write times is the same as the number of pages.

.. table:: **Table 1** GS_REL_IOSTAT columns

   ========= ====== =======================
   Column    Type   Description
   ========= ====== =======================
   phyrds    Bigint Number of disk reads
   phywrts   Bigint Number of disk writes
   phyblkrd  Bigint Number of read pages
   phyblkwrt Bigint Number of written pages
   ========= ====== =======================
