:original_name: dws_04_0903.html

.. _dws_04_0903:

Archiving
=========

archive_mode
------------

**Parameter description**: When **archive_mode** is enabled, completed WAL segments are sent to archive storage by setting **archive_command**.

**Type**: SIGHUP

**Value range**: Boolean

-  **on**: The archiving is enabled.
-  **off**: The archiving is disabled.

**Default value**: **off**

.. important::

   When :ref:`wal_level <en-us_topic_0000001510402533__s9be202a993664326975a0e79a16d60c0>` is set to **minimal**, **archive_mode** cannot be used.

archive_command
---------------

**Parameter description**: Specifies the command used to archive WALs set by the administrator. You are advised to set the archive log path to an absolute path.

**Type**: SIGHUP

**Value range**: a string

**Default value**: **(disabled)**

.. important::

   -  Any **%p** in the string is replaced with the absolute path of the file to archive, and any **%f** is replaced with only the file name. (The relative path is relative to the data directory.) Use **%%** to embed an actual **%** character in the command.

   -  This command returns zero only if it succeeds. Example:

      ::

         archive_command = 'cp --remove-destination %p /mnt/server/archivedir/%f'
         archive_command = 'copy %p /mnt/server/archivedir/%f'  # Windows

   -  **--remove-destination** indicates that files will be overwritten during the archiving.

   -  When **archive_mode** is set to **on** or not specified, a **backup** folder will be created in the **pg_xlog** directory and WALs will be compressed and copied to the **pg_xlog/backup** directory.

archive_timeout
---------------

**Parameter description**: Specifies the archiving period.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to INT_MAX. The unit is second. **0** indicates that archiving timeout is disabled.

**Default value**: **0**

.. important::

   -  The server is forced to switch to a new WAL segment file with the period specified by this parameter.
   -  Archived files that are closed early due to a forced switch are still of the same length as completely full files. Therefore, a very short **archive_timeout** will bloat the archive storage. You are advised to set **archive_timeout** to **60s**.
