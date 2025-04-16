:original_name: dws_07_0101.html

.. _dws_07_0101:

.. _en-us_topic_0000001860198597:

gs_dump
=======

Context
-------

**gs_dump** is tool provided by GaussDB(DWS) to export database information. You can export a database or its objects, such as schemas, tables, and views. The database can be the default **postgres** database or a user-specified database.

When **gs_dump** is used to export data, other users can still access the database (readable or writable).

**gs_dump** can export complete, consistent data. For example, if **gs_dump** is started to export database A at T1, data of the database at that time point will be exported, and modifications on the database after that time point will not be exported.

**gs_dump** can export database information to a plain-text SQL script file or archive file.

-  Plain-text SQL script: It contains the SQL statements required to restore the database. You can use **gsql** to execute the SQL script. With only a little modification, the SQL script can rebuild a database on other hosts or database products.
-  Archive file: It contains data required to restore the database. It can be a tar-, directory-, or custom-format archive. For details, see :ref:`Table 1 <en-us_topic_0000001860198597__en-us_topic_0000001860318617_en-us_topic_0000001099130242_teb56550d78ba40669508ee8a78bce1bc>`. The export result must be used with :ref:`gs_restore <en-us_topic_0000001860198833>` to restore the database. The system allows you to select or even to sort the content to import.

Functions
---------

**gs_dump** can create export files in four formats, which are specified by **-F** or **--format=**, as listed in :ref:`Table 1 <en-us_topic_0000001860198597__en-us_topic_0000001860318617_en-us_topic_0000001099130242_teb56550d78ba40669508ee8a78bce1bc>`.

.. _en-us_topic_0000001860198597__en-us_topic_0000001860318617_en-us_topic_0000001099130242_teb56550d78ba40669508ee8a78bce1bc:

.. table:: **Table 1** Formats of exported files

   +------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | Format     | Value of -F | Description                                                                                                                                                                                           | Suggestion                                                                       | Corresponding Import Tool                                                                                                      |
   +============+=============+=======================================================================================================================================================================================================+==================================================================================+================================================================================================================================+
   | Plain-text | p           | A plain-text script file containing SQL statements and commands. The commands can be executed on **gsql**, a command line terminal, to recreate database objects and load table data.                 | You are advised to use plain-text export files for small databases.              | Before using **gsql** to restore database objects, you can use a text editor to edit the exported plain-text file as required. |
   +------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | Custom     | c           | A binary file that allows the restoration of all or selected database objects from an exported file.                                                                                                  | You are advised to use custom-format archive files for medium or large database. | You can use :ref:`gs_restore <en-us_topic_0000001860198833>` to import database objects from a custom-format archive.          |
   +------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | Directory  | d           | A directory containing directory files and the data files of tables and BLOB objects.                                                                                                                 | ``-``                                                                            |                                                                                                                                |
   +------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | .tar       | t           | A tar-format archive that allows the restoration of all or selected database objects from an exported file. It cannot be further compressed and has an 8-GB limitation on the size of a single table. | ``-``                                                                            |                                                                                                                                |
   +------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+

.. note::

   To reduce the size of an exported file, you can use the **gs_dump** tool to compress it to a plain-text file or custom-format file. By default, a plain-text file is not compressed when generated. When a custom-format archive is generated, a medium level of compression is applied by default. Archived exported files cannot be compressed using **gs_dump**.

Precautions
-----------

Do not modify an exported file or its content. Otherwise, restoration may fail.

To ensure the data consistency and integrity, **gs_dump** acquires a share lock on a table to be dumped. If another transaction has acquired a share lock on the table, **gs_dump** waits until this lock is released and then locks the table for dumping. If the table cannot be locked within the specified time, the dump fails. You can customize the timeout duration to wait for lock release by specifying the **--lock-wait-timeout** parameter.

Syntax
------

.. code-block::

   gs_dump [OPTION]... [DBNAME]

.. note::

   *DBNAME* does not follow a short or long option. It specifies the database to connect to.

   For example:

   Specify *DBNAME* without a **-d** option preceding it.

   .. code-block::

      gs_dump -p port_number  postgres -f dump1.sql

   Or

   .. code-block::

      export PGDATABASE=postgres

   .. code-block::

       gs_dump -p port_number -f dump1.sql

   Environment variable: *PGDATABASE*

