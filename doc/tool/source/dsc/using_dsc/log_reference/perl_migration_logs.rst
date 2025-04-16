:original_name: dws_16_0034.html

.. _dws_16_0034:

Perl Migration Logs
===================

The DSC writes all log information to a single file, **perlDSC.log**.

.. note::

   Since the DSC will execute the SQL to migrate the SQL scripts inside Perl files, the following :ref:`SQL migration logs <en-us_topic_0000001813598760>` are also supported:

   -  Activity Logging
   -  Error Logging
   -  successRead
   -  successWrite

Logging Levels
--------------

The logging level for Perl Migration logs is configured using the **logging-level** parameter.

Logging
-------

DSC saves all logs, warnings and error information to **perlDSC.log**. This file is available in the **log** folder. The log file consists of details such as user who executed the migration and files that have been migrated along with the timestamp.

The structure of the **perlDSC.log** file is as follows:

.. code-block::

   2018-07-08 13:35:10 INFO teradatacore.pm:1316 Extracting SQL contents from perl files started
   2018-07-08 13:35:10 INFO teradatacore.pm:1329 Extracting SQL contents from perl files completed
   2018-07-08 13:35:10 INFO teradatacore.pm:1331 Migrating SQL files
   2018-07-08 13:35:12 INFO teradatacore.pm:1348 Migrating SQL files completed
   2018-07-08 13:35:12 INFO teradatacore.pm:1349 Merging migrated SQL contents to perl files started
   2018-07-08 13:35:12 INFO teradatacore.pm:1362 Merging migrated SQL contents to perl files completed
   2018-07-08 13:35:12 INFO teradatacore.pm:1364 Perl file migration completed
   2018-07-08 13:35:32 INFO teradatacore.pm:1316 Extracting SQL contents from perl files started
   2018-07-08 13:35:58 ERROR teradatacore.pm:426 opendir ../../../../../perltest/ failed
   2018-07-08 13:36:17 INFO teradatacore.pm:1316 Extracting SQL contents from perl files started
   2018-07-08 13:38:21 INFO teradatacore.pm:1329 Extracting SQL contents from perl files completed
   2018-07-08 13:38:21 INFO teradatacore.pm:1331 Migrating SQL files
   2018-07-08 13:38:22 INFO teradatacore.pm:1348 Migrating SQL files completed
   2018-07-08 13:38:22 INFO teradatacore.pm:1349 Merging migrated SQL contents to perl files started
   2018-07-08 13:38:37 ERROR teradatacore.pm:1044 Directory ../../../../../perltest/ should have 700, but has   0 permission
   2018-07-08 13:38:53 ERROR teradatacore.pm:1241 Another migration process is running on same folder, re-execute after the process has completed
   2018-07-08 13:39:01 INFO teradatacore.pm:1316 Extracting SQL contents from perl files started
   2018-07-08 13:39:51 INFO teradatacore.pm:1329 Extracting SQL contents from perl files completed
   2018-07-08 13:39:51 INFO teradatacore.pm:1331 Migrating SQL files
   2018-07-08 13:39:53 INFO teradatacore.pm:1348 Migrating SQL files completed
   2018-07-08 13:39:54 INFO teradatacore.pm:1349 Merging migrated SQL contents to perl files started
   2018-07-08 13:39:55 INFO teradatacore.pm:1362 Merging migrated SQL contents to perl files completed
   2018-07-08 13:39:57 INFO teradatacore.pm:1364 Perl file migration completed
