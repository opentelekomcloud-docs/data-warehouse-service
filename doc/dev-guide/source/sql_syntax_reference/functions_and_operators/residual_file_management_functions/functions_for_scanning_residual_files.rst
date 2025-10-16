:original_name: dws_06_0373.html

.. _dws_06_0373:

Functions for Scanning Residual Files
=====================================

Only cluster 8.3.0 and later versions support the function of scanning residual files. All functions described in this section can be invoked only by the **sysadmin** role.

pg_scan_residualfiles()
-----------------------

Description: This function scans all residual file records in the database connected to the current node. It is an instance-level function and is related to the current database, and it can run on any instance.

-  When it is executed on a CN, it scans the database of the CN and OBS for residual files.
-  When it is executed on a CN, it scans the database of the DN for residual files.

Return type: record

The following information is displayed:

.. table:: **Table 1** Return fields

   +--------+------+-------------------------------------------------------------------------+
   | Field  | Type | Description                                                             |
   +========+======+=========================================================================+
   | pgscrf | text | Local path of the metadata file that records residual file information. |
   +--------+------+-------------------------------------------------------------------------+

Example:

::

   SELECT * FROM pg_scan_residualfiles();
                                  pgscrf
   --------------------------------------------------------------------
    pg_residualfiles/pgscrf_meta_1663_16323_20231027143716005244
    pg_residualfiles/pgscrf_meta_2147484281_16323_20231027143716005713
   (2 rows)

.. note::

   The restrictions on using this function are as follows:

   -  Only the current database can be scanned for residual files. Currently, only the default tablespace, OBS tablespace, and user-defined tablespace are scanned.
   -  This function is a heavy-load operation and cannot be used concurrently on a single node. You should avoid using this function when the service is overloaded or the resource is scarce.
   -  Only hot partitions in cold-hot-partition-separated tables are supported. Cold partitions are not supported.
   -  Do not use this function during the upgrade observation period.
   -  Residual files in the deleted database cannot be scanned.
   -  Inconsistent cluster metadata information can cause this function incorrectly scan valid data files.

pgxc_scan_residualfiles(query_flag)
-----------------------------------

Description: Unified CN execution function of **pg_scan_residualfiles()** This function is a cluster-level function and is related to the current database. It runs on CNs.

Parameter description: **query_flag**. The parameter type is int, indicating the execution range. **1** indicates the CN, **2** indicates the primary DN, and **4** indicates the standby DN. The query union set can be obtained through the OR operation. For example, **1|2=3** indicates the CN and primary DN, and **1|2|4=7** indicates the CN, primary DN, and standby DN. The default value is **7**.

Return type: record

The command output is as follows.

.. table:: **Table 2** Return fields

   +-------------+------+-------------------------------------------------------------------------+
   | Field       | Type | Description                                                             |
   +=============+======+=========================================================================+
   | nodename    | name | Node name.                                                              |
   +-------------+------+-------------------------------------------------------------------------+
   | instance_id | text | Instance name.                                                          |
   +-------------+------+-------------------------------------------------------------------------+
   | pgscrf      | text | Local path of the metadata file that records residual file information. |
   +-------------+------+-------------------------------------------------------------------------+

Example:

::

   SELECT * FROM pgxc_scan_residualfiles();
     node_name   | instance_id |                               pgscrf
   --------------+-------------+--------------------------------------------------------------------
    dn_6001_6002 | dn_6001     | pg_residualfiles/pgscrf_meta_1663_16323_20231027144103839354
    dn_6001_6002 | dn_6001     | pg_residualfiles/pgscrf_meta_2147484281_16323_20231027144103839826
    cn_5001      | cn_5001     | pg_residualfiles/pgscrf_meta_1663_16323_20231027144103946217
    dn_6007_6008 | dn_6008     | pg_residualfiles/pgscrf_meta_1663_16323_20231027144104171311
   (4 rows)

   SELECT * FROM pgxc_scan_residualfiles(1);
    node_name | instance_id |                            pgscrf
   -----------+-------------+--------------------------------------------------------------
    cn_5001   | cn_5001     | pg_residualfiles/pgscrf_meta_1663_16323_20231027144421861628
   (1 row)

   SELECT * FROM pgxc_scan_residualfiles(2);
     node_name   | instance_id |                               pgscrf
   --------------+-------------+--------------------------------------------------------------------
    dn_6001_6002 | dn_6001     | pg_residualfiles/pgscrf_meta_1663_16323_20231027144424210395
    dn_6001_6002 | dn_6001     | pg_residualfiles/pgscrf_meta_2147484281_16323_20231027144424210855
   (2 rows)

   SELECT * FROM pgxc_scan_residualfiles(4);
     node_name   | instance_id |                            pgscrf
   --------------+-------------+--------------------------------------------------------------
    dn_6007_6008 | dn_6008     | pg_residualfiles/pgscrf_meta_1663_16323_20231027144427492060
   (1 row)