Parameter Description
---------------------

Common parameters:

-  -f, --file=FILENAME

   Sends the output to the specified file or directory. If this parameter is omitted, the standard output is generated. If the output format is **(-F c/-F d/-F t)**, the **-f** parameter must be specified. If the value of the **-f** parameter contains a directory, the directory has the read and write permissions to the current user.

-  -F, --format=c|d|t|p

   Selects the exported file format. Its format can be:

   -  **p|plain**: Generates a text SQL script file. This is the default value.

   -  **c|custom**: Outputs a custom-format archive as a directory to be used as the input of **gs_restore**. This is the most flexible output format in which users can manually select it and reorder the archived items during the restore process. An archive in this format is compressed by default.

   -  d|directory: A directory containing directory files and the data files of tables and BLOB objects.

   -  t|tar: Outputs a tar format as the archive form that is suitable for the input of **gs_restore**. The .tar format is compatible with the directory format. Extracting a .tar archive generates a valid directory-format archive. However, the .tar archive cannot be further compressed and has an 8-GB limitation on the size of a single table. The order of table data items cannot be changed during restoration.

      A .tar archive can be used as input of **gsql**.

-  -v, --verbose

   Specifies the verbose mode. If it is specified, **gs_dump** writes detailed object comments and the number of startups/stops to the dump file, and progress messages to standard error.

-  -V, --version

   Prints the *gs_dump* version and exits.

-  -Z, --compress=0-9

   Specifies the used compression level.

   Value range: 0 to 9

   -  **0** indicates no compression.
   -  **1** indicates that the compression ratio is the lowest and processing speed the fastest.
   -  **9** indicates the compression ratio is the highest and processing speed the slowest.

   For the custom-format archive, this option specifies the compression level of a single table data segment. By default, data is compressed at a medium level. Setting the non-zero compression level will result in that the entire text output files are to be compressed, as if the text has been compressed using the gzip tool, but the default method is non-compression. The .tar archive format does not support compression currently.

-  --lock-wait-timeout=TIMEOUT

   Do not keep waiting to obtain shared table locks at the beginning of the dump. Consider it as failed if you are unable to lock a table within the specified time. The timeout duration can be specified in any of the formats accepted by **SET statement_timeout**.

-  -?, --help

   Shows help about **gs_dump** parameters and exits.

Dump parameters:

-  -a, --data-only

   Generates only the data, not the schema (data definition). Dumps the table data, big objects, and sequence values.

-  -b, --blobs

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  -c, --clean

   Before writing the command of creating database objects into the backup file, write the command of clearing (deleting) database objects to the backup files. (If no objects exist in the target database, **gs_restore** probably displays some error information.)

   This parameter is used only for the plain-text format. For the archive format, you can specify the option when using **gs_restore**.

-  -C, --create

   The backup file content starts with the commands of creating the database and connecting to the created database. (If the script is in this format, any database to be connected is allowed before running the script.)

   This parameter is used only for the plain-text format. For the archive format, you can specify the option when using **gs_restore**.

-  -E, --encoding=ENCODING

   Creates a dump file in the specified character set encoding. By default, the dump file is created in the database encoding. (Alternatively, you can set the environment variable **PGCLIENTENCODING** to the required dump encoding.)

-  -n, --schema=SCHEMA

   Dumps only schemas matching the schema names. This option contains the schema and all its contained objects. If this option is not specified, all non-system schemas in the target database will be dumped. Multiple schemas can be selected by specifying multiple **-n** options. The schema parameter is interpreted as a pattern according to the same rules used by the **\\d** command of **gsql**. Therefore, multiple schemas can also be selected by writing wildcard characters in the pattern. When you use wildcards, quote the pattern to prevent the shell from expanding the wildcards.

   .. note::

      -  If **-n** is specified, **gs_dump** does not dump any other database objects that the selected schemas might depend upon. Therefore, there is no guarantee that the results of a specific-schema dump can be automatically restored to an empty database.
      -  If **-n** is specified, the non-schema objects are not dumped.

   Multiple schemas can be dumped. Entering **-n** *schemaname* multiple times dumps multiple schemas.

   For example:

   .. code-block::

      gs_dump -h host_name -p port_number postgres -f backup/bkp_shl2.sql -n sch1 -n sch2

   In the preceding example, **sch1** and **sch2** are dumped.

