:original_name: dws_16_0026.html

.. _dws_16_0026:

.. _en-us_topic_0000001860198977:

Teradata SQL Migration
======================

DSC supports the migration from Teradata to GaussDB(DWS), including the migration of schemas, DML, queries, system functions, and type conversion.

Performing Teradata SQL Migration
---------------------------------

Run the following commands to set the source database, input and output folder paths, log paths, and application language.

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

During the execution of DSC, the migration summary, including the progress and completion status, is displayed on the console. Execution information and errors will be recorded in :ref:`SQL Migration Logs <en-us_topic_0000001813598760>`.

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

For details about how to migrate Teradata SQL using DSC, see :ref:`Executing DSC <en-us_topic_0000001813598740>`.

.. note::

   During the migration, the metadata of the input script can be called and is stored in the following files:

   -  Teradata migration

      1.teradata-set-table.properties

   Clear the preceding files in the following scenarios:

   -  Migration of different files
   -  Migration of the same file with different parameter settings