.. note::

   The restrictions on using this function are as follows:

   -  Only the current database can be scanned for residual files. Currently, only the default tablespace, OBS tablespace, and user-defined tablespace are scanned.
   -  This function is a heavy-load operation and cannot be used concurrently on a single node. You should avoid using this function when the service is overloaded or the resource is scarce.
   -  Only hot partitions in cold-hot-partition-separated tables are supported. Cold partitions are not supported.
   -  Do not use this function during the upgrade observation period.
   -  Residual files in the deleted database cannot be scanned.
   -  Inconsistent cluster metadata information can cause this function incorrectly scan valid data files.

pg_get_scan_residualfiles()
---------------------------

Description: Obtains all residual file records scanned on the current node. This function is an instance-level function and is irrelevant to the current database. It can run on any instance.

Return type: record

The command output is as follows.

.. table:: **Table 3** Return fields

   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Field        | Type        | Description                                                                                                                                 |
   +==============+=============+=============================================================================================================================================+
   | handled      | bool        | Whether residual files have been processed, moved, or modified.                                                                             |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | dbname       | text        | Database name                                                                                                                               |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | residualfile | text        | Path of the residual file                                                                                                                   |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | size         | bigint      | File size, in bytes. For residual files in the OBS path, the value of this parameter is **0**.                                              |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | inode        | bigint      | Inode in the stat information of the residual file. For residual files in the OBS path, the value of this parameter is **0**.               |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | atime        | timestamptz | **Access time** in the stat information of the residual file. For residual files in the OBS path, the value of this parameter is empty.     |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | mtime        | timestamptz | **Modify time** in the **stat** information of the residual file. For residual files in the OBS path, the value of this parameter is empty. |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | ctime        | timestamptz | **Change time** in the stat information of the residual file. For residual files in the OBS path, the value of this parameter is empty.     |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | filepath     | text        | Local path of the metadata file that records residual file information, corresponding to the **pgscrf_meta** file.                          |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | notes        | text        | Notes                                                                                                                                       |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+

pgxc_get_scan_residualfiles(query_flag)
---------------------------------------

Description: Unified CN execution function of **pg_get_scan_residualfiles()** This function is a cluster-level function and is irrelevant to the current database. It runs on CNs.

The **query_flag** parameter is of the int type and indicates the execution range. **1** indicates the CN, **2** indicates the primary DN, and **4** indicates the standby DN. The query union set can be obtained through the OR operation. For example, **1|2=3** indicates the CN and primary DN, and **1|2|4=7** indicates the CN, primary DN, and standby DN. The default value is **7**.

Return type: record

The command output is as follows.

.. table:: **Table 4** Return fields

   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Field        | Type        | Description                                                                                                                                 |
   +==============+=============+=============================================================================================================================================+
   | nodename     | name        | Node name                                                                                                                                   |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | instance_id  | text        | Instance name                                                                                                                               |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | handled      | bool        | Whether residual files have been processed, moved, or modified.                                                                             |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | dbname       | text        | Database name                                                                                                                               |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | residualfile | text        | Path of the residual file                                                                                                                   |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | size         | bigint      | File size, in bytes. For residual files in the OBS path, the value of this parameter is **0**.                                              |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | inode        | bigint      | Inode in the stat information of the residual file. For residual files in the OBS path, the value of this parameter is **0**.               |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | atime        | timestamptz | **Access time** in the stat information of the residual file. For residual files in the OBS path, the value of this parameter is empty.     |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | mtime        | timestamptz | **Modify time** in the **stat** information of the residual file. For residual files in the OBS path, the value of this parameter is empty. |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | ctime        | timestamptz | **Change time** in the stat information of the residual file. For residual files in the OBS path, the value of this parameter is empty.     |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | filepath     | text        | Local path of the metadata file that records residual file information, corresponding to the **pgscrf_meta** file.                          |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | notes        | text        | Notes                                                                                                                                       |
   +--------------+-------------+---------------------------------------------------------------------------------------------------------------------------------------------+

