:original_name: dws_06_0348.html

.. _dws_06_0348:

Functions for Verifying Residual Files
======================================

pg_verify_residualfiles(filepath)
---------------------------------

Description: Verifies whether the file recorded in the parameter specified file is a residual file. This function is an instance-level function and is related to the current database. It can run on any instance.

Parameter type: text

Return type: bool

The following table describes return columns.

.. table:: **Table 1** pg_verify_residualfiles (filepath) return fields

   ========== ==== =====================================
   Column     Type Description
   ========== ==== =====================================
   isverified bool Whether the verification is complete.
   ========== ==== =====================================

Example:

::

   postgres=#SELECT * FROM pg_verify_residualfiles('pgrf_20200908160211441546');
    isverified
   ------------
    t
   (1 row)

.. note::

   This function only verifies whether the recorded file is a residual file in the current database. If the recorded file is not in the current database, the verification is not applicable.

pg_verify_residualfiles()
-------------------------

Description: Verifies whether recorded files on all residual file lists of the current instance are residual files. This function is an instance-level function and is related to the current database. It can run on any instance.

Parameter type: none

Return type: record

The following table describes return columns.

.. table:: **Table 2** pg_verify_residualfiles () return fields

   ======== ==== =====================================
   Column   Type Description
   ======== ==== =====================================
   result   bool Whether the verification is complete.
   filepath text Residual file path.
   notes    text Notes.
   ======== ==== =====================================

Example:

::

   postgres=#SELECT * FROM pg_verify_residualfiles();
    result |         filepath          | notes
   --------+---------------------------+-------
    t      | pgrf_20200908160211441546 |
   (1 row)

.. note::

   This function only verifies whether the recorded file is a residual file in the current database. If the recorded file is not in the current database, the verification is not applicable.

pgxc_verify_residualfiles()
---------------------------

Description: Unified CN query function of pg_verify_residualfiles() This function is a cluster-level function and is related to the current database. It runs on CNs.

Parameter type: none

Return type: record

The following table describes return columns.

.. table:: **Table 3** pgxc_verify_residualfiles () return fields

   ======== ==== =====================================
   Column   Type Description
   ======== ==== =====================================
   nodename text Node name.
   result   bool Whether the verification is complete.
   filepath text Residual file path.
   notes    text Notes.
   ======== ==== =====================================

Example:

::

   postgres=#SELECT * FROM pgxc_verify_residualfiles();
      nodename   | result |         filepath          | notes
   --------------+--------+---------------------------+-------
    cn_5001      | t      | pgrf_20200910170129360401 |
    dn_6001_6002 | t      | pgrf_20200908160211441546 |
   (2 rows)

.. note::

   This function only verifies whether the recorded file is a residual file in the current database. If the recorded file is not in the current database, the verification is not applicable.

pg_is_residualfiles(residualfile)
---------------------------------

Description: Queries whether a specified **relfilenode** is a residual file in the current database. This function is an instance-level function and is related to the current database. It can run on any instance.

Parameter type: text

Return type: bool

The following table describes return columns.

.. table:: **Table 4** pg_is_residualfiles (residualfile) return fields

   ====== ==== ==================================
   Column Type Description
   ====== ==== ==================================
   result bool Whether a file is a residual file.
   ====== ==== ==================================

Example:

::

   postgres=#SELECT * FROM pg_is_residualfiles('base/49155/114691');
    result
   --------
    t
   (1 row)

.. note::

   This function only verifies whether the recorded file is a residual file in the current database. If the recorded file is not in the current database, it is verified as a residual file.

   For example, the file **base/15092/14790** is not regarded as a residual file in a **gaussdb** database, but it is regarded as a residual file in other databases.

   .. code-block::

      postgres=#SELECT * FROM pg_is_residualfiles('base/15092/14790');
      result
      --------
      f
      (1 row)

      postgres=#\c db2
      db2=# SELECT * FROM pg_is_residualfiles('base/15092/14790');
      result
      --------
      t
      (1 row)
