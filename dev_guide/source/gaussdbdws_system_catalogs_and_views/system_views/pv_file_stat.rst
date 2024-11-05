:original_name: dws_04_0848.html

.. _dws_04_0848:

PV_FILE_STAT
============

By collecting statistics about the data file I/Os, **PV_FILE_STAT** displays the I/O performance of the data to detect the performance problems, such as abnormal I/O operations.

.. table:: **Table 1** PV_FILE_STAT columns

   +-----------+--------+-------------------------------------------------------------------------+
   | Name      | Type   | Description                                                             |
   +===========+========+=========================================================================+
   | filenum   | oid    | File ID                                                                 |
   +-----------+--------+-------------------------------------------------------------------------+
   | dbid      | oid    | Database ID                                                             |
   +-----------+--------+-------------------------------------------------------------------------+
   | spcid     | oid    | Tablespace ID                                                           |
   +-----------+--------+-------------------------------------------------------------------------+
   | phyrds    | bigint | Number of times of reading physical files                               |
   +-----------+--------+-------------------------------------------------------------------------+
   | phywrts   | bigint | Number of times of writing into physical files                          |
   +-----------+--------+-------------------------------------------------------------------------+
   | phyblkrd  | bigint | Number of times of reading physical file blocks                         |
   +-----------+--------+-------------------------------------------------------------------------+
   | phyblkwrt | bigint | Number of times of writing into physical file blocks                    |
   +-----------+--------+-------------------------------------------------------------------------+
   | readtim   | bigint | Total duration of reading files. The unit is microsecond.               |
   +-----------+--------+-------------------------------------------------------------------------+
   | writetim  | bigint | Total duration of writing files. The unit is microsecond.               |
   +-----------+--------+-------------------------------------------------------------------------+
   | avgiotim  | bigint | Average duration of reading and writing files. The unit is microsecond. |
   +-----------+--------+-------------------------------------------------------------------------+
   | lstiotim  | bigint | Duration of the last file reading. The unit is microsecond.             |
   +-----------+--------+-------------------------------------------------------------------------+
   | miniotim  | bigint | Minimum duration of reading and writing files. The unit is microsecond. |
   +-----------+--------+-------------------------------------------------------------------------+
   | maxiowtm  | bigint | Maximum duration of reading and writing files. The unit is microsecond. |
   +-----------+--------+-------------------------------------------------------------------------+
