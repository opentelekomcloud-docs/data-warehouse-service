:original_name: dws_mt_0184.html

.. _dws_mt_0184:

Database Schema Conversion
==========================

Function
--------

**runDSC.sh** or **runDSC.bat** is used to migrate schemas and queries of Teradata, Oracle, Netezza, MySQL, and DB2 to GaussDB(DWS).

Format
------

**Linux**:

.. code-block::

   ./runDSC.sh
   --source-db<source-database>
   [--input-folder<input-script-path>]
   [--output-folder<output-script-path>]
   [-application-lang <application-lang>]
   [--conversion-type<conversion-type>]
   [--log-folder<log-path>]
   [--version-number <Gauss Kernel Version>]
   [--target-db<target-database>

**Windows**:

.. code-block::

   runDSC.bat
   --source-db<source-database>
   [--input-folder<input-script-path>]
   [--output-folder<output-script-path>]
   [-application-lang <application-lang>]
   [--conversion-type<conversion-type>]
   [--log-folder<log-path>]
   [--version-number <Gauss Kernel Version>]
   [--target-db<target-database>

Parameter Description
---------------------

.. table:: **Table 1** Parameters

   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | Long               | Short   | Data Type | Description                                                          | Value Range                                                                                           | Default Value | Example                                                                                                       |
   +====================+=========+===========+======================================================================+=======================================================================================================+===============+===============================================================================================================+
   | --source-db        | -S      | String    | Source database                                                      | -  Oracle                                                                                             | N/A           | --source-db *Oracle*\ (or)                                                                                    |
   |                    |         |           |                                                                      | -  Teradata                                                                                           |               |                                                                                                               |
   |                    |         |           |                                                                      | -  Netezza                                                                                            |               | -S *Oracle*                                                                                                   |
   |                    |         |           |                                                                      | -  MySQL                                                                                              |               |                                                                                                               |
   |                    |         |           |                                                                      | -  DB2                                                                                                |               |                                                                                                               |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --input-folder     | -I      | String    | Input folder containing Teradata or Oracle scripts                   | N/A                                                                                                   | N/A           | --input-folder */home/testmigration/Documentation/input*                                                      |
   |                    |         |           |                                                                      |                                                                                                       |               |                                                                                                               |
   |                    |         |           |                                                                      |                                                                                                       |               | (or)                                                                                                          |
   |                    |         |           |                                                                      |                                                                                                       |               |                                                                                                               |
   |                    |         |           |                                                                      |                                                                                                       |               | -I */home/testmigration/Documentation/input*                                                                  |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --output-folder    | -O      | String    | Output folder where the migrated scripts are saved                   | N/A                                                                                                   | N/A           | --output-folder */home/testmigration/Documentation/output*\ (or)-O */home/testmigration/Documentation/output* |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --application-lang | -A      | String    | Application language parser used for migration                       | -  SQL                                                                                                | SQL           | --application-lang Perl                                                                                       |
   |                    |         |           |                                                                      | -  Perl                                                                                               |               |                                                                                                               |
   |                    |         |           | **SQL**: Migrate SQL schemas or scripts in SQL files.                |                                                                                                       |               | or                                                                                                            |
   |                    |         |           |                                                                      |                                                                                                       |               |                                                                                                               |
   |                    |         |           | **Perl**: Migrate **BTEQ** or **SQL_LANG** scripts in Perl files.    |                                                                                                       |               | -A *Perl*                                                                                                     |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --conversion-type  | -M      | String    | Migration type. Set this parameter based on input scripts.           | -  .. _en-us_topic_0000001188362520__en-us_topic_0219651208_en-us_topic_0213040018_li57800760204849:  | Bulk          | --conversion-type *bulk*                                                                                      |
   |                    |         |           |                                                                      |                                                                                                       |               |                                                                                                               |
   |                    |         |           | **Bulk**: Migrate DML and DDL scripts.                               |    Bulk                                                                                               |               | or                                                                                                            |
   |                    |         |           |                                                                      |                                                                                                       |               |                                                                                                               |
   |                    |         |           | **BLogic**: Migrate service logic, such as procedures and functions. | -  .. _en-us_topic_0000001188362520__en-us_topic_0219651208_en-us_topic_0213040018_li6200471420490:   |               | -M *bulk*                                                                                                     |
   |                    |         |           |                                                                      |                                                                                                       |               |                                                                                                               |
   |                    |         |           | **BLogic** is used only for Oracle PL/SQL.                           |    BLogic                                                                                             |               |                                                                                                               |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --log-folder       | -L      | String    | Log file path                                                        | N/A                                                                                                   | N/A           | --log-folder */home/testmigration/Documentation*\ (or)-L */home/testmigration/Documentation*                  |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --version-number   | -VN     | String    | Oracle specified parameter                                           | Oracle                                                                                                | N/A           | --version-number                                                                                              |
   |                    |         |           |                                                                      |                                                                                                       |               |                                                                                                               |
   |                    |         |           |                                                                      |                                                                                                       |               | or                                                                                                            |
   |                    |         |           |                                                                      |                                                                                                       |               |                                                                                                               |
   |                    |         |           |                                                                      |                                                                                                       |               | -V1R8_330                                                                                                     |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --target-db        | -T      | String    | Target database                                                      | -  gaussdbT                                                                                           | gaussdbA      | --target-db gaussdbT (or)                                                                                     |
   |                    |         |           |                                                                      | -  gaussdbA                                                                                           |               |                                                                                                               |
   |                    |         |           |                                                                      |                                                                                                       |               | *-T gaussdbT*                                                                                                 |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------+---------------+---------------------------------------------------------------------------------------------------------------+

Usage Guidelines
----------------

It is mandatory to specify the source database, input folder path, and output folder path, and optional to specify the migration type and log path.

.. note::

   If no log path is specified, DSC creates the **log** folder under **TOOL_HOME** to store logs.

Example
-------

.. code-block::

   ./runDSC.sh --source-db Oracle --input-folder opt/DSC/DSC/input/oracle/ --output-folder /opt/DSC/DSC/output/ --log-folder /opt/DSC/DSC/log/ --application-lang SQL --conversion-type bulk --target-db gaussdbT

System Response
---------------

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

.. note::

   If there is no sql file present in the input folder, then the following message is displayed in console:

   |image1|

Environment Creation and Restoration Procedure (database and database user)
---------------------------------------------------------------------------

**GaussDB(DWS): Database Creation and Schema Setup**

#. Log into postgres:

   .. code-block::

      gsql -p <port> -d postgres
      drop database <database name>;
      create database <database name>;
      \c <database name>
      GRANT ALL PRIVILEGES ON DATABASE <database name> TO <user>;
      grant database to <user>;\q
      gsql -p <port> -d <database name> -U <user> -W <password> -h <IP> -f
      drop database <database name>;
      create database <database name>;
      \c <database name>;
      GRANT ALL PRIVILEGES ON DATABASE <database name> TO <user>;
      gsql -p <port> -d <database name> -U <user> -W <password>  -f

#. Run all files in setup.

**Commands**:

.. code-block::

   sh runDSC.sh -S oracle -M blogic -I <input path>
   sh runDSC.sh -S oracle -M bulk -I <input path>

**Configuration Details**

#. Set the value of **GaussDBSQLExec** to **True**, and update the **gaussdb.properties** file.
#. Create a user (T) and a database (A). Add all schemas.

Verification After Migration
----------------------------

After DSC converts the source sql files, execute the converted files on target gaussdb and provide a report with details of number of statements succeeded and failed.

After the DSC finishes the translation, it will invoke (controlled through a configuration item) post migration verification script. The verification script (for details about the configuration, see the configuration file) is connected to the target GaussDB database and executed.

The post migration verification script will connect to the target gauss database (details are configured in a configuration file) and executes the scripts.

#. **application.properties** in config folder

   Execute migrated script on Gauss DB: true/false, default value = false

   executesqlingauss=true

   true: It will execute the migrated script on gaussdb

#. **gaussdb.properties** in config folder

   #Target Database configurations

   .. code-block::

      #gauss database user with all privileges
       gaussdb-user=
       gaussdb-port=
       #Database name for GaussDBA
       gaussdb-name=
       #gaussdb ip
       gaussdb-ip=

   **Dependency between gsql and zsql clients**

   a. gsql (GaussDB(DWS)) is required for executing scripts on GaussDB(DWS). Therefore, to ensure the smooth running of DSC, DSC is required to run on a node installed with a GaussDB(DWS) instance or client (gsql), and the user that performs verification must have the permission for executing commands using gsql or zsql.

   b. Since the Gauss DB Instance/Client can be installed on a linux OS only, this can be used to verify functionality only on a linux environment.

   c. To execute the gsql command on a remote GaussDB instance, it is advised to add the client system IP/hostname in the following configuration file of Gauss DB instance.

      .. code-block::

         /home/gsmig/database/coordinator
         ---pg_hba.conf

**Response**

GaussDB(DWS)

.. code-block::

   ********************** Verification Started ******************************
   Sql script execution on Gauss DB start time : Wed Jan 22 17:27:07 CST 2020
   Sql script execution on Gauss DB end time : Wed Jan 22 17:27:44 CST 2020

   Summary of Verification :
   ==================================================================================================================================
   Statement                | Total               | Passed              | Failed              | Success Rate(%)
   -----------------------------------------------------------------------------------------------------------------------------------
   COMMENT                  | 15                  | 15                  | 0                   | 100
   CREATE VIEW              | 4                   | 3                   | 1                   | 75
   CREATE INDEX             | 4                   | 3                   | 1                   | 75
   CREATE TABLE             | 6                   | 6                   | 0                   | 100
   ALTER TABLE              | 3                   | 3                   | 0                   | 100
   ---------------------------------------------------------------------------------------------------------------------------------
   Total                    | 32                  | 30                  | 2                   | 93

   Gauss Execution Log file : /home/gsmig/18Jan/DSC/DSC/log/gaussexecutionlog.log
   Gauss Execution Error Log file : /home/gsmig/18Jan/DSC/DSC/log/gaussexecutionerror.log
   Verification finished in 38 seconds

   ********************** Verification Completed ****************************

.. |image1| image:: /_static/images/en-us_image_0000001188362656.png