pg_archive_scan_residualfiles()
-------------------------------

Description: Archives all residual file records scanned on the database connected to the current node. This function is an instance-level function and is related to the current database. It can run on any instance.

Return type: record

The command output is as follows.

.. table:: **Table 5** Return fields

   +---------+--------+------------------------------------------------------------------------------------------------------------------------------------+
   | Field   | Type   | Description                                                                                                                        |
   +=========+========+====================================================================================================================================+
   | archive | text   | Path of the local folder after archiving. Residual files in the OBS path are archived in the corresponding OBS database directory. |
   +---------+--------+------------------------------------------------------------------------------------------------------------------------------------+
   | count   | bigint | Number of archived residual files.                                                                                                 |
   +---------+--------+------------------------------------------------------------------------------------------------------------------------------------+
   | size    | bigint | Total size of archived residual files, in bytes.                                                                                   |
   +---------+--------+------------------------------------------------------------------------------------------------------------------------------------+

Example:

::

   SELECT * FROM pg_archive_scan_residualfiles();
                              archive                            | count | size
   --------------------------------------------------------------+-------+-------
    pg_residualfiles/archive/pgscrf_archive_20231027144613791801 |     4 | 81920
   (1 row)

.. note::

   -  This function can archive only the residual files recorded in the current login database. During archiving, the residual files are verified. The verification results are as follows:

      -  Verification passed: After the verification is passed, the residual files are archived and marked as handled.
      -  Verification failed: If the verification fails, the residual files are not archived and marked as handled.
      -  Verification delayed: If the verification is delayed, it indicates that verification is not possible at the moment, and archiving is skipped. The verification delay is related to the transaction completion status and the standby node's redo progress.

   -  The actual archive directory and the corresponding tablespace are in the same file system. Deleting the tablespace also deletes the archived residual files.
   -  This function cannot be used after DDL delay is enabled.
   -  This function is a heavy-load operation and cannot be used concurrently on a single node. You should avoid using this function when the service is overloaded or the resource is scarce.

pgxc_archive_scan_residualfiles(query_flag)
-------------------------------------------

Description: Unified CN execution function of **pg_archive_scan_residualfiles()** This function is a cluster-level function and is related to the current database. It runs on CNs.

The **query_flag** parameter is of the int type and indicates the execution range. **1** indicates the CN, **2** indicates the primary DN, and **4** indicates the standby DN. The query union set can be obtained through the OR operation. For example, **1|2=3** indicates the CN and primary DN, and **1|2|4=7** indicates the CN, primary DN, and standby DN. The default value is **7**.

Return type: record

The command output is as follows.

.. table:: **Table 6** Return fields

   +-------------+--------+------------------------------------------------------------------------------------------------------------------------------------+
   | Field       | Type   | Description                                                                                                                        |
   +=============+========+====================================================================================================================================+
   | nodename    | name   | Node name.                                                                                                                         |
   +-------------+--------+------------------------------------------------------------------------------------------------------------------------------------+
   | instance_id | text   | Instance name.                                                                                                                     |
   +-------------+--------+------------------------------------------------------------------------------------------------------------------------------------+
   | archive     | text   | Path of the local folder after archiving. Residual files in the OBS path are archived in the corresponding OBS database directory. |
   +-------------+--------+------------------------------------------------------------------------------------------------------------------------------------+
   | count       | bigint | Number of archived residual files.                                                                                                 |
   +-------------+--------+------------------------------------------------------------------------------------------------------------------------------------+
   | size        | bigint | Total size of archived residual files, in bytes.                                                                                   |
   +-------------+--------+------------------------------------------------------------------------------------------------------------------------------------+

