:original_name: mt_tool_index.html

.. _mt_tool_index:

Overview
========

After switching to GaussDB(DWS) databases, you may need to migrate user data and application SQL scripts to new databases. In particular,the migration of application SQL scripts is complex, risky, and time-consuming.

Database Schema Convertor (DSC) is a CLI tool running on Linux or Windows. It is designed to provide simple, fast, and reliable migration of application SQL scripts. DSC parses the SQL scripts of source database applications using its syntax migration logic and converts the scripts to the ones applicable to GaussDB(DWS) databases.

DSC does not require a connection to databases, and performs migration offline. The tool also displays the status of a migration process and logs the errors that occur during the process, helping quickly locate faults.

Migration Objects
-----------------

DSC can migrate the following objects of Teradata, Oracle, Netezza, MySQL, and DB2:

-  Common objects supported by Teradata, Oracle, Netezza, MySQL, and DB2: SQL schemas and SQL queries
-  Objects supported only by Oracle and Netezza: PL/SQL objects
-  Objects supported only by Teradata: Perl files containing **BTEQ** and **SQL_LANG** scripts

Migration Process
-----------------

The process of using DSC to migrate SQL scripts is as follows:

#. Export the SQL scripts from a Teradata or Oracle database to the Linux or Windows server installed with DSC.
#. Use DSC to migrate syntax. Specify the paths of the input files, output files, and logs in the command.
#. DSC automatically archives the migrated SQL scripts and the logs into the specified paths.


.. figure:: /_static/images/en-us_image_0000001234042287.png
   :alt: **Figure 1** Migration process using DSC

   **Figure 1** Migration process using DSC
