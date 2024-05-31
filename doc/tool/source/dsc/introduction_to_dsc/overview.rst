:original_name: mt_tool_index.html

.. _mt_tool_index:

Overview
========

After switching to GaussDB(DWS) databases, you may need to migrate user data and application SQL scripts to new databases. In particular,the migration of application SQL scripts is complex, risky, and time-consuming.

DSC is a CLI tool running on Linux or Windows. It is dedicated to providing users with simple, fast, and reliable application SQL script migration services. It parses the SQL scripts of source database applications using the built-in syntax migration logic, and converts them to SQL scripts applicable to GaussDB(DWS) databases.

DSC does not require a connection to databases, and performs migration offline. The tool also displays the status of a migration process and logs the errors that occur during the process, helping quickly locate faults.

Migration Objects
-----------------

DSC can migrate the following Teradata and MySQL database objects:

-  Common objects supported by Teradata and MySQL: SQL schemas and SQL queries
-  Objects supported only by Teradata: Perl files containing **BTEQ** and **SQL_LANG** scripts

Migration Process
-----------------

The process of using DSC to migrate SQL scripts is as follows:

#. Export the SQL scripts from a Teradata or MySQL database to the Linux or Windows server installed with DSC.
#. Execute DSC to migrate the syntax. In the command, specify the paths of the input file, output file, and logs.
#. DSC automatically archives the migrated SQL scripts and the logs into the specified paths.


.. figure:: /_static/images/en-us_image_0000001657865966.png
   :alt: **Figure 1** DSC migration process

   **Figure 1** DSC migration process