-  -N, --exclude-schema=SCHEMA

   Does not dump any tables matching the table pattern. The pattern is interpreted according to the same rules as for **-n**. **-N** can be specified multiple times to exclude schemas matching any of the specified patterns.

   When both **-n** and **-N** are specified, the schemas that match at least one **-n** option but no **-N** is dumped. If **-N** is specified and **-n** is not, the schemas matching **-N** are excluded from what is normally dumped.

   Dump allows you to exclude multiple schemas during dumping.

   Specifies **-N exclude schema name** to exclude multiple schemas while dumping.

   For example:

   .. code-block::

      gs_dump -h host_name -p port_number postgres -f backup/bkp_shl2.sql -N sch1 -N sch2

   In the preceding example, **sch1** and **sch2** will be excluded during the dumping.

-  -o, --oids

   Dumps object identifiers (OIDs) as parts of the data in each table. Use this parameter if your application references the OID columns in some way (for example, in a foreign key constraint). If the preceding situation does not occur, do not use this parameter.

-  -O, --no-owner

   Do not output commands to set ownership of objects to match the original database. By default, **gs_dump** issues the **ALTER OWNER** or **SET SESSION AUTHORIZATION** command to set ownership of created database objects. These statements will fail when the script is running unless it is started by a system administrator (or the same user that owns all of the objects in the script). To make a script that can be stored by any user and give the user ownership of all objects, specify **-O**.

   This parameter is used only for the plain-text format. For the archive format, you can specify the option when using **gs_restore**.

-  .. _en-us_topic_0000001860198597__en-us_topic_0000001860318617_en-us_topic_0000001099130242_l95dba45fc0df4807a8b924830aafbaf5:

   -s, --schema-only

   Dumps only the object definition (schema) but not data.

-  -S, --sysadmin=NAME

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  -t, --table=TABLE

   Specifies a list of tables, views, sequences, or foreign tables to be dumped. You can use multiple **-t** parameters or wildcard characters to specify tables.

   When using wildcards to specify dump tables, quote the pattern to prevent the shell from expanding the wildcards.

   The **-n** and **-N** options have no effect when **-t** is used, because tables selected by using **-t** will be dumped regardless of those options, and non-table objects will not be dumped.

   .. note::

      The number of **-t** parameters must be less than or equal to 100.

      If the number of **-t** parameters is greater than 100, you are advised to use the **--include-table-file** parameter to replace some **-t** parameters.

      If **-t** is specified, **gs_dump** does not dump any other database objects that the selected tables might depend upon. Therefore, there is no guarantee that the results of a specific-table dump can be automatically restored to an empty database.

      **-t tablename** only dumps visible tables in the default search path. **-t '*.tablename'** dumps *tablename* tables in all the schemas of the dumped database. **-t schema.table** dumps tables in a specific schema.

      **-t tablename** does not export the trigger information from a table.

   For example:

   .. code-block::

      gs_dump -h host_name -p port_number postgres -f backup/bkp_shl2.sql -t schema1.table1 -t schema2.table2

   In the preceding example, **schema1.table1** and **schema2.table2** are dumped.

-  --include-table-file=FILENAME

   Specifies the table file to be dumped.

-  -T, --exclude-table=TABLE

   Specifies a list of tables, views, sequences, or foreign tables not to be dumped. You can use multiple **-t** parameters or wildcard characters to specify tables.

   When **-t** and **-T** are input, the object will be stored in **-t** list not **-T** table object.

   For example:

   .. code-block::

      gs_dump -h host_name -p port_number postgres -f backup/bkp_shl2.sql -T table1 -T table2

   In the preceding example, **table1** and **table2** are excluded from the dumping.

-  --exclude-table-file=FILENAME

   Specifies the table file to be dumped.

   .. note::

      Same as **--include-table-file**, the content format of this parameter is as follows:

      schema1.table1

      schema2.table2

      ...

-  -x, --no-privileges|--no-acl

   Prevents the dumping of access permissions (grant/revoke commands).

