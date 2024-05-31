:original_name: dws_16_0025.html

.. _dws_16_0025:

Migration Parameters
====================

Parameter Description
---------------------

.. table:: **Table 1** Parameters

   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | Long               | Short   | Data Type | Description                                                          | Value Range | Default Value | Example                                                                                                       |
   +====================+=========+===========+======================================================================+=============+===============+===============================================================================================================+
   | --source-db        | -S      | String    | Source database                                                      | -  Teradata | N/A           | --source-db Teradata(or)                                                                                      |
   |                    |         |           |                                                                      | -  MySQL    |               |                                                                                                               |
   |                    |         |           |                                                                      |             |               | -S Teradata                                                                                                   |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --input-folder     | -I      | String    | Input folder containing Teradata or Oracle scripts                   | N/A         | N/A           | --input-folder */home/testmigration/Documentation/input*                                                      |
   |                    |         |           |                                                                      |             |               |                                                                                                               |
   |                    |         |           |                                                                      |             |               | (or)                                                                                                          |
   |                    |         |           |                                                                      |             |               |                                                                                                               |
   |                    |         |           |                                                                      |             |               | -I */home/testmigration/Documentation/input*                                                                  |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --output-folder    | -O      | String    | Output folder where the migrated scripts are saved                   | N/A         | N/A           | --output-folder */home/testmigration/Documentation/output*\ (or)-O */home/testmigration/Documentation/output* |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --application-lang | -A      | String    | Application language parser used for migration                       | -  SQL      | SQL           | --application-lang Perl                                                                                       |
   |                    |         |           |                                                                      | -  Perl     |               |                                                                                                               |
   |                    |         |           | **SQL**: Migrate SQL schemas or scripts in SQL files.                |             |               | or                                                                                                            |
   |                    |         |           |                                                                      |             |               |                                                                                                               |
   |                    |         |           | **Perl**: Migrate **BTEQ** or **SQL_LANG** scripts in Perl files.    |             |               | -A *Perl*                                                                                                     |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --conversion-type  | -M      | String    | Migration type. Set this parameter based on input scripts.           | -  Bulk     | Bulk          | --conversion-type *ddl*                                                                                       |
   |                    |         |           |                                                                      | -  BLogic   |               |                                                                                                               |
   |                    |         |           | **Bulk**: Migrate DML and DDL scripts.                               |             |               | or                                                                                                            |
   |                    |         |           |                                                                      |             |               |                                                                                                               |
   |                    |         |           | **BLogic**: Migrate service logic, such as procedures and functions. |             |               | -M *ddl*                                                                                                      |
   |                    |         |           |                                                                      |             |               |                                                                                                               |
   |                    |         |           | **BLogic** is used only for Oracle PL/SQL.                           |             |               |                                                                                                               |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --log-folder       | -L      | String    | Log file Path                                                        | N/A         | N/A           | --log-folder */home/testmigration/Documentation*\ (or)-L */home/testmigration/Documentation*                  |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --version-number   | -VN     | String    | Oracle specified parameter                                           | Oracle      | N/A           | --version-number                                                                                              |
   |                    |         |           |                                                                      |             |               |                                                                                                               |
   |                    |         |           |                                                                      |             |               | or                                                                                                            |
   |                    |         |           |                                                                      |             |               |                                                                                                               |
   |                    |         |           |                                                                      |             |               | -V1R8_330                                                                                                     |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------+---------------+---------------------------------------------------------------------------------------------------------------+
   | --target-db        | -T      | String    | Target database                                                      | -  gaussdbA | gaussdbA      | --target-db gaussdbA (or)                                                                                     |
   |                    |         |           |                                                                      |             |               |                                                                                                               |
   |                    |         |           |                                                                      |             |               | *-T gaussdbA*                                                                                                 |
   +--------------------+---------+-----------+----------------------------------------------------------------------+-------------+---------------+---------------------------------------------------------------------------------------------------------------+

Usage Guideline
---------------

It is mandatory to specify the source database, input folder path, and output folder path, and optional to specify the migration type and log path.

.. note::

   If a user does not specify the logging path, then the tool creates the **log** folder in the TOOL_HOME path and saves all the logs in this *log* folder.

Examples
--------

.. code-block::

   ./runDSC.sh --source-db Teradata --input-folder opt/DSC/DSC/input/oracle/ --output-folder /opt/DSC/DSC/output/ --log-folder /opt/DSC/DSC/log/ --application-lang SQL --conversion-type ddl --targetdb gaussdbA

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

**Creating a** **GaussDB(DWS) -** **Database and Schema**

#. Log in to postgres:

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
   sh runDSC.sh -I input/ -S oracle -M ddl -L log_temp -P input/bulk/1_table/

.. |image1| image:: /_static/images/en-us_image_0000001387842380.png
