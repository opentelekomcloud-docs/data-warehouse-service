:original_name: dws_16_0020.html

.. _dws_16_0020:

.. _en-us_topic_0000001819416093:

Executing DSC
=============

Important Notes
---------------

-  Before starting DSC, specify the output folder path. Separate the input folder path, output folder path, and log path with spaces. The input folder path cannot contain spaces which will cause an error when DSC is used for migrating data. For details, see :ref:`Troubleshooting <en-us_topic_0000001819336325>`.

-  If the output folder contains subfolders or files, DSC deletes the subfolders and files or overwrites them based on parameter settings in the **application.properties** configuration file in the **config** folder before the migration. Deleted or overwritten subfolders and files cannot be restored by DSC.
-  If migration tasks are performed concurrently on the same server (by the same or different DSCs), different migration tasks must use different output folder paths and log paths.
-  You can specify a log path by configuring optional parameters. If the path is not specified, DSC automatically creates a log folder under **TOOL_HOME**. For details, see :ref:`Log Reference <en-us_topic_0000001772696088>`.

Migration Methods
-----------------

You can run the **runDSC.sh** or **runDSC.bat** command to perform a migration task on Windows and Linux. For details, see :ref:`Table 1 <en-us_topic_0000001819416093__en-us_topic_0000001706224057_en-us_topic_0000001432327697_table296962642912>`.

.. _en-us_topic_0000001819416093__en-us_topic_0000001706224057_en-us_topic_0000001432327697_table296962642912:

.. table:: **Table 1** Migration on Windows and Linux

   +---------------------------------------------------------------+-------------------------------------------------------+
   | Migration                                                     | CLI Parameter                                         |
   +===============================================================+=======================================================+
   | :ref:`Teradata SQL Migration <en-us_topic_0000001772696076>`  | .. code-block::                                       |
   |                                                               |                                                       |
   |                                                               |    > ./runDSC.sh                                      |
   |                                                               |     --source-db Teradata                              |
   |                                                               |    [--application-lang SQL]                           |
   |                                                               |    [ --input-folder <input-script-path> ]             |
   |                                                               |    [--output-folder <output-script-path> ]            |
   |                                                               |    [--log-folder <log-path>]                          |
   |                                                               |    [--target-db/-T][Optional]                         |
   |                                                               |                                                       |
   |                                                               | .. code-block::                                       |
   |                                                               |                                                       |
   |                                                               |    > runDSC.bat                                       |
   |                                                               |    --source-db Teradata                               |
   |                                                               |    [--application-lang SQL]                           |
   |                                                               |    [ --input-folder <input-script-path> ]             |
   |                                                               |    [--output-folder <output-script-path> ]            |
   |                                                               |    [--log-folder <log-path>]                          |
   |                                                               |    [--target-db/-T][Optional]                         |
   +---------------------------------------------------------------+-------------------------------------------------------+
   | :ref:`Teradata Perl Migration <en-us_topic_0000001772536404>` | .. code-block::                                       |
   |                                                               |                                                       |
   |                                                               |    > ./runDSC.sh                                      |
   |                                                               |    --source-db Teradata                               |
   |                                                               |    [--application-lang Perl]                          |
   |                                                               |    [--input-folder <input-script-path> ]              |
   |                                                               |    [--output-folder <output-script-path> ]            |
   |                                                               |    [--log-folder <log-path>]                          |
   |                                                               |    [--target-db/-T][Optional]                         |
   |                                                               |                                                       |
   |                                                               | .. code-block::                                       |
   |                                                               |                                                       |
   |                                                               |    > runDSC.bat                                       |
   |                                                               |      --source-db Teradata                             |
   |                                                               |    [--application-lang Perl]                          |
   |                                                               |    [--input-folder <input-script-path> ]              |
   |                                                               |    [--output-folder <output-script-path> ]            |
   |                                                               |    [--log-folder <log-path>]                          |
   |                                                               |    [--target-db/-T][Optional]                         |
   +---------------------------------------------------------------+-------------------------------------------------------+
   | :ref:`MySQL SQL Migration <en-us_topic_0000001819416105>`     | .. code-block::                                       |
   |                                                               |                                                       |
   |                                                               |    > ./runDSC.sh                                      |
   |                                                               |    --source-db MySql                                  |
   |                                                               |    [--application-lang SQL]                           |
   |                                                               |    [--input-folder <input-script-path>]               |
   |                                                               |    [--output-folder <output-script-path>]             |
   |                                                               |    [--log-folder <log-path>]                          |
   |                                                               |    [--conversion-type <conversion-Type-BulkOrBlogic>] |
   |                                                               |    [--target-db/-T]                                   |
   |                                                               |                                                       |
   |                                                               | .. code-block::                                       |
   |                                                               |                                                       |
   |                                                               |    > runDSC.bat                                       |
   |                                                               |    --source-db MySql                                  |
   |                                                               |    [--application-lang SQL]                           |
   |                                                               |    [--input-folder <input-script-path>]               |
   |                                                               |    [--output-folder <output-script-path>]             |
   |                                                               |    [--log-folder <log-path>]                          |
   |                                                               |    [--conversion-type <conversion-Type-BulkOrBlogic>] |
   |                                                               |    [--target-db/-T]                                   |
   +---------------------------------------------------------------+-------------------------------------------------------+

.. note::

   -  The CLI parameters are described as follows:

      -  **source-db** specifies the source database. The value can be **Teradata**, which is case-insensitive.

      -  **conversion-type** specifies the migration type. This parameter is optional. DSC supports the following migration types:

         **Bulk**: migrates DML and DDL scripts.

         **BLogic**: migrates service logic, such as stored procedures and functions.

      -  **target-db** specifies the target database. The parameter value is **GaussDB A**.

   -  Command output description:

      **Migration process start time** indicates the migration start time and **Migration process end time** indicates the migration end time. **Total process time** indicates the total migration duration, in milliseconds. In addition, the total number of migrated files, total number of processors, number of used processors, log file path, and error log file path are also displayed on the console.

Examples
--------

-  Example 1: Run the following command to migrate the SQL file of the Oracle database to the SQL script of GaussDB on Linux:

   .. code-block::

      ./runDSC.sh --source-db Teradata --input-folder D:\test\conversion\input --output-folder D:\test\conversion\output --log-folder D:\test\conversion\log --conversion-type ddl --targetdb gaussdbA

-  Example 2: Run the following command to migrate the SQL file of the Oracle database to the SQL script of GaussDB on Windows:

   .. code-block::

      runDSC.bat --source-db Teradata --input-folder D:\test\conversion\input --output-folder D:\test\conversion\output --log-folder D:\test\conversion\log --conversion-type ddl --targetdb gaussdbA

Migration details are displayed on the console (including the progress and completion status):

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