-  --column-inserts|--attribute-inserts

   Exports data by running the **INSERT** command with explicit column names {INSERT INTO table (column, ...) VALUES ...}. This will cause a slow restoration. However, since this option generates an independent command for each row, an error in reloading a row causes only the loss of the row rather than the entire table content.

-  --disable-dollar-quoting

   Disables the use of dollar sign ($) for function bodies, and forces them to be quoted using the SQL standard string syntax.

-  --disable-triggers

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  --exclude-table-data=TABLE

   Does not dump data that matches any of table patterns. The pattern is interpreted according to the same rules as for **-t**.

   **--exclude-table-data** can be entered more than once to exclude tables matching any of several patterns. When the user needs the specified table definition rather than data in the table, this option is helpful.

   To exclude data of all tables in the database, see :ref:`--schema-only <en-us_topic_0000001860198597__en-us_topic_0000001860318617_en-us_topic_0000001099130242_l95dba45fc0df4807a8b924830aafbaf5>`.

-  --inserts

   Dumps data when the **INSERT** statement (rather than **COPY**) is issued. This will cause a slow restoration.

   However, since this option generates an independent command for each row, an error in reloading a row causes only the loss of the row rather than the entire table content. The restoration may fail if you rearrange the column order. The **--column-inserts** option is unaffected against column order changes, though even slower.

-  --no-security-labels

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  --no-tablespaces

   This parameter is no longer used in 8.2.0.100 and is only kept for compatibility with earlier versions.

   Does not issue commands to select tablespaces. All the objects will be created during the restoration process, no matter which tablespace is selected when using this option.

   This parameter is used only for the plain-text format. For the archive format, you can specify the option when using **gs_restore**.

-  --no-unlogged-table-data

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  --non-lock-table

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  --quote-all-identifiers

   Forcibly quotes all identifiers. This parameter is useful when you dump a database for migration to a later version, in which additional keywords may be introduced.

-  --section=SECTION

   Specifies dumped name sections (pre-data, data, or post-data).

-  --serializable-deferrable

   Uses a serializable transaction for the dump to ensure that the used snapshot is consistent with later database status. Perform this operation at a time point in the transaction flow, at which everything is normal. This ensures successful transaction and avoids serialization failures of other transactions, which requires serialization again.

   This option has no benefits for disaster recovery. During the upgrade of the original database, load a database copy as a report or other shared read-only dump is helpful. The option does not exist, dump reveals a status which is different from the submitted sequence status of any transaction.

   This option will make no difference if there are no active read-write transactions when **gs_dump** is started. If the read-write transactions are in active status, the dump start time will be delayed for an uncertain period.

-  --use-set-session-authorization

   Specifies that the standard SQL **SET SESSION AUTHORIZATION** command rather than **ALTER OWNER** is returned to ensure the object ownership. This makes dumping more standard. However, if a dump file contains objects that have historical problems, restoration may fail. A dump using **SET SESSION AUTHORIZATION** requires the system administrator rights, whereas **ALTER OWNER** requires lower permissions.

-  --with-encryption=AES128

   Specifies that dumping data needs to be encrypted using AES128.

-  --with-key=KEY

   Specifies that the key length of AES128 must be 16 bytes.

-  --include-nodes

   Includes the **TO NODE** or **TO GROUP** statement in the dumped **CREATE TABLE** or **CREATE FOREIGN TABLE** statement. This parameter is valid only for HDFS and foreign tables.

-  --include-extensions

   Includes extensions in the dump.

-  --include-depend-objs

   Includes information about the objects that depend on the specified object in the backup result. This parameter takes effect only if the **-t** or **--include-table-file** parameter is specified.

-  --exclude-self

   Excludes information about the specified object from the backup result. This parameter takes effect only if the **-t** or **--include-table-file** parameter is specified.

-  --cstore-fine-disaster (Discarded)

   If this parameter is selected and a table whose **fine_disaster_table_role** is **primary** is dumped, a table definition whose **fine_disaster_table_role** is **standby** is generated.

   This parameter is discarded in 8.2.1.

-  --only-publications

   If this parameter is specified, only the definitions of all publications (publication) in the current database are dumped. This parameter is supported by version 8.2.1 or later clusters.

-  --no-comment

   The dump does not contain object comments. This is supported only by clusters of version 9.1.0.100 or later.

