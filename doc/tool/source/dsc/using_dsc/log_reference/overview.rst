:original_name: dws_16_0217.html

.. _dws_16_0217:

Overview
========

The log files are the repository for all operations and status of the DSC. The following log files are available:

-  **SQL Migration Logs**

   #. *DSC.log*: SQL Migration all activities.
   #. *DSCError.log*: SQL Migration errors.
   #. *successRead.log*: SQL Migration successful input file reads.
   #. *successWrite.log*: SQL Migration successful output file writes.

-  **Perl Migration** **Logs**

   #. *perlDSC.log*: Perl Migration all activities, warnings and errors.

**Apache Log4j** is used for the DSC logging framework. The following Log4j configuration files are used and can be customized as required:

-  Teradata/Oracle/Netezza/DB2 : config/log4j2.xml
-  MySQL : config/log4j2_mysql.xml
