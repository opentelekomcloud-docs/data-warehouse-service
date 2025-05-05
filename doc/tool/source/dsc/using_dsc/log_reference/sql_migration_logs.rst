:original_name: dws_16_0033.html

.. _dws_16_0033:

.. _en-us_topic_0000001813598760:

SQL Migration Logs
==================

The SQL DSC (DSC.jar) supports the following types of logging:

-  Activity logging
-  Error logging
-  successRead
-  successWrite

.. note::

   -  If a user specifies a log path, then all the logs are saved in the specified log path.
   -  If a user does not specify the logging path, then the tool creates the **log** folder in the TOOL_HOME path and saves all the logs in this *log* folder.
   -  To control the disk space usage, the maximum size of a log file is 10 MB. You can have a maximum of 10 log files.
   -  The tool does not log sensitive data such as queries.

Activity Logging
----------------

DSC saves all log and error information to **DSC.log**. This file is available in the **log** folder. The **DSC.log** file consists of details such as user who executed the migration and files that have been migrated along with the timestamp. The logging level for activity logging is INFO.

The file structure of the **DSC.log** file is as follows:

.. code-block::

   2020-01-22 09:35:10,769 INFO CLMigrationUtility:159 DSC is initiated by xxxxx
   2020-01-22 09:35:10,828 INFO CLMigrationUtility:456 Successfully changed permission of files in D:\Migration\Gauss_Tools_18_Migration\code\migration\config
   2020-01-22 09:35:10,832 INFO PropertyLoader:90 Successfully loaded Property file : D:\Migration\Gauss_Tools_18_Migration\code\migration\config\application.properties
   2020-01-22 09:35:10,833 INFO ApplicationPropertyLoader:42 Application properties have been loaded Successfully
   2020-01-22 09:35:10,917 INFO MigrationValidatorService:549 Files in output directory has been overwritten as configured by xxxxx
   2020-01-22 09:35:10,920 INFO PropertyLoader:90 Successfully loaded Property file : D:\Migration\Gauss_Tools_18_Migration\code\migration\config\features-oracle.properties
   2020-01-22 09:35:10,921 INFO FeatureLoader:41 Features have been loaded Successfully
   2020-01-22 09:35:10,926 INFO MigrationService:80 DSC process start time : Wed Jan 22 09:35:10 GMT+05:30 2020
   2020-01-22 09:35:10,933 INFO FileHandler:179  File is not supported. D:\Migration_Output\Source\ARRYTYPE.sql-
   2020-01-22 09:35:10,934 INFO FileHandler:179  File is not supported. D:\Migration_Output\Source\varray.sql-
   2020-01-22 09:35:12,816 INFO PropertyLoader:90 Successfully loaded Property file : D:\Migration\Gauss_Tools_18_Migration\code\migration\config\global-temp-tables.properties
   2020-01-22 09:35:12,830 INFO PropertyLoader:90 Successfully loaded Property file : D:\Migration\Gauss_Tools_18_Migration\code\migration\config\create-types-UDT.properties
   2020-01-22 09:35:12,834 INFO PropertyLoader:90 Successfully loaded Property file : D:\Migration\Gauss_Tools_18_Migration\code\migration\config\package-names-oracle.properties
   2020-01-22 09:35:12,849 INFO DBMigrationService:76 Number of Available Processors: 4
   2020-01-22 09:35:12,850 INFO DBMigrationService:78 Configured simultaneous processes in the Tool : 3
   2020-01-22 09:35:13,032 INFO MigrationProcessor:94 File name: D:\Migration_Output\Source\Input.sql is started
   2020-01-22 09:35:13,270 INFO FileHandler:606 guessencoding command output = Error: Unable to access jarfile D:\Migration\Gauss_Tools_18_Migration\code\migration\RE_migration\target\dsctool.jar , for file= D:\Migration_Output\Source\Input.sql
   2020-01-22 09:35:13,272 INFO FileHandler:625 couldn't get the encoding format, so using the default charset for D:\Migration_Output\Source\Input.sql
   2020-01-22 09:35:13,272 INFO FileHandler:310 File D:\Migration_Output\Source\Input.sql will be read with charset : UTF-8
   2020-01-22 09:35:13,390 INFO FileHandler:668 D:\Migration_Output\target\output\Input.sql - File already exists/Failed to create target file
   2020-01-22 09:35:13,562 INFO FileHandler:606 guessencoding command output = Error: Unable to access jarfile D:\Migration\Gauss_Tools_18_Migration\code\migration\RE_migration\target\dsctool.jar , for file= D:\Migration_Output\Source\Input.sql
   2020-01-22 09:35:13,563 INFO FileHandler:625 couldn't get the encoding format, so using the default charset for D:\Migration_Output\Source\Input.sql
   2020-01-22 09:35:13,563 INFO FileHandler:675 File D:\Migration_Output\Source\Input.sql will be written with charset : UTF-8
   2020-01-22 09:35:13,604 INFO MigrationProcessor:139 File name: D:\Migration_Output\Source\Input.sql is processed successfully
   2020-01-22 09:35:13,605 INFO MigrationService:147 Total number of files in Input folder : 3
   2020-01-22 09:35:13,605 INFO MigrationService:148 Total number of queries : 1
   22020-01-22 09:35:13,607 INFO PropertyLoader:164 Successfully updated Property file : D:\Migration\Gauss_Tools_18_Migration\code\migration\config\global-temp-tables.properties
   2020-01-22 09:35:13,630 INFO PropertyLoader:164 Successfully updated Property file : D:\Migration\Gauss_Tools_18_Migration\code\migration\config\create-types-UDT.properties
   2020-01-22 09:35:13,631 INFO PropertyLoader:164 Successfully updated Property file : D:\Migration\Gauss_Tools_18_Migration\code\migration\config\package-names-oracle.properties
   2020-01-22 09:35:13,632 INFO CLMigrationUtility:305 Log file : dsc.log and the file is present in the path : D:\Migration_Output\log
   2020-01-22 09:35:13,632 INFO CLMigrationUtility:312 DSC process end time : Wed Jan 22 09:35:13 GMT+05:30 2020
   2020-01-22 09:35:13,632 INFO CLMigrationUtility:217 Total process time : 2842 seconds