-  --dont-overwrite-file

   The existing files in plain-text, .tar, and custom formats will be overwritten. This parameter is not used for the directory format.

   For example:

   Assume that the **backup.sql** file exists in the current directory. If you specify **-f backup.sql** in the input command, and the **backup.sql** file is generated in the current directory, the original file will be overwritten.

   If the backup file already exists and **--dont-overwrite-file** is specified, an error will be reported with the message that the dump file exists.

   .. code-block::

      gs_dump -p port_number postgres -f backup.sql -F plain --dont-overwrite-file

.. note::

   -  The **-s/--schema-only** and **-a/--data-only** parameters do not coexist.
   -  The **-c/--clean** and **-a/--data-only** parameters do not coexist.
   -  **--inserts/--column-inserts** and **-o/--oids** do not coexist, because **OIDS** cannot be set using the **INSERT** statement.
   -  **--role** must be used in conjunction with **--rolepassword**.
   -  **--binary-upgrade-usermap** must be used in conjunction with **--binary-upgrade**.
   -  **--include-depend-objs**/**--exclude-self** takes effect only when **-t**/**--include-table-file** is specified.
   -  **--exclude-self** must be used with **--include-depend-objs**.

Connection parameters:

-  -h, --host=HOSTNAME

   Specifies the host name. If the value begins with a slash (/), it is used as the directory for the UNIX domain socket. The default is taken from the PGHOST environment variable (if available). Otherwise, a Unix domain socket connection is attempted.

   This parameter is used only for defining names of the hosts outside a cluster. The names of the hosts inside the cluster must be 127.0.0.1.

   Example: the host name

   Environment Variable: *PGHOST*

-  -p, --port=PORT

   Specifies the host port.

   Environment variable: *PGPORT*

-  -U, --username=NAME

   Specifies the user name of the host to connect to.

   Environment variable: *PGUSER*

-  -w, --no-password

   Never issue a password prompt. The connection attempt fails if the host requires password verification and the password is not provided in other ways. This parameter is useful in batch jobs and scripts in which no user password is required.

-  -W, --password=PASSWORD

   Specifies the user password to connect to. If the host uses the trust authentication policy, the administrator does not need to enter the **-W** option. If the **-W** option is not provided and you are not a system administrator, the Dump Restore tool will ask you to enter a password.

-  --role=ROLENAME

   Specifies a role name to be used for creating the dump. If this option is selected, the **SET ROLE** command will be issued after the database is connected to **gs_dump**. It is useful when the authenticated user (specified by **-U**) lacks the permissions required by **gs_dump**. It allows the user to switch to a role with the required permissions. Some installations have a policy against logging in directly as a system administrator. This option allows dumping data without violating the policy.

-  --rolepassword=ROLEPASSWORD

   Password for the role

Description
-----------

**Scenario 1**

If your database cluster has any local additions to the template1 database, restore the output of **gs_dump** into an empty database with caution. Otherwise, you are likely to obtain errors due to duplicate definitions of the added objects. To create an empty database without any local additions, copy data from template0 rather than template1. Example:

.. code-block::

   CREATE DATABASE foo WITH TEMPLATE template0;

The .tar format file size must be smaller than 8 GB. (This is the tar file format limitations.) The total size of a .tar archive and any of the other output formats are not limited, except possibly by the OS.

The dump file generated by **gs_dump** does not contain the statistics used by the optimizer to make execution plans. Therefore, you are advised to run **ANALYZE** after restoring data from a dump file to ensure optimal performance. The dump file does not contain any **ALTER DATABASE ... SET** commands; these settings are dumped by **gs_dumpall**, along with database users and other installation settings.

**Scenario 2**

When the value of **SEQUENCE** reaches the maximum or minimum value, backing up the value of **SEQUENCE** using **gs_dump** will exit due to an execution error. Handle the problem by referring to the following example:

#. The value of **SEQUENCE** reaches the maximum value, but the maximum value is less than **2^63-2**.

Error message example:

Object defined by sequence

.. code-block::

   CREATE SEQUENCE seq INCREMENT 1 MINVALUE 1 MAXVALUE 3 START WITH 1;

Perform the **gs_dump** backup.

