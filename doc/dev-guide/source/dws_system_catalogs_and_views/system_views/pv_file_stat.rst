:original_name: dws_04_0848.html

.. _dws_04_0848:

PV_FILE_STAT
============

By collecting statistics about the data file I/Os, **PV_FILE_STAT** displays the I/O performance of the data to detect the performance problems, such as abnormal I/O operations.

.. table:: **Table 1** PV_FILE_STAT columns

   +-----------+--------+-------------------------------------------------------------+
   | Column    | Type   | Description                                                 |
   +===========+========+=============================================================+
   | filenum   | OID    | File ID.                                                    |
   +-----------+--------+-------------------------------------------------------------+
   | dbid      | OID    | Database ID.                                                |
   +-----------+--------+-------------------------------------------------------------+
   | spcid     | OID    | Tablespace ID.                                              |
   +-----------+--------+-------------------------------------------------------------+
   | phyrds    | Bigint | Number of physical files read.                              |
   +-----------+--------+-------------------------------------------------------------+
   | phywrts   | Bigint | Number of physical files written.                           |
   +-----------+--------+-------------------------------------------------------------+
   | phyblkrd  | Bigint | Number of physical file blocks read.                        |
   +-----------+--------+-------------------------------------------------------------+
   | phyblkwrt | Bigint | Number of physical file blocks written.                     |
   +-----------+--------+-------------------------------------------------------------+
   | readtim   | Bigint | Total duration of file reads, in microseconds.              |
   +-----------+--------+-------------------------------------------------------------+
   | writetim  | Bigint | Total duration of file writes, in microseconds.             |
   +-----------+--------+-------------------------------------------------------------+
   | avgiotim  | Bigint | Average duration of file reads and writes, in microseconds. |
   +-----------+--------+-------------------------------------------------------------+
   | lstiotim  | Bigint | Duration of the last file read, in microseconds.            |
   +-----------+--------+-------------------------------------------------------------+
   | miniotim  | Bigint | Minimum duration of file reads and writes, in microseconds. |
   +-----------+--------+-------------------------------------------------------------+
   | maxiowtm  | Bigint | Maximum duration of file reads and writes, in microseconds. |
   +-----------+--------+-------------------------------------------------------------+
