:original_name: dws_07_0102.html

.. _dws_07_0102:

.. _en-us_topic_0000001148856437:

gs_dumpall
==========

Background
----------

**gs_dumpall** is a tool provided by GaussDB(DWS) to export all database information, including the data of the default **postgres** database, data of user-specified databases, and global objects of all databases in a cluster.

When **gs_dumpall** is used to export data, other users still can access the databases (readable or writable) in a cluster.

**gs_dumpall** can export complete, consistent data. For example, if **gs_dumpall** is started to export all databases from a cluster at T1, data of the databases at that time point will be exported, and modifications on the databases after that time point will not be exported.

To export all databases in a cluster:

-  **gs_dumpall** exports all global objects, including information about database users and groups, tablespaces, and attributes (for example, global access permissions).
-  **gs_dumpall** invokes **gs_dump** to export SQL scripts, which contain all the SQL statements required for restoring databases.

Both of the preceding exported files are plain-text SQL scripts. Use **gsql** to execute them to restore databases.

Precautions
-----------

-  Do not modify an exported file or its content. Otherwise, restoration may fail.
-  To ensure the data consistency and integrity, **gs_dumpall** sets a share lock for a table to be dumped. If a share lock has been set for the table in other transactions, **gs_dumpall** locks the table after it is released. If the table cannot be locked in the specified time, the dump will fail. You can customize the timeout duration to wait for lock release by specifying the **--lock-wait-timeout** parameter.
-  During an export, **gs_dumpall** reads all tables in a database. Therefore, you need to connect to the database as a cluster administrator to export a complete file. When using **gsql** to execute the SQL scripts, you need the administrator permissions so that you can add users and user groups, and create databases.

Syntax
------

.. code-block::

   gs_dumpall [OPTION]...

Parameter Description
---------------------

Common parameters:

-  -f, --filename=FILENAME

   Sends the output to the specified file. If this parameter is omitted, the standard output is used.

-  -v, --verbose

   Specifies the verbose mode. This will cause **gs_dumpall** to output detailed object comments and start/stop times to the dump file, and progress messages to standard error.

-  -V, --version

   Prints the version information for **gs_dumpall** and exits.

-  --lock-wait-timeout=TIMEOUT

   Do not keep waiting to obtain shared table locks at the beginning of the dump. Consider it as failed if you are unable to lock a table within the specified time. The timeout duration can be specified in any of the formats accepted by **SET statement_timeout**.

-  -?, --help

   Shows help about the command line parameters for **gs_dumpall** and exits.

Dump parameters:

-  -a, --data-only

   Dumps only the data, not the schema (data definition).

-  -c, --clean

   Runs SQL statements to delete databases before rebuilding them. Statements for dumping roles and tablespaces are added.

-  -g, --globals-only

   Dumps only global objects (roles and tablespaces) but no databases.

-  -o, --oids

   Dumps object identifiers (OIDs) as parts of the data in each table. Use this parameter if your application references the OID columns in some way (for example, in a foreign key constraint). If the preceding situation does not occur, do not use this parameter.

-  -O, --no-owner

   Do not output commands to set ownership of objects to match the original database. By default, **gs_dumpall** issues the **ALTER OWNER** or **SET SESSION AUTHORIZATION** statement to set ownership of created schema elements. These statements will fail when the script is running unless it is started by a system administrator (or the same user that owns all of the objects in the script). To make a script that can be stored by any user and give the user ownership of all objects, specify **-O**.

-  -r, --roles-only

   Dumps only roles but not databases or tablespaces.

-  -s, --schema-only

   Dumps only the object definition (schema) but not data.

-  -S, --sysadmin=NAME

   Name of the system administrator during the dump.

-  -t, --tablespaces-only

   Dumps only tablespaces but not databases or roles.

-  -x, --no-privileges

   Prevents the dumping of access permissions (grant/revoke commands).

-  --column-inserts|--attribute-inserts

   Exports data by running the **INSERT** command with explicit column names {INSERT INTO table (column, ...) VALUES ...}. This will cause a slow restoration. However, since this option generates an independent command for each row, an error in reloading a row causes only the loss of the row rather than the entire table content.

-  --disable-dollar-quoting

   Disables the use of dollar sign ($) for function bodies, and forces them to be quoted using the SQL standard string syntax.

-  --disable-triggers

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  --inserts

   Dumps data when the **INSERT** statement (rather than **COPY**) is issued. This will cause a slow restoration. The restoration may fail if you rearrange the column order. The **--column-inserts** parameter is safer against column order changes, though even slower.

-  --no-security-labels

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  --no-tablespaces

   Do not output statements to create tablespaces or select tablespaces for objects. All the objects will be created during the restoration process, no matter which tablespace is selected when using this option.

-  --no-unlogged-table-data

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  --quote-all-identifiers

   Forcibly quotes all identifiers. This parameter is useful when you dump a database for migration to a later version, in which additional keywords may be introduced.

-  --dont-overwrite-file

   Do not overwrite the current file.

-  --use-set-session-authorization

   Specifies that the standard SQL **SET SESSION AUTHORIZATION** command rather than **ALTER OWNER** is returned to ensure the object ownership. This makes dumping more standard. However, if a dump file contains objects that have historical problems, restoration may fail. A dump using **SET SESSION AUTHORIZATION** requires the system administrator rights, whereas **ALTER OWNER** requires lower permissions.

