:original_name: dws_07_0676.html

.. _dws_07_0676:

MySQL SQL Migration
===================

DSC supports the migration from MySQL to GaussDB(DWS), including the migration of schemas, DML, queries, system functions, and PL/SQL.

Performing MySQL Migration on Linux
-----------------------------------

Run the following command on Linux to start the migration. You need to specify the source database, input and output folder paths, and log paths. The application language can be SQL or Perl. The default language is SQL. The migration type can be **Bulk** or **BLogic**.

.. code-block::

   ./runDSC.sh
   --source-db MySQL
   [--input-folder <input-script-path>]
   [--output-folder <output-script-path>]
   [--application-lang SQL]
   [--conversion-type <conversion-type>]
   [--log-folder <log-path>]

During the execution of DSC, the migration summary, including the progress and completion status, is displayed on the console.

.. code-block::

   ********************** Schema Conversion Started *************************
   DSC process start time : Mon Jan 20 17:24:49 IST 2020
   Statement count progress 100% completed [FILE(1/1)]
   Schema Conversion Progress 100% completed
   **************************************************************************
   Total number of files in input folder : 1
   Total number of valid files in input folder : 1
   **************************************************************************
   Log file path :....../DSC/DSC/log/dsc.log
   Error Log file :
   DSC process end time : Mon Jan 20 17:24:49 IST 2020
   DSC total process time : 0 seconds
   ********************* Schema Conversion Completed ************************

Performing MySQL Migration on Windows
-------------------------------------

Run the following command on Windows to start the migration. You need to specify the source database, input and output folder paths, and log paths. The application language can be SQL or Perl. The default language is SQL. The migration type can be **Bulk** or **BLogic**.

.. code-block::

   runDSC.bat
   --source-db MySQL
   [--input-folder <input-script-path>]
   [--output-folder <output-script-path>]
   [--application-lang SQL]
   [--conversion-type <conversion-type>]
   [--log-folder <log-path>]

During the execution of DSC, the migration summary, including the progress and completion status, is displayed on the console.

.. code-block::

   ********************** Schema Conversion Started *************************
   DSC process start time : Mon Jan 20 17:24:49 IST 2020
   Statement count progress 100% completed [FILE(1/1)]
   Schema Conversion Progress 100% completed
   **************************************************************************
   Total number of files in input folder : 1
   **************************************************************************
   Log file path :....../DSC/DSC/log/dsc.log
   DSC process end time : Mon Jan 20 17:24:49 IST 2020
   DSC total process time : 0 seconds
   ********************* Schema Conversion Completed ************************

-  Run the following commands to migrate objects, such as functions, store procedures, and packages that contain PL/SQL statements:

   **Linux**:

   .. code-block::

      ./runDSC.sh --source-db MySQL --input-folder /opt/DSC/DSC/input/mysql/ --output-folder /opt/DSC/DSC/output/ --application-lang SQL --conversion-type BULK --log-folder /opt/DSC/DSC/log/

   **Windows**:

   .. code-block::

      runDSC.bat --source-db MySQL--target-db GaussDBA --input-folder D:\test\conversion\input --output-folder D:\test\conversion\output --log-folder D:\test\conversion\log --application-lang SQL --conversion-type Bulk

   During the execution of DSC, the migration summary, including the progress and completion status, is displayed on the console. Execution information and errors are written into :ref:`log files <dws_mt_0189>`.

   .. code-block::

      ********************** Schema Conversion Started *************************
      DSC process start time : Mon Jan 20 17:24:49 IST 2020
      Statement count progress 100% completed [FILE(1/1)]

      Schema Conversion Progress 100% completed
      **************************************************************************
      Total number of files in input folder : 1
      **************************************************************************
      Log file path :....../DSC/DSC/log/dsc.log
      DSC process end time : Mon Jan 20 17:24:49 IST 2020
      DSC total process time : 0 seconds
      ********************* Schema Conversion Completed ************************
