:original_name: dws_16_0018.html

.. _dws_16_0018:

.. _en-us_topic_0000001860198829:

Prerequisites
=============

.. _en-us_topic_0000001860198829__en-us_topic_0000001382208082_section20896201617216:

Executing Custom DB Scripts
---------------------------

Custom scripts are executed to support input keywords that do not exist in certain versions of the target database. These scripts must be executed in each target database before the migration.

:ref:`Table 1 <en-us_topic_0000001860198829__en-us_topic_0000001382208082_table101312310197>` describes the custom scripts in the **DSC/scripts/** directory. For details about how to execute custom scripts, see :ref:`Custom DB Script Configuration <en-us_topic_0000001860198829__en-us_topic_0000001382208082_section974418973610>`.

.. _en-us_topic_0000001860198829__en-us_topic_0000001382208082_table101312310197:

.. table:: **Table 1** Custom DB scripts

   +------------------------------------+---------------------------------------------------+
   | Script                             | Description                                       |
   +====================================+===================================================+
   | mig_fn_get_datatype_short_name.sql | Custom DB script for Teradata functions           |
   +------------------------------------+---------------------------------------------------+
   | mig_fn_castasint.sql               | Custom DB script for migration of CAST AS INTEGER |
   +------------------------------------+---------------------------------------------------+
   | vw_td_dbc_tables.sql               | Custom DB script for migration of DBC.TABLES      |
   +------------------------------------+---------------------------------------------------+
   | vw_td_dbc_indices.sql              | Custom DB script for migration of DBC.INDICES     |
   +------------------------------------+---------------------------------------------------+

Follow the steps to execute custom DB scripts:

#. Use any of the following methods to execute the required scripts in all target databases for which migration is to be performed:

   -  Use **gsql**.

      -  Use **gsql** to connect to the target database and paste all content in the SQL file to **gsql**, which will automatically execute the pasted contents.

         Run the following command to connect to the database:

         ::

            gsql -h <host_addr_xxx.xxx.xxx.xxx> -d <database_name> -U <user_name> -W <password> -p <port_number> -r

      -  Use **gsql** to connect to the target database and execute a SQL file.

         Run the following command to connect to the database and run the SQL file:

         ::

            gsql -h <host_addr_xxx.xxx.xxx.xxx> -d <database_name> -U <user_name>  -W <password>  -p <port_number> -f <filename.sql> -o <output_filename> -L <log_filename.log>  -r

   -  Use Data Studio.

      Use Data Studio to connect to the target database, and then open and run the SQL file in Data Studio.

.. _en-us_topic_0000001860198829__en-us_topic_0000001382208082_section974418973610:

Custom DB Script Configuration
------------------------------

Custom scripts are SQL files used to migrate from Teradata/Oracle the input keywords that do not exist in the target database.

These scripts must be executed in each target database before the migration.

Open the **scripts** folder in the release package. :ref:`Table 2 <en-us_topic_0000001860198829__en-us_topic_0000001382208082_table177441591362>` lists the folders and files in the **scripts** folder.

The SQL files contain the scripts for the custom migration functions. The GaussDB(DWS) database needs to use these functions to support specific features of Teradata.

.. _en-us_topic_0000001860198829__en-us_topic_0000001382208082_table177441591362:

.. table:: **Table 2** Custom DB scripts for DSC

   +--------------------+------------------------------------+----------------------------------------------------------+
   | Folder             | Script File                        | Description                                              |
   +====================+====================================+==========================================================+
   | -- scripts         | ``-``                              | Folder: all scripts                                      |
   +--------------------+------------------------------------+----------------------------------------------------------+
   | ------ teradata    | ``-``                              | Folder: Teradata functions and scripts                   |
   +--------------------+------------------------------------+----------------------------------------------------------+
   | -------- view      | ``-``                              | Folder: scripts to configure views                       |
   +--------------------+------------------------------------+----------------------------------------------------------+
   | ``-``              | vw_td_dbc_tables.sql               | Script: used to enable migration of Teradata DBC.TABLES  |
   +--------------------+------------------------------------+----------------------------------------------------------+
   | ``-``              | vw_td_dbc_indices.sql              | Script: used to enable migration of Teradata DBC.INDICES |
   +--------------------+------------------------------------+----------------------------------------------------------+
   | -------- function  | ``-``                              | Folder: scripts to configure Teradata system functions   |
   +--------------------+------------------------------------+----------------------------------------------------------+
   | -X                 | mig_fn_get_datatype_short_name.sql | Script: used to enable migration of Teradata DBC.COLUMNS |
   +--------------------+------------------------------------+----------------------------------------------------------+
   | ``-``              | mig_fn_castasint.sql               | Script: used to enable migration of CAST AS INTEGER      |
   +--------------------+------------------------------------+----------------------------------------------------------+
   | --------db_scripts | ``-``                              | Folder: scripts to enable Teradata custom functions      |
   +--------------------+------------------------------------+----------------------------------------------------------+
   | ``-``              | mig_fn_get_datatype_short_name.sql | Script: used to enable migration of Teradata DBC.COLUMNS |
   +--------------------+------------------------------------+----------------------------------------------------------+
   | --------core       | ``-``                              | Folder: Teradata core scripts                            |
   +--------------------+------------------------------------+----------------------------------------------------------+
   | ``-``              | teradatacore.pm                    | Script: used to perform Perl migration                   |
   +--------------------+------------------------------------+----------------------------------------------------------+

Configuring DSC and Migration Properties
----------------------------------------

To configure DSC, configure parameters in the configuration files in the **config** folder of DSC. :ref:`Table 3 <en-us_topic_0000001860198829__en-us_topic_0000001382208082_table132687359208>` describes the parameters.

.. _en-us_topic_0000001860198829__en-us_topic_0000001382208082_table132687359208:

.. table:: **Table 3** Parameters for configuring DSC

   +---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------+
   | Migration                                                     | Configuration File                                                                                  | Parameter                                                          |
   +===============================================================+=====================================================================================================+====================================================================+
   | :ref:`Teradata SQL Migration <en-us_topic_0000001860198977>`  | -  DSC: *application.properties*                                                                    | ::                                                                 |
   |                                                               | -  :ref:`Teradata SQL Configuration <en-us_topic_0000001813438796>`: *features-teradata.properties* |                                                                    |
   |                                                               |                                                                                                     |    deleteToTruncate=True/False                                     |
   |                                                               |                                                                                                     |    distributeByHash=one/many                                       |
   |                                                               |                                                                                                     |    extendedGroupByClause=True/False                                |
   |                                                               |                                                                                                     |    inToExists=True/False                                           |
   |                                                               |                                                                                                     |    rowstoreToColumnstore=True/False                                |
   |                                                               |                                                                                                     |    session_mode=Teradata/ANSI                                      |
   |                                                               |                                                                                                     |    tdMigrateDollar=True/False                                      |
   |                                                               |                                                                                                     |    tdMigrateALIAS=True/False                                       |
   |                                                               |                                                                                                     |    tdMigrateNULLIFZero=True/False                                  |
   |                                                               |                                                                                                     |    tdMigrateZEROIFNULL=True/False                                  |
   |                                                               |                                                                                                     |    volatile=local temporary/unlogged                               |
   +---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------+
   | :ref:`Teradata Perl Migration <en-us_topic_0000001860318449>` | -  DSC: *application.properties*                                                                    | ::                                                                 |
   |                                                               | -  :ref:`Teradata Perl Configuration <en-us_topic_0000001813439212>`: *perl-migration.properties*   |                                                                    |
   |                                                               |                                                                                                     |    add-timing-on=True/False                                        |
   |                                                               |                                                                                                     |    db-bteq-tag-name=bteq                                           |
   |                                                               |                                                                                                     |    db-tdsql-tag-name=sql_lang                                      |
   |                                                               |                                                                                                     |    logging-level=error/warning/info                                |
   |                                                               |                                                                                                     |    migrate-variables=True/False                                    |
   |                                                               |                                                                                                     |    remove-intermediate-files=True/False                            |
   |                                                               |                                                                                                     |    target_files=overwrite/cancel                                   |
   |                                                               |                                                                                                     |    migrate-executequery=True/False                                 |
   +---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------+
   | :ref:`MySQL SQL Migration <en-us_topic_0000001860198801>`     | -  DSC: *application.properties*                                                                    | ::                                                                 |
   |                                                               | -  :ref:`MySQL Configuration <en-us_topic_0000001860318481>`: *features-mysql.properties*           |                                                                    |
   |                                                               |                                                                                                     |    table.databaseAsSchema=true                                     |
   |                                                               |                                                                                                     |    table.defaultSchema=public                                      |
   |                                                               |                                                                                                     |    table.schema=                                                   |
   |                                                               |                                                                                                     |    table.orientation=ROW                                           |
   |                                                               |                                                                                                     |    table.type=HASH                                                 |
   |                                                               |                                                                                                     |    table.partition-key.choose.strategy=partitionKeyChooserStrategy |
   |                                                               |                                                                                                     |    table.partition-key.name=                                       |
   |                                                               |                                                                                                     |    table.compress.mode=NOCOMPRESS                                  |
   |                                                               |                                                                                                     |    table.compress.level=0                                          |
   |                                                               |                                                                                                     |    table.compress.row=NO                                           |
   |                                                               |                                                                                                     |    table.compress.column=LOW                                       |
   |                                                               |                                                                                                     |    table.database.template=template0                               |
   |                                                               |                                                                                                     |    table.index.rename=false                                        |
   |                                                               |                                                                                                     |    table.database.onlyFullGroupBy=true                             |
   |                                                               |                                                                                                     |    table.database.realAsFloat=false                                |
   +---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------+
