:original_name: dws_mt_0038.html

.. _dws_mt_0038:

Teradata SQL Migration
======================

DSC supports the migration from Teradata to GaussDB(DWS), including the migration of schemas, DML, queries, system functions, and type conversion.

Performing Teradata SQL Migration
---------------------------------

Run the following commands to set the source database, input and output folder paths, log path, and application language:

**Linux:**

.. code-block::

   ./runDSC.sh
   --source-db Teradata
   [--input-folder <input-script-path>]
   [--output-folder <output-script-path>]
   [--log-folder <log-path>]
   [--application-lang SQL]

**Windows**:

.. code-block::

   runDSC.bat
   --source-db Teradata
   [--input-folder <input-script-path>]
   [--output-folder <output-script-path>]
   [--log-folder <log-path>]
   [--application-lang SQL]

For example:

**Linux**:

.. code-block::

   ./runDSC.sh --source-db Teradata --target-db GaussDBA --input-folder /opt/DSC/DSC/input/teradata/ --output-folder /opt/DSC/DSC/output/ --log-folder /opt/DSC/DSC/log/ --application-lang SQL --conversion-type Bulk

**Windows**:

.. code-block::

   runDSC.bat --source-db Teradata --target-db GaussDBA --input-folder D:\test\conversion\input --output-folder D:\test\conversion\output --log-folder D:\test\conversion\log --application-lang SQL --conversion-type Bulk

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

For details about how to migrate Teradata SQL using DSC, see :ref:`Migrating Data Using DSC <dws_mt_0035>`.

.. note::

   During the migration, the metadata of the input script can be called and is stored in the following files:

   -  Teradata migration

      teradata-set-table.properties

   -  Oracle migration

      #. global-temp-table.properties
      #. global-temp-tables.properties
      #. primary-key-constraints.properties
      #. package-definition.properties
      #. package-names-oracle.properties
      #. create-types-UDT.properties

   Clear the preceding files in the following scenarios:

   -  Migration of different files
   -  Migration of the same file with different parameter settings
