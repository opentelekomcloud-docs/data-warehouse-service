:original_name: dws_06_0404.html

.. _dws_06_0404:

Function for Obtaining File Information of Tablespaces in the Trash Directory
=============================================================================

pgxc_tablespace_trash_info()
----------------------------

Description: Obtains the file statistics of the tablespaces whose filesystem is OBS in the trash directory in a storage-compute decoupling architecture. The information includes the number of files, total file size, and whether the automatic cleanup lifecycle policy is enabled for the trash directory. This function is available only for clusters of version 9.1.0.222 or later.

Return type: record

Return columns:

+-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| Column                | Type                  | Description                                                                                                                          |
+=======================+=======================+======================================================================================================================================+
| spcname               | text                  | Tablespace name.                                                                                                                     |
+-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| file_count            | int                   | Number of tablespace-related files in the trash directory.                                                                           |
+-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| total_size            | int                   | Total size of tablespace files in the trash directory.                                                                               |
+-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| lifecycle             | text                  | Whether the expiration policy for lifecycle is added to the trash directory. This field does not take effect in the current version. |
|                       |                       |                                                                                                                                      |
|                       |                       | -  **active**: The policy is added.                                                                                                  |
|                       |                       | -  **inactive**: The policy is not added.                                                                                            |
+-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------+

Example:

::

   select * from pgxc_tablespace_trash_info();
   spcname   | file_count | total_size  | lifecycle
   ----------+------------+-------------+-----------
   cu_obs_tbs|      0     |      0      | inactive
   (1 row)