Error Logging
-------------

DSC logs only the errors that are encountered during the migration process to **DSCError.log**. This file is available in the **log** folder. The **DSCError.log** file consists of details such as date and time of the error and the details of the file (file name) along with the query position. The logging level for error logging is ERROR.

The file structure of the **DSCError.log** file is as follows:

.. code-block::

   2017-06-29 14:07:39,585 ERROR TeradataBulkHandler:172 Error occurred during processing of input in Bulk Migration. PreQueryValidation failed in not proper termination or exclude keyword. /home/testmigration/Documentation/Input/c005.sql for Query in position : 4
   2017-06-29 14:07:39,962 ERROR TeradataBulkHandler:172 Error occurred during processing of input in Bulk Migration. PreQueryValidation failed in not proper termination or exclude keyword. /home/testmigration/Documentation/Input/c013.sql for Query in position : 11
   2017-06-29 14:07:40,136 ERROR QueryConversionUtility:250 Query is not converted as it contains unsupported keyword: join select
   2017-06-29 14:07:40,136 ERROR TeradataBulkHandler:172 Error occurred during processing of input in Bulk Migration. PreQueryValidation failed in not proper termination or exclude keyword. /home/testmigration/Documentation/Input/sample.sql for Query in position : 1
   2017-06-29 14:07:40,136 ERROR TeradataBulkHandler:172 Error occurred during processing of input in Bulk Migration. PreQueryValidation failed in not proper termination or exclude keyword. /home/testmigration/Documentation/Input/sample.sql for Query in position : 3

successRead
-----------

After a file has been read by the DSC, the file is logged for tracking purposes. In certain scenarios, these logs let the user know the status of the execution of files. This file is available in the **log** folder. The log file consists of details such as date and time, and the details of the file name. The logging level for this log file is INFO.

The file structure of the **successRead.log** file is as follows:

.. code-block::

   2017-07-21 14:13:00,461 INFO readlogger:213 /home/testmigration/Documentation/is not in.sql is read successfully.
   2017-07-21 14:13:00,957 INFO readlogger:213 /home/testmigration/Documentation/date quotes.sql is read successfully.
   2017-07-21 14:13:01,509 INFO readlogger:213 /home/testmigration/Documentation/column alias replace.sql is read successfully.
   2017-07-21 14:13:02,034 INFO readlogger:213 /home/testmigration/Documentation/sampleRownum.sql is read successfully.
   2017-07-21 14:13:02,578 INFO readlogger:213 /home/testmigration/Documentation/samp.sql is read successfully.
   2017-07-21 14:13:03,145 INFO readlogger:213 /home/testmigration/Documentation/2.6BuildInputs/testWithNodataSamples.sql is read successfully.

successWrite
------------

DSC reads a file, processes it, and writes the output to the disk. This is logged to the success write log file. In some scenarios, this log lets the user know which of the files are successfully processed. In case of a re-run, the user can skip these files and run the remaining files. This file is available in the **log** folder. The log file consists of details such as date and time, and the details of the file name. The logging level for this log file is INFO.

The file structure of the **successWrite.log** file is as follows:

.. code-block::

   2017-07-21 14:13:00,616 INFO writelogger:595 /home/testmigration/Documentation/is not in.sql has written successfully.
   2017-07-21 14:13:01,055 INFO writelogger:595 /home/testmigration/Documentation/date quotes.sql has written successfully.
   2017-07-21 14:13:01,569 INFO writelogger:595 /home/testmigration/Documentation/column alias replace.sql has written successfully.
   2017-07-21 14:13:02,055 INFO writelogger:595 /home/testmigration/Documentation/sampleRownum.sql has written successfully.
   2017-07-21 14:13:02,597 INFO writelogger:595 /home/testmigration/Documentation/samp.sql has written successfully.
   2017-07-21 14:13:03,178 INFO writelogger:595 /home/testmigration/Documentation/testWithNodataSamples.sql has written successfully.
