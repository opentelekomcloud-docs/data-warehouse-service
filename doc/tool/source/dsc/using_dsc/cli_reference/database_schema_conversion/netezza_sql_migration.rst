:original_name: dws_16_0216.html

.. _dws_16_0216:

Netezza SQL Migration
=====================

DSC supports the migration from Netezza to GaussDB(DWS), including the migration of schemas, DML, queries, system functions, and PL/SQL.

Run the following commands to set the source database, input and output folder paths, log paths, application language, and conversion type:

**Linux**:

.. code-block::

   ./runDSC.sh
   --source-db Netezza
   [--input-folder <input-script-path>]
   [--output-folder <output-script-path>]
   [--log-folder <log-path>]
   [--application-lang SQL]
   [--conversion-type <conversion-type>]

**Windows**:

.. code-block::

   runDSC.bat
   --source-db Netezza
   [--input-folder <input-script-path>]
   [--output-folder <output-script-path>]
   [--log-folder <log-path>]
   [--application-lang SQL]
   [--conversion-type <conversion-type>]

For example:

**Linux**:

.. code-block::

   ./runDSC.sh --source-db Netezza --input-folder /opt/DSC/DSC/input/mysql/ --output-folder /opt/DSC/DSC/output/ --application-lang SQL --conversion-type BULK --log-folder/opt/DSC/DSC/log/

**Windows**:

.. code-block::

   runDSC.bat --source-db Netezza--target-db GaussDBA --input-folder D:\test\conversion\input --output-folder D:\test\conversion\output --log-folder D:\test\conversion\log --application-lang SQL --conversion-type Bulk

During the execution of DSC, the migration summary, including the progress and completion status, is displayed on the console. Execution information and errors will be recorded in :ref:`SQL Migration Logs <en-us_topic_0000001819416117>`.

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

For details about how to migrate Netezza SQL using DSC, see :ref:`Executing DSC <en-us_topic_0000001819416093>`.