.. code-block::

   gs_dump -U dbadmin -W {password} -p 37300 postgres -t PUBLIC.seq -f backup/MPPDB_backup.sql
   gs_dump[port='37300'][postgres][2019-12-27 15:09:49]: The total objects number is 337.
   gs_dump[port='37300'][postgres][2019-12-27 15:09:49]: WARNING:  get invalid xid from GTM because connection is not established
   gs_dump[port='37300'][postgres][2019-12-27 15:09:49]: WARNING:  Failed to receive GTM rollback transaction response  for aborting prepared (null).
   gs_dump: [port='37300'] [postgres] [archiver (db)] [2019-12-27 15:09:49] query failed: ERROR:  Can not connect to gtm when getting gxid, there is a connection error.
   gs_dump: [port='37300'] [postgres] [archiver (db)] [2019-12-27 15:09:49] query was: RELEASE bfnextval

Handling procedure:

Run the following SQL statement to connect to the PostgreSQL database and change the maximum value of **sequence seq1**:

.. code-block::

   gsql -p 37300 postgres -r -c "ALTER SEQUENCE PUBLIC.seq MAXVALUE 10;"

Use the dump tool to back up the data.

.. code-block::

   gs_dump -U dbadmin -W {password} -p 37300 postgres -t PUBLIC.seq -f backup/MPPDB_backup.sql
   gs_dump[port='37300'][postgres][2019-12-27 15:10:53]: The total objects number is 337.
   gs_dump[port='37300'][postgres][2019-12-27 15:10:53]: [100.00%] 337 objects have been dumped.
   gs_dump[port='37300'][postgres][2019-12-27 15:10:53]: dump database postgres successfully
   gs_dump[port='37300'][postgres][2019-12-27 15:10:53]: total time: 230  ms

2. The value of **SEQUENCE** reaches the minimum or the maximum value of **2^63-2**.

The **gs_dump** command does not support backup of the **SEQUENCE** value in this scenario.

.. note::

   The SQL end does not support the modification of **MAXVALUE** when **SEQUENCE** reaches the maximum value of **2^63-2** or the modification of **MINVALUE** when **SEQUENCE** reaches the minimum value.

**Scenario 3**

**gs_dump** is mainly used to export metadata of the entire database. The performance of exporting a single table is optimized, but the performance of exporting multiple tables is poor. If multiple tables need to be exported, you are advised to export them one by one. Example:

.. code-block::

   gs_dump -U dbadmin -W {password} -p 37300 postgres -t public.table01 -s -f backup/table01.sql
   gs_dump -U dbadmin -W {password} -p 37300 postgres -t public.table02 -s -f backup/table02.sql

When services are stopped or during off-peak hours, you can increase the value of **--non-lock-table** to improve the **gs_dump** performance. Example:

.. code-block::

   gs_dump -U dbadmin -W {password} -p 37300 postgres -t public.table03 -s --non-lock-table -f backup/table03.sql

Examples
--------

Use **gs_dump** to dump a database as a SQL text file or a file in other formats.

In the following examples, **password** indicates the password configured by the database user. **backup/MPPDB_backup.sql** indicates an exported file where **backup** indicates the relative path of the current directory. **37300** indicates the port ID of the database server. **postgres** indicates the name of the database to be accessed.

.. note::

   Before exporting files, ensure that the directory exists and you have the read and write permissions on the directory.

Example 1: Use **gs_dump** to export the full information of the **postgres** database. The exported **MPPDB_backup.sql** file is in plain-text format.

.. code-block::

   gs_dump -U dbadmin -W {password} -f backup/MPPDB_backup.sql -p 37300 postgres -F p
   gs_dump[port='37300'][postgres][2018-06-27 09:49:17]: The total objects number is 356.
   gs_dump[port='37300'][postgres][2018-06-27 09:49:17]: [100.00%] 356 objects have been dumped.
   gs_dump[port='37300'][postgres][2018-06-27 09:49:17]: dump database postgres successfully
   gs_dump[port='37300'][postgres][2018-06-27 09:49:17]: total time: 1274  ms

Use **gsql** to import data from the export plain-text file.

Example 2: Use **gs_dump** to export the full information of the **postgres** database. The exported **MPPDB_backup.tar** file is in .tar format.

