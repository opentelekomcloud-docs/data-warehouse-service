:original_name: dws_06_0060.html

.. _dws_06_0060:

Residual File Management Functions
==================================

Functions for Obtaining the Residual File List
----------------------------------------------

-  pg_get_residualfiles()

   Description: Obtains all residual file records of the current node. This function is an instance-level function and is irrelevant to the current database. It can run on any instance.

   Parameter type: none

   Return type: record

   The following table describes return columns.

   ============ ==== ==================
   Column       Type Description
   ============ ==== ==================
   isverified   bool Verified or not
   isdeleted    bool Deleted or not
   dbname       text Database name
   residualfile text Data file path
   filepath     text Residual file path
   notes        text Notes
   ============ ==== ==================

   Example:

   ::

      select * from pg_get_residualfiles();
       isverified | isdeleted | dbname |   residualfile    |         filepath          | notes
      ------------+-----------+--------+-------------------+---------------------------+-------
       f          | f         | db2    | base/49155/114691 | pgrf_20200908160211441546 |
       f          | f         | db2    | base/49155/114694 | pgrf_20200908160211441546 |
       f          | f         | db2    | base/49155/114696 | pgrf_20200908160211441546 |
      (3 rows)

-  pgxc_get_residualfiles()

   Description: Unified CN query function of pg_get_residualfiles() This function is a cluster-level function and is irrelevant to the current database. It runs on CNs.

   Parameter type: none

   Return type: record

   The following table describes return columns.

   ============ ==== ==================
   Column       Type Description
   ============ ==== ==================
   nodename     text Node name
   isverified   bool Verified or not
   isdeleted    bool Deleted or not
   dbname       text Database name
   residualfile text Data file path
   filepath     text Residual file path
   notes        text Notes
   ============ ==== ==================

   Example:

   ::

      select * from pgxc_get_residualfiles();
         nodename   | isverified | isdeleted |  dbname  |   residualfile    |         filepath          | notes
      --------------+------------+-----------+----------+-------------------+---------------------------+-------
       cn_5001      | f          | f         | gaussdb | base/15092/32803  | pgrf_20200910170129360401 |
       dn_6001_6002 | f          | f         | db2      | base/49155/114691 | pgrf_20200908160211441546 |
       dn_6001_6002 | f          | f         | db2      | base/49155/114694 | pgrf_20200908160211441546 |
       dn_6001_6002 | f          | f         | db2      | base/49155/114696 | pgrf_20200908160211441546 |
      (4 rows)

Functions for Verifying Residual Files
--------------------------------------

-  pg_verify_residualfiles(filepath)

   Description: Verifies whether the file recorded in the parameter specified file is a residual file. This function is an instance-level function and is related to the current database. It can run on any instance.

   Parameter type: text

   Return type: bool

   The following table describes return columns.

   ========== ==== =============================
   Column     Type Description
   ========== ==== =============================
   isverified bool Verification completed or not
   ========== ==== =============================

   Example:

   ::

      select * from pg_verify_residualfiles('pgrf_20200908160211441546');
       isverified
      ------------
       t
      (1 row)

   .. note::

      This function only verifies whether the recorded file is a residual file in the current database. If the recorded file is not in the current database, the verification is not applicable.

-  pg_verify_residualfiles()

   Description: Verifies whether recorded files on all residual file lists of the current instance are residual files. This function is an instance-level function and is related to the current database. It can run on any instance.

   Parameter type: none

   Return type: record

   The following table describes return columns.

   ======== ==== =============================
   Column   Type Description
   ======== ==== =============================
   result   bool Verification completed or not
   filepath text Residual file path
   notes    text Notes
   ======== ==== =============================

   Example:

   ::

      select * from pg_verify_residualfiles();
       result |         filepath          | notes
      --------+---------------------------+-------
       t      | pgrf_20200908160211441546 |
      (1 row)

   .. note::

      This function only verifies whether the recorded file is a residual file in the current database. If the recorded file is not in the current database, the verification is not applicable.

