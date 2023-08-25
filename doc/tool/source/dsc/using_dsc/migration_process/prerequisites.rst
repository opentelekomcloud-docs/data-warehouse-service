:original_name: dws_mt_0033.html

.. _dws_mt_0033:

Prerequisites
=============

Executing Custom DB Scripts
---------------------------

Custom scripts are executed to support input keywords that do not exist in certain versions of the target database. These scripts must be executed in each target database before the migration.

:ref:`Table 1 <en-us_topic_0000001233922139__en-us_topic_0218440503_table101312310197>` describes the custom scripts in the **DSC/scripts/** directory. For details about how to execute custom scripts, see :ref:`Custom DB Script Configuration <en-us_topic_0000001233922139__en-us_topic_0218440503_section974418973610>`.

.. _en-us_topic_0000001233922139__en-us_topic_0218440503_table101312310197:

.. table:: **Table 1** Custom DB scripts

   +------------------------------------+--------------------------------------------------------+
   | Script                             | Description                                            |
   +====================================+========================================================+
   | date_functions.sql                 | Custom DB script for Oracle date functions             |
   +------------------------------------+--------------------------------------------------------+
   | environment_functions.sql          | Custom DB script for Oracle environment functions      |
   +------------------------------------+--------------------------------------------------------+
   | string_functions.sql               | Custom DB script for Oracle string functions.          |
   +------------------------------------+--------------------------------------------------------+
   | pkg_variable_scripts.sql           | Custom DB script for Oracle package variable functions |
   +------------------------------------+--------------------------------------------------------+
   | sequence_scripts.sql               | Custom DB script for Oracle sequence functions         |
   +------------------------------------+--------------------------------------------------------+
   | mig_fn_get_datatype_short_name.sql | Custom DB script for Teradata functions                |
   +------------------------------------+--------------------------------------------------------+
   | mig_fn_castasint.sql               | Custom DB script for migration of CAST AS INTEGER      |
   +------------------------------------+--------------------------------------------------------+
   | vw_td_dbc_tables.sql               | Custom DB script for migration of DBC.TABLES           |
   +------------------------------------+--------------------------------------------------------+
   | vw_td_dbc_indices.sql              | Custom DB script for migration of DBC.INDICES          |
   +------------------------------------+--------------------------------------------------------+

.. table:: **Table 2** Custom DB scripts (Oracle to GaussDB T)

   +--------------------------------------+-------------------------------------------------------------------------------------------------------+
   | Script                               | Description                                                                                           |
   +======================================+=======================================================================================================+
   | create_user_and_temptable_enable.sql | Custom DB script for create user and local temporary table is used to address Oracle Package feature. |
   +--------------------------------------+-------------------------------------------------------------------------------------------------------+
   | pkg_variable_scripts.sql             | Custom DB script for Oracle package variable functions                                                |
   +--------------------------------------+-------------------------------------------------------------------------------------------------------+

Follow the steps to execute custom DB scripts:

#. Use any of the following methods to execute the required scripts in all target databases for which migration is to be performed:

   -  Use **gsql**.

      -  Use **gsql** to connect to the target database and paste all content in the SQL file to **gsql**, which will automatically execute the pasted contents.

         Run the following command to connect to the database:

         .. code-block::

            gsql -h <host_addr_xxx.xxx.xxx.xxx> -d <database_name> -U <user_name> -W <password> -p <port_number> -r

      -  Use **gsql** to connect to the target database and execute a SQL file.

         Run the following command to connect to the database and run the SQL file:

         .. code-block::

            gsql -h <host_addr_xxx.xxx.xxx.xxx> -d <database_name> -U <user_name>  -W <password>  -p <port_number> -f <filename.sql> -o <output_filename> -L <log_filename.log>  -r

   -  Use Data Studio.

      Use Data Studio to connect to the target database, and then open and run the SQL file in Data Studio.

.. _en-us_topic_0000001233922139__en-us_topic_0218440503_section974418973610:

Custom DB Script Configuration
------------------------------

Custom scripts are SQL files used to migrate from Teradata/Oracle the input keywords that do not exist in the target database.

These scripts must be executed in each target database before the migration.

Open the **scripts** folder in the release package. :ref:`Table 3 <en-us_topic_0000001233922139__en-us_topic_0218440503_table177441591362>` lists the folders and files in the **scripts** folder.

The SQL files contain the scripts for the custom migration functions. These functions are required to support the specific features of Teradata and Oracle.

.. _en-us_topic_0000001233922139__en-us_topic_0218440503_table177441591362:

.. table:: **Table 3** Custom DB scripts for DSC

   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | Folder             | Script File                        | Description                                                                                    |
   +====================+====================================+================================================================================================+
   | -- scripts         | ``-``                              | Folder: all scripts                                                                            |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ------ oracle      | ``-``                              | Folder: Oracle functions and scripts                                                           |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | -------- sequence  | ``-``                              | Folder: scripts to configure Oracle sequences                                                  |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ``-``              | sequence_scripts.sql               | Script: used to enable migration of Oracle :ref:`sequence <dws_mt_0112>`                       |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | -------- package   | ``-``                              | Folder: scripts to configure Oracle package variables                                          |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ``-``              | pkg_variable_scripts.sql           | Script: used to enable migration of Oracle installation :ref:`package variables <dws_mt_0159>` |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | -------- function  | ``-``                              | Folder: scripts to configure Oracle system functions                                           |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ``-``              | date_functions.sql                 | Script: used to enable migration of Oracle date functions                                      |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ``-``              | environment_functions.sql          | Script: used to enable migration of Oracle environment functions                               |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ``-``              | string_functions.sql               | Script: used to enable migration of Oracle string functions                                    |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ------ teradata    | ``-``                              | Folder: Teradata functions and scripts                                                         |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | -------- view      | ``-``                              | Folder: scripts to configure views                                                             |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ``-``              | vw_td_dbc_tables.sql               | Script: used to enable migration of Teradata DBC.TABLES                                        |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ``-``              | vw_td_dbc_indices.sql              | Script: used to enable migration of Teradata DBC.INDICES                                       |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | -------- function  | ``-``                              | Folder: scripts to configure Teradata system functions                                         |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | -X                 | mig_fn_get_datatype_short_name.sql | Script: used to enable migration of Teradata DBC.COLUMNS                                       |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ``-``              | mig_fn_castasint.sql               | Script: used to enable migration of CAST AS INTEGER                                            |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | --------db_scripts | ``-``                              | Folder: scripts to enable Teradata custom functions                                            |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ``-``              | mig_fn_get_datatype_short_name.sql | Script: used to enable migration of Teradata DBC.COLUMNS                                       |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | --------core       | ``-``                              | Folder: Teradata core scripts                                                                  |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+
   | ``-``              | teradatacore.pm                    | Script: used to perform Perl migration                                                         |
   +--------------------+------------------------------------+------------------------------------------------------------------------------------------------+

Configuring DSC and Migration Properties
----------------------------------------

To configure DSC, configure parameters in the configuration files in the **config** folder of DSC. :ref:`Table 4 <en-us_topic_0000001233922139__en-us_topic_0218440503_table142173481672>` describes the parameters.

.. _en-us_topic_0000001233922139__en-us_topic_0218440503_table142173481672:

.. table:: **Table 4** Parameters for configuring DSC

   +----------------------------------------------+------------------------------------------------------------------------------------+----------------------------------------------------+
   | Scenario                                     | Configuration File                                                                 | Parameter                                          |
   +==============================================+====================================================================================+====================================================+
   | :ref:`Teradata SQL Migration <dws_mt_0038>`  | -  DSC: *application.properties*                                                   | .. code-block::                                    |
   |                                              | -  :ref:`Teradata SQL Configuration <dws_mt_0026>`: *features-teradata.properties* |                                                    |
   |                                              |                                                                                    |    deleteToTruncate=True/False                     |
   |                                              |                                                                                    |    distributeByHash=one/many                       |
   |                                              |                                                                                    |    extendedGroupByClause=True/False                |
   |                                              |                                                                                    |    inToExists=True/False                           |
   |                                              |                                                                                    |    rowstoreToColumnstore=True/False                |
   |                                              |                                                                                    |    session_mode=Teradata/ANSI                      |
   |                                              |                                                                                    |    tdMigrateDollar=True/False                      |
   |                                              |                                                                                    |    tdMigrateALIAS=True/False                       |
   |                                              |                                                                                    |    tdMigrateNULLIFZero=True/False                  |
   |                                              |                                                                                    |    tdMigrateZEROIFNULL=True/False                  |
   |                                              |                                                                                    |    volatile=local temporary/unlogged               |
   +----------------------------------------------+------------------------------------------------------------------------------------+----------------------------------------------------+
   | :ref:`Oracle SQL Migration <dws_mt_0039>`    | -  DSC: *application.properties*                                                   | .. code-block::                                    |
   |                                              |                                                                                    |                                                    |
   |                                              | -  :ref:`Oracle SQL Configuration <dws_mt_0027>`:                                  |    exceptionHandler=True/False                     |
   |                                              |                                                                                    |    TxHandler=True/False                            |
   |                                              |    *features-oracle.properties*                                                    |    foreignKeyHandler=True/False                    |
   |                                              |                                                                                    |    globalTempTable=GLOBAL/LOCAL                    |
   |                                              |                                                                                    |    onCommitDeleteRows=Delete/Preserve              |
   |                                              |                                                                                    |    maxValInSequence=0..9223372036854775807         |
   |                                              |                                                                                    |    mergeImplementation=WITH/SPLIT                  |
   |                                              |                                                                                    |    RemoveHashPartition=True/False                  |
   |                                              |                                                                                    |    RemoveHashSubPartition=True/False               |
   |                                              |                                                                                    |    RemoveListPartition=True/False                  |
   |                                              |                                                                                    |    RemoveListSubPartition=True/False               |
   |                                              |                                                                                    |    RemoveRangeSubPartition=True/False              |
   |                                              |                                                                                    |    MigSupportSequence=True/False                   |
   +----------------------------------------------+------------------------------------------------------------------------------------+----------------------------------------------------+
   | :ref:`Teradata Perl Migration <dws_mt_0041>` | -  DSC: *application.properties*                                                   | .. code-block::                                    |
   |                                              | -  :ref:`Teradata Perl Configuration <dws_mt_0029>`: *perl-migration.properties*   |                                                    |
   |                                              |                                                                                    |    add-timing-on=True/False                        |
   |                                              |                                                                                    |    db-bteq-tag-name=bteq                           |
   |                                              |                                                                                    |    db-tdsql-tag-name=sql_lang                      |
   |                                              |                                                                                    |    logging-level=error/warning/info                |
   |                                              |                                                                                    |    migrate-variables=True/False                    |
   |                                              |                                                                                    |    remove-intermediate-files=True/False            |
   |                                              |                                                                                    |    target_files=overwrite/cancel                   |
   |                                              |                                                                                    |    migrate-executequery=True/False                 |
   +----------------------------------------------+------------------------------------------------------------------------------------+----------------------------------------------------+
   | :ref:`MySQL SQL Migration <dws_07_0676>`     | -  DSC: *application.properties*                                                   | .. code-block::                                    |
   |                                              | -  :ref:`MySQL SQL Configuration <dws_07_0666>`: *features-mysql.properties*       |                                                    |
   |                                              |                                                                                    |    table.databaseAsSchema=true                     |
   |                                              |                                                                                    |    table.defaultSchema=public                      |
   |                                              |                                                                                    |    table.schema=                                   |
   |                                              |                                                                                    |    table.orientation=ROW                           |
   |                                              |                                                                                    |    table.type=HASH                                 |
   |                                              |                                                                                    |    table.partition-                                |
   |                                              |                                                                                    |    key.choose.strategy=partitionKeyChooserStrategy |
   |                                              |                                                                                    |    table.partition-key.name=                       |
   |                                              |                                                                                    |    table.compress.mode=NOCOMPRESS                  |
   |                                              |                                                                                    |    table.compress.level=0                          |
   |                                              |                                                                                    |    table.compress.row=NO                           |
   |                                              |                                                                                    |    table.compress.column=LOW                       |
   |                                              |                                                                                    |    table.database.template=template0               |
   +----------------------------------------------+------------------------------------------------------------------------------------+----------------------------------------------------+
   | :ref:`Netezza SQL Migration <dws_07_0675>`   | -  DSC: *application.properties*                                                   | .. code-block::                                    |
   |                                              | -  :ref:`Netezza Configuration <dws_mt_0030>`: *features-netezza.properties*       |                                                    |
   |                                              |                                                                                    |    rowstoreToColumnstore=false                     |
   +----------------------------------------------+------------------------------------------------------------------------------------+----------------------------------------------------+
   | :ref:`DB2 Syntax Migration <dws_07_0684>`    | DSC: *application.properties*                                                      | .. code-block::                                    |
   |                                              |                                                                                    |                                                    |
   |                                              |                                                                                    |    exceptionHandler=True/False                     |
   |                                              |                                                                                    |    TxHandler=True/False                            |
   |                                              |                                                                                    |    foreignKeyHandler=True/False                    |
   |                                              |                                                                                    |    globalTempTable=GLOBAL/LOCAL                    |
   |                                              |                                                                                    |    onCommitDeleteRows=Delete/Preserve              |
   |                                              |                                                                                    |    maxValInSequence=0..9223372036854775807         |
   |                                              |                                                                                    |    mergeImplementation=WITH/SPLIT                  |
   |                                              |                                                                                    |    RemoveHashPartition=True/False                  |
   |                                              |                                                                                    |    RemoveHashSubPartition=True/False               |
   |                                              |                                                                                    |    RemoveListPartition=True/False                  |
   |                                              |                                                                                    |    RemoveListSubPartition=True/False               |
   |                                              |                                                                                    |    RemoveRangeSubPartition=True/False              |
   |                                              |                                                                                    |    MigSupportSequence=True/False                   |
   +----------------------------------------------+------------------------------------------------------------------------------------+----------------------------------------------------+