Example:

::

   SELECT * FROM pgxc_archive_scan_residualfiles();
     node_name   | instance_id |                           archive                            | count | size
   --------------+-------------+--------------------------------------------------------------+-------+-------
    cn_5001      | cn_5001     | pg_residualfiles/archive/pgscrf_archive_20231027145050896440 |     1 | 40960
    dn_6007_6008 | dn_6008     | pg_residualfiles/archive/pgscrf_archive_20231027145051018138 |     1 | 24576
   (2 rows)

.. note::

   -  This function can archive only the residual files recorded in the current login database. During archiving, the residual files are verified. The verification results are as follows:

      -  Verification passed: After the verification is passed, the residual files are archived and marked as handled.
      -  Verification failed: If the verification fails, the residual files are not archived and marked as handled.
      -  Verification delayed: If the verification is delayed, it indicates that verification is not possible at the moment, and archiving is skipped. The verification delay is related to the transaction completion status and the standby node's redo progress.

   -  The actual archive directory and the corresponding tablespace are in the same file system. Deleting the tablespace also deletes the archived residual files.
   -  This function cannot be used after DDL delay is enabled.
   -  This function is a heavy-load operation and cannot be used concurrently on a single node. You should avoid using this function when the service is overloaded or the resource is scarce.

pg_rm_scan_residualfiles_archive()
----------------------------------

Description: Deletes all archived residual file records on the current node. This function is an instance-level function and is irrelevant to the current database. It can run on any instance.

Return type: record

The command output is as follows.

.. table:: **Table 7** Return fields

   +-------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Field | Type   | Description                                                                                                                                                                                                                             |
   +=======+========+=========================================================================================================================================================================================================================================+
   | count | bigint | Number of residual files that have been deleted from the archive. For residual files in the local path, the number of deleted files is counted. For residual files in the OBS path, the number of deleted table directories is counted. |
   +-------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | size  | bigint | Total size of residual files that have been deleted from the archive, in bytes. For residual files in the OBS path, the value of this parameter is **0**.                                                                               |
   +-------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example:

::

   SELECT * FROM pg_rm_scan_residualfiles_archive();
    count | size
   -------+-------
        4 | 81920
   (1 row)

pgxc_rm_scan_residualfiles_archive(query_flag)
----------------------------------------------

Description: Unified CN execution function of **pg_rm_scan_residualfiles_archive()** This function is a cluster-level function and is irrelevant to the current database. It runs on CNs.

The **query_flag** parameter is of the int type and indicates the execution range. **1** indicates the CN, **2** indicates the primary DN, and **4** indicates the standby DN. The query union set can be obtained through the OR operation. For example, **1|2=3** indicates the CN and primary DN, and **1|2|4=7** indicates the CN, primary DN, and standby DN. The default value is **7**.

Return type: record

The command output is as follows.

.. table:: **Table 8** Return fields

   +-------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Field       | Type   | Description                                                                                                                                                                                                                             |
   +=============+========+=========================================================================================================================================================================================================================================+
   | nodename    | name   | Node name.                                                                                                                                                                                                                              |
   +-------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | instance_id | text   | Instance name.                                                                                                                                                                                                                          |
   +-------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | count       | bigint | Number of residual files that have been deleted from the archive. For residual files in the local path, the number of deleted files is counted. For residual files in the OBS path, the number of deleted table directories is counted. |
   +-------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | size        | bigint | Total size of residual files that have been deleted from the archive, in bytes. For residual files in the OBS path, the value of this parameter is **0**.                                                                               |
   +-------------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example:

::

   SELECT * FROM pgxc_rm_scan_residualfiles_archive();
     node_name   | instance_id | count | size
   --------------+-------------+-------+-------
    dn_6001_6002 | dn_6001     |     4 | 81920
    cn_5001      | cn_5001     |     1 | 40960
    dn_6007_6008 | dn_6008     |     1 | 24576
    coordinator1 | coordinator1|     1 |    0
   (4 rows)