-  pgxc_verify_residualfiles()

   Description: Unified CN query function of pg_verify_residualfiles() This function is a cluster-level function and is related to the current database. It runs on CNs.

   Parameter type: none

   Return type: record

   The following table describes return columns.

   ======== ==== =============================
   Column   Type Description
   ======== ==== =============================
   nodename text Node name
   result   bool Verification completed or not
   filepath text Residual file path
   notes    text Notes
   ======== ==== =============================

   Example:

   ::

      select * from pgxc_verify_residualfiles();
         nodename   | result |         filepath          | notes
      --------------+--------+---------------------------+-------
       cn_5001      | t      | pgrf_20200910170129360401 |
       dn_6001_6002 | t      | pgrf_20200908160211441546 |
      (2 rows)

   .. note::

      This function only verifies whether the recorded file is a residual file in the current database. If the recorded file is not in the current database, the verification is not applicable.

-  pg_is_residualfiles(residualfile)

   Description: Queries whether a specified **relfilenode** is a residual file in the current database. This function is an instance-level function and is related to the current database. It can run on any instance.

   Parameter type: text

   Return type: bool

   The following table describes return columns.

   ====== ==== ====================
   Column Type Description
   ====== ==== ====================
   result bool Residual file or not
   ====== ==== ====================

   Example:

   ::

      select * from pg_is_residualfiles('base/49155/114691');
       result
      --------
       t
      (1 row)

   .. note::

      This function only verifies whether the recorded file is a residual file in the current database. If the recorded file is not in the current database, it is verified as a residual file.

      For example, the file **base/15092/14790** is not regarded as a residual file in a **gaussdb** database, but it is regarded as a residual file in other databases.

      select \* from pg_is_residualfiles('base/15092/14790');

      result

      ``--------``

      f

      (1 row)

      \\c db2

      db2=# select \* from pg_is_residualfiles('base/15092/14790');

      result

      ``--------``

      t

      (1 row)

Functions for Deleting Residual Files
-------------------------------------

-  pg_rm_residualfiles(filepath)

   Description: Deletes files from a specified residual file list on the current instance. This function is an instance-level function and is irrelevant to the current database. It can run on any instance.

   Parameter type: text

   Return type: record

   The following table describes return columns.

   ====== ==== =========================
   Column Type Description
   ====== ==== =========================
   result bool Deletion completed or not
   ====== ==== =========================

   Example:

   ::

      select * from pg_rm_residualfiles('pgrf_20200908160211441599');
       result
      --------
       t
      (1 row)

   .. note::

      1. Residual files can be deleted only after verification using the pg_verify_residualfiles() function.

      2. All verified files, regardless which database they are in, will be deleted.

      3. If all files recorded in the specified file have been deleted, the specified file will be removed and backed up in the **$PGDATA/pg_residualfile/backup** directory.

-  pg_rm_residualfiles()

   Description: Deletes all files recorded on all residual file lists on the current instance. This function is an instance-level function and is irrelevant to the current database. It can run on any instance.

   Parameter type: none

   Return type: record

   The following table describes return columns.

   ======== ==== ==================
   Column   Type Description
   ======== ==== ==================
   result   bool Deleted or not
   filepath text Residual file path
   notes    text Notes
   ======== ==== ==================

   Example:

   ::

      select * from pg_rm_residualfiles();
       result |         filepath          | notes
      --------+---------------------------+-------
       t      | pgrf_20200908160211441546 |
      (1 row)

   .. note::

      -  Residual files can be deleted only after verification using the pg_verify_residualfiles() function.
      -  All verified files, regardless which database they are in, will be deleted.
      -  If all files recorded in the specified file have been deleted, the specified file will be removed and backed up in the **$PGDATA/pg_residualfile/backup** directory.

-  pgxc_rm_residualfiles()

   Description: Unified CN query function of pgxc_rm_residualfiles. This function is a cluster-level function and is irrelevant to the current database. It runs on CNs.

   Parameter type: none

   Return type: record

   The following table describes return columns.

   ======== ==== =========================
   Column   Type Description
   ======== ==== =========================
   nodename text Node name
   result   bool Deletion completed or not
   filepath text Residual file path
   notes    text Notes
   ======== ==== =========================

   Example:

   ::

      select * from pgxc_rm_residualfiles();
         nodename   | result |         filepath          | notes
      --------------+--------+---------------------------+-------
       cn_5001      | t      | pgrf_20200910170129360401 |
       dn_6001_6002 | t      | pgrf_20200908160211441546 |
      (2 rows)

