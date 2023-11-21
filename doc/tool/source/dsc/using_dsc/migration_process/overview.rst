:original_name: dws_mt_0207.html

.. _dws_mt_0207:

Overview
========

The following use cases for migration are supported by DSC:

-  Migrate Teradata SQL
-  Migrate Oracle SQL
-  Migrate Teradata Perl files
-  Migrate Netezza
-  Migrate MySQL SQL
-  Migrate DB2

:ref:`Figure 1 <en-us_topic_0000001188681026__en-us_topic_0218440516_fig3820191814472>` shows the DSC Migration Process.

.. _en-us_topic_0000001188681026__en-us_topic_0218440516_fig3820191814472:

.. figure:: /_static/images/en-us_image_0000001234200759.png
   :alt: **Figure 1** Syntax migration process

   **Figure 1** Syntax migration process

This section contains information about the prerequisites to be completed before starting the migration process.

Executing Custom Scripts
------------------------

The DSC configuration contains the following custom DB scripts in the *DSC/scripts*:

-  *date_functions.sql* : Custom DB script for Oracle date functions
-  *environment_functions.sql*: Custom DB script for Oracle environment functions
-  *string_functions.sql*: Custom DB script for Oracle string functions
-  *pkg_variable_scripts.sql*: Custom DB script for Oracle package variable functions
-  *sequence_scripts.sql*: Custom DB script for Oracle sequence functions
-  *mig_fn_get_datatype_short_name.sql*: Custom DB script for Teradata functions
-  *mig_fn_castasint.sql* : Custom DB script for migration of CAST AS INTEGER
-  *vw_td_dbc_tables.sql*: Custom DB script for migration of DBC.TABLES
-  *vw_td_dbc_indices.sql*: Custom DB script for migration of DBC.INDICES

These DB scripts are required to support certain input keywords not present in one or more versions of the target DB. These scripts need to be executed once in the target DB prior to migration.

For details about executing custom database scripts, see :ref:`Custom DB Script Configuration <en-us_topic_0000001233922139__en-us_topic_0218440503_section974418973610>`.

Use any of the following methods to execute the required scripts in all target GaussDB(DWS) databases for which migration is to be performed:

-  Run the following command to connect to the GaussDB(DWS) database and paste all content in the .sql file to **gsql**, which will automatically execute the pasted content.

   Run the following command to connect to the GaussDB(DWS) database:

   ::

      gsql -h <host_addr_xxx.xxx.xxx.xxx> -d <database_name> -U <user_name> -W <password> -p <port_number> -r

-  Use gsql to connect to the GaussDB(DWS) database and execute the **.sql** file:

   Run the following command to connect to the GaussDB(DWS) database and execute the **.sql** file:

   ::

      gsql -h <host_addr_xxx.xxx.xxx.xxx> -d <database_name> -U <user_name>  -W <password>  -p <port_number> -f <filename.sql> -o <output_filename> -L <log_filename.log>  -r

-  Use Data Studio to connect to the GaussDB(DWS) database, and open and execute the **.sql** file in Data Studio.

-  Before Oracle PL/SQL objects (procedures or functions) are migrated, migrate all DDL and DML using the **Bulk** migration type. Then, migrate the scripts containing PL/SQL objects using the **BLogic** migration type.

   .. note::

      If the migration type is **Bulk**, the input file cannot contain any PL/SQL objects.

      Similarly, if the migration type is **BLogic**, the input files must not contain any DDL/DML.

Configuring DSC and Migration Properties
----------------------------------------

The DSC configuration contains the following configuration files in the *DSC/config* folder:

-  *application.properties*: Configuration parameters for the DSC
-  *features-teradata.properties*: Configuration parameters for Teradata SQL Migration
-  *features-oracle.properties*: Configuration parameters for Oracle SQL Migration
-  *oracle-migration.properties*: Configuration parameters for Oracle (Beta) SQL Migration
-  *perl-migration.properties*: Configuration parameters for Perl Migration
-  *features-netezza.properties*: Configuration parameters for Netezza Migration
-  *features-mysql.properties*: Configuration parameters for MySQL SQL Migration

For details about how to update configuration parameters, see :ref:`DSC Configuration <dws_mt_0025>`.
