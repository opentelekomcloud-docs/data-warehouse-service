:original_name: dws_mt_0039.html

.. _dws_mt_0039:

Oracle SQL Migration
====================

DSC supports the migration from Oracle to GaussDB(DWS), including the migration of schemas, DML, queries, system functions, and PL/SQL.

Performing Oracle SQL Migration
-------------------------------

Run the following commands to set the source database, input and output folder paths, log paths, application language, and conversion type:

**Linux**:

.. code-block::

   ./runDSC.sh
   --source-db Oracle
   [--input-folder <input-script-path>]
   [--output-folder <output-script-path>]
   [--log-folder <log-path>]
   [--application-lang Oracle]
   [--conversion-type <conversion-type>]

**Windows**:

.. code-block::

   runDSC.bat
   --source-db Oracle
   [--input-folder <input-script-path>]
   [--output-folder <output-script-path>]
   [--log-folder <log-path>]
   [--application-lang Oracle]
   [--conversion-type <conversion-type>]

-  When migrating common DDL statements (such as tables, views, indexes, sequences, and so on) that do not contain PL/SQL statements, use the Bulk mode (that is, set **conversion-type** to **Bulk**).

   Run the following commands, which include the folder paths in the example and the **conversion-type** set to :ref:`Bulk <en-us_topic_0000001188362520__en-us_topic_0219651208_en-us_topic_0213040018_li57800760204849>`:

   **Linux**:

   .. code-block::

      ./runDSC.sh --source-db Oracle --input-folder /opt/DSC/DSC/input/oracle/ --output-folder /opt/DSC/DSC/output/ --log-folder /opt/DSC/DSC/log/ --application-lang SQL --conversion-type bulk --target-db gaussdbA

   **Windows**:

   .. code-block::

      runDSC.bat --source-db Oracle --input-folder D:\test\conversion\input --output-folder D:\test\conversion\output --log-folder D:\test\conversion\log --application-lang SQL --conversion-type bulk --target-db gaussdbA

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

-  When migrating objects such as functions, procedures, and packages that contain PL/SQL statements, use the BLogic mode. That is, set **conversion-type** to **BLogic**.

   Run the following command, which includes the folder paths in the example and the **conversion-type** set to :ref:`BLogic <en-us_topic_0000001188362520__en-us_topic_0219651208_en-us_topic_0213040018_li6200471420490>`:

   .. code-block::

      runDSC.bat --source-db Oracle --input-folder D:\test\conversion\input --output-folder D:\test\conversion\output --log-folder D:\test\conversion\log --application-lang SQL --conversion-type blogic --target-db gaussdbA

   During the execution of DSC, the migration summary, including the progress and completion status, is displayed on the console. Execution information and errors are written into :ref:`log files <dws_mt_0189>`.

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

   Common DDL scripts and PL/SQL scripts should be stored in different input folders for migration.

Oracle PACKAGE Migration Precautions
------------------------------------

1. The package specifications (that is, the package header) and the package body should be stored in different files and in the same input path for migration.

2. You need to migrate common DDL statements in Bulk mode, including all table structure information referenced in the **PACKAGE** script, to form a dictionary in the **config/create-types-UDT.properties** file. Then, migrate the package specifications (that is, the package header) and the package body in Blogic mode. The details are as follows:

When some Oracle **PACKAGE** packages define package specifications, the **tbName.colName%TYPE** syntax is used to declare custom record types based on other table objects.

::

   For example:
       CREATE OR REPLACE PACKAGE p_emp
       AS
           --Define the RECORD type
           TYPE re_emp IS RECORD(
               rno emp.empno%TYPE,
               rname emp.empname%TYPE
           );

       END;

The column data type cannot be specified using the **tbName.colName%TYPE** syntax in a **CREATE TYPE** statement on GaussDB(DWS). Therefore, DSC needs to build a database context environment containing the **EMP** table information during migration. In this case, you need to use DSC to migrate all table creation scripts, that is, to migrate common DDL statements in Bulk mode. DSC automatically generates corresponding data dictionaries. After the context environment containing various table information is built, you can migrate the Oracle PACKAGE in Blogic mode. In this case, the **re_emp** record type is migrated based on the column type of the **EMP** table.

::

   Expected output
       CREATE TYPE p_emp.re_emp AS (
           rno NUMBER(4),
           rname VARCHAR2(10)
       );

For details about how to migrate Oracle SQL using DSC, see :ref:`Migrating Data Using DSC <dws_mt_0035>`.