Using the Residual File Management Function:
--------------------------------------------

Procedure:

#. Call the **pgxc_get_residualfiles()** function to obtain the name of the database that has residual files.
#. Go to the databases where residual files exist and call the **pgxc_verify_residualfiles()** function to verify the residual files recorded in the current database.
#. Call the pgxc_rm_residualfiles() function to delete all the verified residual files.

.. note::

   The pgxc residual file management function only operates on the CN and the current primary DN, and does not verify or clear residual files on the standby DN. Therefore, after the primary DN is cleared, you need to clear residual files on the standby DN or build the standby DN in a timely manner. This prevents residual files on the standby DN from being copied back to the primary DN due to incremental build after a primary/standby switchover.

Example:

The following example uses two user-created databases, **db1** and **db2**.

|image1|

#. Run the following command to obtain all residual file records of the cluster on the CNs:

   ::

      db1=# select * from pgxc_get_residualfiles() order by 4, 6; -- order by is optional.

   |image2|

   In the current cluster:

   -  Residual file records exist in the **db1** and **db2** databases on the **dn_6001_6002** node (active node instance).
   -  Residual files are displayed in the **residualfile** column.
   -  The **filepath** column lists the files that record residual files. These files are stored in the **pg_residualfiles** directory under the instance data directory.

#. Call the **pgxc_verify_residualfiles()** function to verify the db1 database.

   ::

      db1=# select * from pgxc_verify_residualfiles();

   |image3|

   Verification functions are at the database level. Therefore, when a verification function is called in the **db1** database, it only verifies residual files in **db1**.

   You can call the **get** function again to check whether the verification is complete.

   ::

      db1=# select * from pgxc_get_residualfiles() order by 4, 6;

   |image4|

   As shown in the preceding figure, the residual files in the **db1** database have been verified, and the residual files in the db2 database are not verified.

#. Call the **pgxc_rm_residualfiles()** function to delete residual files.

   ::

      db1=# select * from pgxc_rm_residualfiles();

   |image5|

#. Call the **pgxc_get_residualfiles()** function again to check the deletion result.

   |image6|

   The result shows that the residual files in the **db1** database are deleted (**isdeleted** is marked as **t**) and the residual files in the **db2** database are not deleted.

   In addition, nine query results are displayed. Compared with the previous query results, a record for the residual file ending with **9438** is missing. This is because the record file that records the residual file ending with **9438** contains only one record, which is deleted in step 3. If all residual files in a record file are deleted, the record file is also deleted. Deleted files are backed up in the **pg_residualfiles/backup** directory.

   |image7|

#. To delete files from the **db2** database, you need to call the **verify** function in the **db2** database and then call the **rm** function.

   a. Go to the **db2** database and call the verification function.

      |image8|

      Query the verification result:

      |image9|

   b. Call the deletion function:

      |image10|

   c. Query the deletion result:

      |image11|

      All residual files recorded in the record file whose name ends with **8342** have been deleted, so the record file is deleted and backed up in the **backup** directory. As a result, no records are found.

      |image12|

.. |image1| image:: /_static/images/en-us_image_0000001145911035.png
.. |image2| image:: /_static/images/en-us_image_0000001145511057.png
.. |image3| image:: /_static/images/en-us_image_0000001145710991.png
.. |image4| image:: /_static/images/en-us_image_0000001098831060.png
.. |image5| image:: /_static/images/en-us_image_0000001098831058.png
.. |image6| image:: /_static/images/en-us_image_0000001145830907.png
.. |image7| image:: /_static/images/en-us_image_0000001098991054.png
.. |image8| image:: /_static/images/en-us_image_0000001145911037.png
.. |image9| image:: /_static/images/en-us_image_0000001098671232.png
.. |image10| image:: /_static/images/en-us_image_0000001098671234.png
.. |image11| image:: /_static/images/en-us_image_0000001145830909.png
.. |image12| image:: /_static/images/en-us_image_0000001145710989.png
