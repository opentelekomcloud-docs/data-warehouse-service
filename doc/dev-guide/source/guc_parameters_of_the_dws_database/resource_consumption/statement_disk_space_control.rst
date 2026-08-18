:original_name: dws_04_0894.html

.. _dws_04_0894:

Statement Disk Space Control
============================

This section describes parameters related to statement disk space control, which are used to limit the disk space usage of statements.

sql_use_spacelimit
------------------

**Parameter description**: Specifies the allowed maximum space for files to be spilled to disks in a single SQL statement on a single DN. This parameter limits the space occupied by ordinary tables, temporary tables, and intermediate result sets spilled to disks. System administrators are also restricted by this parameter.

**Type**: USERSET

**Value range**: an integer ranging from -1 to INT_MAX. The unit is KB. **-1** indicates no limit.

**Default value**: Set **sql_use_spacelimit** to 10% of the total disk space of the instance.

.. note::

   For example, if **sql_use_spacelimit** is set to **100** in the statement, and the data spilled to disks on a single DN exceeds 100 KB, DWS will stop the query and display a message indicating threshold exceeded.

   ::

      insert into user1.t1 select * from user2.t1;
      ERROR:  The space used on DN (104 kB) has exceeded the sql use space limit (100 kB).

   Handling suggestion:

   -  Optimize the statement to reduce the data spilled to disks.
   -  If the disk space is sufficient, increase the value of this parameter.

temp_file_limit
---------------

**Parameter description**: Specifies the total space for files spilled to disks in a single thread. The temporary file can be the one used by sorting or hash tables, or cursors in a session.

This is a session-level setting.

**Type**: SUSET

**Value range**: an integer ranging from -1 to INT_MAX. The unit is KB. **-1** indicates no limit.

**Default value**: Set **temp_file_limit** to 10% of the total disk space of the instance.

.. important::

   This parameter does not apply to disk space occupied by temporary tablespaces used for executing SQL queries.

bi_page_reuse_factor
--------------------

**Parameter description**: Specifies the percentage of idle space of old pages that can be reused when page replication is used for data synchronization between primary and standby DNs in the scenario where data is inserted into row-store tables in batches.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 100. The value is a percentage. Value **0** indicates that the old pages are not reused and new pages are requested.

**Default value**: **70**

.. important::

   -  You are not advised to set this parameter to a value less than **50** (except **0**). If the idle space of the reused page is small, too much old page data will be transmitted between the primary and standby DNs. As a result, the batch insertion performance deteriorates.
   -  You are not advised to set this parameter to a value greater than **90**. If this parameter is set to a value greater than **90**, idle pages will be frequently queried, but old pages cannot be reused.
