:original_name: dws_gsql_005.html

.. _dws_gsql_005:

Online Help
===========

Procedure
---------

-  When a database is being connected, run the following commands to obtain the help information:

   .. code-block::

      gsql --help

   The following information is displayed:

   .. code-block::

      ......
      Usage:
        gsql [OPTION]... [DBNAME [USERNAME]]

      General options:
        -c, --command=COMMAND    run only single command (SQL or internal) and exit
        -d, --dbname=DBNAME      database name to connect to (default: "postgres")
        -f, --file=FILENAME      execute commands from file, then exit
      ......

-  After the database is connected, run the following commands to obtain the help information:

   .. code-block::

      help

   The following information is displayed:

   .. code-block::

      You are using gsql, the command-line interface to gaussdb.
      Type:  \copyright for distribution terms
             \h for help with SQL commands
             \? for help with gsql commands
             \g or terminate with semicolon to execute query
             \q to quit

Task Example
------------

#. View the **gsql** help information. For details about the commands, see :ref:`Table 1 <en-us_topic_0000001234200681__en-us_topic_0000001233922251_en-us_topic_0000001145490533_te978a326b9df437dbb26940e8354859b>`.

   .. _en-us_topic_0000001234200681__en-us_topic_0000001233922251_en-us_topic_0000001145490533_te978a326b9df437dbb26940e8354859b:

   .. table:: **Table 1** gsql online help

      +---------------------------------------------------------------------------+---------------------------------------------------------------------------------------+
      | Description                                                               | Example                                                                               |
      +===========================================================================+=======================================================================================+
      | View copyright information.                                               | \\copyright                                                                           |
      +---------------------------------------------------------------------------+---------------------------------------------------------------------------------------+
      | View the help information about SQL statements supported by GaussDB(DWS). | View the help information about SQL statements supported by GaussDB(DWS).             |
      |                                                                           |                                                                                       |
      |                                                                           | For example, view all SQL statements supported by GaussDB(DWS).                       |
      |                                                                           |                                                                                       |
      |                                                                           | ::                                                                                    |
      |                                                                           |                                                                                       |
      |                                                                           |    \h                                                                                 |
      |                                                                           |    Available help:                                                                    |
      |                                                                           |      ABORT                                                                            |
      |                                                                           |      ALTER DATABAE                                                                    |
      |                                                                           |      ALTER DATA SOURCE                                                                |
      |                                                                           |    ... ...                                                                            |
      |                                                                           |                                                                                       |
      |                                                                           | For example, view parameters of the **CREATE DATABASE** command:                      |
      |                                                                           |                                                                                       |
      |                                                                           | ::                                                                                    |
      |                                                                           |                                                                                       |
      |                                                                           |    \help CREATE DATABASE                                                              |
      |                                                                           |    Command:     CREATE DATABASE                                                       |
      |                                                                           |    Description: create a new database                                                 |
      |                                                                           |    Syntax:                                                                            |
      |                                                                           |    CREATE DATABASE database_name                                                      |
      |                                                                           |         [ [ WITH ] {[ OWNER [=] user_name ]|                                          |
      |                                                                           |               [ TEMPLATE [=] template ]|                                              |
      |                                                                           |               [ ENCODING [=] encoding ]|                                              |
      |                                                                           |               [ LC_COLLATE [=] lc_collate ]|                                          |
      |                                                                           |               [ LC_CTYPE [=] lc_ctype ]|                                              |
      |                                                                           |               [ DBCOMPATIBILITY [=] compatibility_type ]|                             |
      |                                                                           |               [ TABLESPACE [=] tablespace_name ]|                                     |
      |                                                                           |               [ CONNECTION LIMIT [=] connlimit ]}[...] ];                             |
      +---------------------------------------------------------------------------+---------------------------------------------------------------------------------------+
      | View help information about gsql commands.                                | For example, view commands supported by **gsql**.                                     |
      |                                                                           |                                                                                       |
      |                                                                           | ::                                                                                    |
      |                                                                           |                                                                                       |
      |                                                                           |    \?                                                                                 |
      |                                                                           |    General                                                                            |
      |                                                                           |      \copyright             show PostgreSQL usage and distribution terms              |
      |                                                                           |      \g [FILE] or ;         execute query (and send results to file or |pipe)         |
      |                                                                           |      \h(\help) [NAME]              help on syntax of SQL commands, * for all commands |
      |                                                                           |      \q                     quit gsql                                                 |
      |                                                                           |    ... ...                                                                            |
      +---------------------------------------------------------------------------+---------------------------------------------------------------------------------------+