-  --with-encryption=AES128

   Specifies that dumping data needs to be encrypted using AES128.

-  --with-key=KEY

   Specifies that the key length of AES128 must be 16 bytes.

-  --include-extensions

   Backs up all CREATE EXTENSION statements if the **include-extensions** parameter is set.

-  --include-templatedb

   Includes template databases during the dump.

-  --dump-nodes

   Includes nodes and Node Groups during the dump.

-  --include-nodes

   Includes the **TO NODE** statement in the dumped **CREATE TABLE** statement.

-  --include-buckets

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  --dump-wrm

   Includes workload resource manager (resource pool, load group, and load group mapping) during the dump.

-  --binary-upgrade

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  --binary-upgrade-usermap="USER1=USER2"

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  --tablespaces-postfix

   Specifies a reserved port for function expansion. This parameter is not recommended.

-  --parallel-jobs

   Specifies the number of concurrent backup processes. The value range is 1-1000.

.. note::

   -  The **-g/--globals-only** and **-r/--roles-only** parameters do not coexist.
   -  The **-g/--globals-only** and **-t/--tablespaces-only** parameters do not coexist.
   -  The **-r/--roles-only** and **-t/--tablespaces-only** parameters do not coexist.
   -  The **-s/--schema-only** and **-a/--data-only** parameters do not coexist.
   -  The **-r/--roles-only** and **-a/--data-only** parameters do not coexist.
   -  The **-t/--tablespaces-only** and **-a/--data-only** parameters do not coexist.
   -  The **-g/--globals-only** and **-a/--data-only** parameters do not coexist.
   -  **--tablespaces-postfix** must be used in conjunction with **--binary-upgrade**.
   -  **--parallel-jobs** must be used in conjunction with **-f/--file**.

Connection parameters:

-  -h, --host

   Specifies the host name. If the value begins with a slash (/), it is used as the directory for the UNIX domain socket. The default value is taken from the *PGHOST* environment variable. If it is not set, a UNIX domain socket connection is attempted.

   This parameter is used only for defining names of the hosts outside a cluster. The names of the hosts inside the cluster must be 127.0.0.1.

   Environment Variable: *PGHOST*

-  -l, --database

   Specifies the name of the database connected to dump all objects and discover other databases to be dumped. If this parameter is not specified, the **postgres** database will be used. If the **postgres** database does not exist, **template1** will be used.

-  -p, --port

   Specifies the TCP port listened to by the server or the local UNIX domain socket file name extension to ensure a correct connection. The default value is the *PGPORT* environment variable.

   Environment variable: *PGPORT*

-  -U, --username

   Specifies the user name to connect to.

   Environment variable: *PGUSER*

-  -w, --no-password

   Never issue a password prompt. The connection attempt fails if the host requires password verification and the password is not provided in other ways. This parameter is useful in batch jobs and scripts in which no user password is required.

-  -W, --password

   Specifies the user password to connect to. If the host uses the trust authentication policy, the administrator does not need to enter the **-W** option. If the **-W** option is not provided and you are not a system administrator, the Dump Restore tool will ask you to enter a password.

-  --role

   Specifies a role name to be used for creating the dump. This option causes **gs_dumpall** to issue the **SET ROLE** statement after connecting to the database. It is useful when the authenticated user (specified by **-U**) lacks the permissions required by **gs_dumpall**. It allows the user to switch to a role with the required permissions. Some installations have a policy against logging in directly as a system administrator. This option allows dumping data without violating the policy.

-  --rolepassword

   Specifies the password of the specific role.

Description
-----------

The **gs_dumpall** internally invokes :ref:`gs_dump <en-us_topic_0000001149216491>`. For details about the diagnosis information, see :ref:`gs_dump <en-us_topic_0000001149216491>`.

Once **gs_dumpall** is restored, run ANALYZE on each database so that the optimizer can provide useful statistics.

**gs_dumpall** requires all needed tablespace directories to exit before the restoration. Otherwise, database creation will fail if the databases are in non-default locations.

Examples
--------

Run **gs_dumpall** to export all databases from a cluster at a time.

.. note::

   **gs_dumpall** supports only plain-text format export. Therefore, only **gsql** can be used to restore a file exported using **gs_dumpall**.

.. code-block::

   gs_dumpall -f backup/bkp2.sql -p 37300
   gs_dump[port='37300'][dbname='postgres'][2018-06-27 09:55:09]: The total objects number is 2371.
   gs_dump[port='37300'][dbname='postgres'][2018-06-27 09:55:35]: [100.00%] 2371 objects have been dumped.
   gs_dump[port='37300'][dbname='postgres'][2018-06-27 09:55:46]: dump database dbname='postgres' successfully
   gs_dump[port='37300'][dbname='postgres'][2018-06-27 09:55:46]: total time: 55567  ms
   gs_dumpall[port='37300'][2018-06-27 09:55:46]: dumpall operation successful
   gs_dumpall[port='37300'][2018-06-27 09:55:46]: total time: 56088  ms

Helpful Links
-------------

:ref:`gs_dump <en-us_topic_0000001149216491>` and :ref:`gs_restore <en-us_topic_0000001102016632>`