.. code-block::

   gs_dump -U dbadmin -W {password} -f backup/MPPDB_backup.tar -p 37300 postgres -F t
   gs_dump[port='37300'][postgres][2018-06-27 10:02:24]: The total objects number is 1369.
   gs_dump[port='37300'][postgres][2018-06-27 10:02:53]: [100.00%] 1369 objects have been dumped.
   gs_dump[port='37300'][postgres][2018-06-27 10:02:53]: dump database postgres successfully
   gs_dump[port='37300'][postgres][2018-06-27 10:02:53]: total time: 50086  ms

Example 3: Use **gs_dump** to export the full information of the **postgres** database. The exported **MPPDB_backup.dmp** file is in custom format.

.. code-block::

   gs_dump -U dbadmin -W {password} -f backup/MPPDB_backup.dmp -p 37300 postgres -F c
   gs_dump[port='37300'][postgres][2018-06-27 10:05:40]: The total objects number is 1369.
   gs_dump[port='37300'][postgres][2018-06-27 10:06:03]: [100.00%] 1369 objects have been dumped.
   gs_dump[port='37300'][postgres][2018-06-27 10:06:03]: dump database postgres successfully
   gs_dump[port='37300'][postgres][2018-06-27 10:06:03]: total time: 36620  ms

Example 4: Use **gs_dump** to export the full information of the **postgres** database. The exported **MPPDB_backup** file is in directory format.

.. code-block::

   gs_dump -U dbadmin -W {password} -f backup/MPPDB_backup -p 37300  postgres -F d
   gs_dump[port='37300'][postgres][2018-06-27 10:16:04]: The total objects number is 1369.
   gs_dump[port='37300'][postgres][2018-06-27 10:16:23]: [100.00%] 1369 objects have been dumped.
   gs_dump[port='37300'][postgres][2018-06-27 10:16:23]: dump database postgres successfully
   gs_dump[port='37300'][postgres][2018-06-27 10:16:23]: total time: 33977  ms

Example 5: Use **gs_dump** to export the information of the **postgres** database, excluding the information of the table specified in the **/home/MPPDB_temp.sql** file. The exported **MPPDB_backup.sql** file is in plain-text format.

.. code-block::

   gs_dump -U dbadmin -W {password} -p 37300 postgres --exclude-table-file=/home/MPPDB_temp.sql -f backup/MPPDB_backup.sql
   gs_dump[port='37300'][postgres][2018-06-27 10:37:01]: The total objects number is 1367.
   gs_dump[port='37300'][postgres][2018-06-27 10:37:22]: [100.00%] 1367 objects have been dumped.
   gs_dump[port='37300'][postgres][2018-06-27 10:37:22]: dump database postgres successfully
   gs_dump[port='37300'][postgres][2018-06-27 10:37:22]: total time: 37017  ms

Example 6: Use **gs_dump** to export only the information about the views that depend on the **testtable** table. Create another **testtable** table, and then restore the views that depend on it.

Back up only the views that depend on the **testtable** table.

.. code-block::

   gs_dump -s -p 37300 postgres -t PUBLIC.testtable --include-depend-objs --exclude-self -f backup/MPPDB_backup.sql -F p
   gs_dump[port='37300'][postgres][2018-06-15 14:12:54]: The total objects number is 331.
   gs_dump[port='37300'][postgres][2018-06-15 14:12:54]: [100.00%] 331 objects have been dumped.
   gs_dump[port='37300'][postgres][2018-06-15 14:12:54]: dump database postgres successfully
   gs_dump[port='37300'][postgres][2018-06-15 14:12:54]: total time: 327  ms

Change the name of the **testtable** table.

.. code-block::

   gsql -p 37300 postgres -r -c "ALTER TABLE PUBLIC.testtable RENAME TO testtable_bak;"

Create a **testtable** table.

.. code-block::

   CREATE TABLE PUBLIC.testtable(a int, b int, c int);

Restore the views for the new **testtable** table.

.. code-block::

   gsql -p 37300 postgres -r -f backup/MPPDB_backup.sql

Helpful Links
-------------

:ref:`gs_dumpall <en-us_topic_0000001860318545>` and :ref:`gs_restore <en-us_topic_0000001860198833>`
