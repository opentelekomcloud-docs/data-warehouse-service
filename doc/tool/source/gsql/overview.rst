:original_name: dws_gsql_002.html

.. _dws_gsql_002:

Overview
========

Basic Functions
---------------

-  **Connect to the database**: Use the gsql client to remotely connect to the GaussDB(DWS) database.

   .. note::

      If the gsql client is used to connect to a database, the connection timeout period will be 5 minutes. If the database has not correctly set up a connection and authenticated the identity of the client within this period, gsql will time out and exit.

      To resolve this problem, see :ref:`Troubleshooting <en-us_topic_0000001860198837>`.

-  **Run SQL statements**: Interactively entered SQL statements and specified SQL statements in a file can be run.
-  **Run meta-commands**: Meta-commands help the administrator view database object information, query cache information, format SQL output, and connect to a new database. For details about meta-commands, see :ref:`Meta-Command Reference <en-us_topic_0000001813598728>`.

Advanced Features
-----------------

:ref:`Table 1 <en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_tafe4ef1072824d3c9d79cb8346c45e6c>` lists the advanced features of gsql.

.. _en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_tafe4ef1072824d3c9d79cb8346c45e6c:

.. table:: **Table 1** Advanced features of gsql

   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Feature                           | Description                                                                                                                                                                                                                                                                                                                                                                             |
   +===================================+=========================================================================================================================================================================================================================================================================================================================================================================================+
   | Variable                          | gsql provides a variable feature that is similar to the **shell** command of Linux. The following **\\set** meta-command of gsql can be used to set a variable:                                                                                                                                                                                                                         |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | .. code-block::                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |    \set varname value                                                                                                                                                                                                                                                                                                                                                                   |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | To delete a variable, run the following command:                                                                                                                                                                                                                                                                                                                                        |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | .. code-block::                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |    \unset varname                                                                                                                                                                                                                                                                                                                                                                       |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                               |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |    -  A variable is a key-value pair. The value length is determined by the special variable **VAR_MAX_LENGTH**. For details, see :ref:`Table 2 <en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_tb7d0ee8c8bbf4c2eb1f55dbe3ad2d5ee>`.                                                                                                           |
   |                                   |    -  Variable names must consist of case-sensitive letters (including non-Latin letters), digits, and underscores (_).                                                                                                                                                                                                                                                                 |
   |                                   |    -  If the **\\set** *varname* meta-command (without the second parameter) is used, the variable is set without a value specified.                                                                                                                                                                                                                                                    |
   |                                   |    -  If the **\\set** meta-command without parameters is used, values of all variables are displayed.                                                                                                                                                                                                                                                                                  |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | For details about variable examples and descriptions, see :ref:`Variable <en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_lc8477a0a74144ad1822fa622fe2f1136>`.                                                                                                                                                                                  |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SQL substitution                  | Common SQL statements can be set to variables using the variable feature of gsql to simplify operations.                                                                                                                                                                                                                                                                                |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | For details about SQL substitution examples and descriptions, see :ref:`SQL substitution <en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_l1817326f33bb4f84acfc1b7c840d1fed>`.                                                                                                                                                                  |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Customized prompt                 | Prompts of gsql can be customized. Prompts can be modified by changing the reserved variables of gsql: *PROMPT1*, *PROMPT2*, and *PROMPT3*.                                                                                                                                                                                                                                             |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | These variables can be set to customized values or the values predefined by gsql. For details, see :ref:`Prompt <en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_laa92fb04b10c4ba18fd3cb8b192e5756>`.                                                                                                                                           |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Client operation history record   | gsql records client operation history. This function is enabled by specifying the **-r** parameter when a client is connected. The number of historical records can be set using the **\\set** command. For example, **\\set HISTSIZE 50** indicates that the number of historical records is set to **50**. **\\set HISTSIZE 0** indicates that the operation history is not recorded. |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                               |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |    -  The default number of historical records is **32**. The maximum number of historical records is **500**. If interactively entered SQL statements contain Chinese characters, only the UTF-8 encoding environment is supported.                                                                                                                                                    |
   |                                   |    -  For security reasons, the records containing sensitive words, such as **PASSWORD** and **IDENTIFIED**, are regarded sensitive and not recorded in historical information. This indicates that you cannot view these records in command output histories.                                                                                                                          |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_lc8477a0a74144ad1822fa622fe2f1136:

   Variable

   To set a variable, run the **\\set** meta-command of gsql. For example, to set variable *foo* to **bar**, run the following command:

   ::

      \set foo bar

   To quote the value of a variable, add a colon (:) before the variable. For example, to view the value of variable *foo*, run the following command:

   ::

      \echo :foo
      bar

   This variable quotation method is suitable for regular SQL statements and meta-commands.

   When the CLI parameter **--dynamic-param** (for details, see :ref:`Table 1 <en-us_topic_0000001813438884__en-us_topic_0000001813438800_en-us_topic_0000001145410531_td295af60028d45cc9362b2f0f81eba40>`) is used or the special variable **DYNAMIC_PARAM_ENABLE** (for details, see :ref:`Table 2 <en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_tb7d0ee8c8bbf4c2eb1f55dbe3ad2d5ee>`) is set to **true**, you can execute the SQL statement to set the variable. The variable name is the column name in the SQL execution result and can be referenced using **${}**. For example:

   ::

      \set DYNAMIC_PARAM_ENABLE true
      SELECT 'Jack' AS "Name";
       Name
      ------
       Jack
      (1 row)

      \echo ${Name}
      Jack

   In the preceding example, the SELECT statement is used to set the **Name** variable, and the **${}** referencing method is used to obtain the value of the **Name** variable. In this example, the special variable **DYNAMIC_PARAM_ENABLE** controls this function. You can also use the CLI parameter **--dynamic-param** to control this function, for example, **gsql -d postgres -p 25308 --dynamic-param -r**.

   .. note::

      -  Do not set variables when the SQL statement execution fails.
      -  If the SQL statement execution result is empty, set the column name as a variable and assign it with an empty string.
      -  If the SQL statement execution result is a record, set the column name as a variable and assign it with the corresponding string.
      -  If the SQL statement execution result contains multiple records, set the column name as a variable concatenated by specific characters, and then assign the value to the variable. The special variable **RESULT_DELIMITER** (for details, see :ref:`Table 2 <en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_tb7d0ee8c8bbf4c2eb1f55dbe3ad2d5ee>`) determines the specific character. The default delimiter is a comma (,).

   Examples of setting variables by executing SQL statements:

   ::

      \set DYNAMIC_PARAM_ENABLE true
      CREATE TABLE student (id INT, name VARCHAR(32)) DISTRIBUTE BY HASH(id);
      CREATE TABLE
      INSERT INTO student VALUES (1, 'Jack'), (2, 'Tom'), (3, 'Jerry');
      INSERT 0 3
      -- Do not set variables when the SQL statement execution fails.
      SELECT id, name FROM student ORDER BY idi;
      ERROR:  column "idi" does not exist
      LINE 1: SELECT id, name FROM student ORDER BY idi;
                                                    ^
      \echo ${id} ${name}
      ${id} ${name}

      -- If the execution result contains multiple records, use specific characters to concatenate the values.
      SELECT id, name FROM student ORDER BY id;
       id | name
      ----+-------
        1 | Jack
        2 | Tom
        3 | Jerry
      (3 rows)

      \echo ${id} ${name}
      1,2,3 Jack,Tom,Jerry

      -- If the execution result contains only one record, execute the following statement to set the variable:
      SELECT id, name FROM student where id = 1;
       id | name
      ----+------
        1 | Jack
      (1 row)

      \echo ${id} ${name}
      1 Jack

      -- If the execution result is empty, assign the variable with an empty string as follows:
      SELECT id, name FROM student where id = 4;
       id | name
      ----+------
      (0 rows)

      \echo ${id} ${name}



   gsql pre-defines some special variables and plans the values of these variables. To ensure compatibility with later versions, do not use these variables for other purposes. For details about all special variables, see :ref:`Table 2 <en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_tb7d0ee8c8bbf4c2eb1f55dbe3ad2d5ee>`.

   .. note::

      -  All the special variables consist of uppercase letters, digits, and underscores (_).
      -  To view the default value of a special variable, run the **\\echo :**\ *varname* meta-command, for example, **\\echo :**\ *DBNAME*.

   .. _en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_tb7d0ee8c8bbf4c2eb1f55dbe3ad2d5ee:

   .. table:: **Table 2** Setting special variables

      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Variable                       | Setting Method                                                    | Description                                                                                                                                                                                                                                                                                                                                                                |
      +================================+===================================================================+============================================================================================================================================================================================================================================================================================================================================================================+
      | DBNAME                         | .. code-block::                                                   | Specifies the name of a connected database. This variable is set again when a database is connected.                                                                                                                                                                                                                                                                       |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |    \set DBNAME dbname                                             |                                                                                                                                                                                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ECHO                           | .. code-block::                                                   | -  If this variable is set to **all**, only the query information is displayed. This has the same effect as specifying the **-a** parameter when gsql is used to connect to a database.                                                                                                                                                                                    |
      |                                |                                                                   | -  If this variable is set to **queries**, the command line and query information are displayed. This has the same effect as specifying the **-e** parameter when gsql is used to connect to a database.                                                                                                                                                                   |
      |                                |    \set ECHO all | queries                                        |                                                                                                                                                                                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ECHO_HIDDEN                    | .. code-block::                                                   | When a meta-command (such as **\\dg**) is used to query database information, the value of this variable determines the query behavior.                                                                                                                                                                                                                                    |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |    \set ECHO_HIDDEN  on | off | noexec                            | -  If this variable is set to **on**, the query statements that are called by the meta-command are displayed, and then the query result is displayed. This has the same effect as specifying the **-E** parameter when gsql is used to connect to a database.                                                                                                              |
      |                                |                                                                   | -  If this variable is set to **off**, only the query result is displayed.                                                                                                                                                                                                                                                                                                 |
      |                                |                                                                   | -  If this variable is set to **noexec**, only the query information is displayed, and the query is not run.                                                                                                                                                                                                                                                               |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ENCODING                       | .. code-block::                                                   | Specifies the character set encoding of the current client.                                                                                                                                                                                                                                                                                                                |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |    \set ENCODING   encoding                                       |                                                                                                                                                                                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | FETCH_COUNT                    | .. code-block::                                                   | -  If the value is an integer greater than **0**, for example, *n*, *n* lines will be selected from the result set to the cache and displayed on the screen when the **SELECT** statement is run.                                                                                                                                                                          |
      |                                |                                                                   | -  If this variable is not set or set to a value less than or equal to **0**, all results are selected at a time to the cache when the **SELECT** statement is run.                                                                                                                                                                                                        |
      |                                |    \set FETCH_COUNT variable                                      |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                  |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   |    Setting this variable to a proper value reduces memory usage. Generally, values from 100 to 1000 are proper.                                                                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | HISTCONTROL                    | .. code-block::                                                   | -  **ignorespace**: A line started with a space is not written to the historical record.                                                                                                                                                                                                                                                                                   |
      |                                |                                                                   | -  **ignoredups**: A line that exists in the historical record is not written to the historical record.                                                                                                                                                                                                                                                                    |
      |                                |    \set HISTCONTROL  ignorespace | ignoredups | ignoreboth | none | -  **ignoreboth**, **none**, or other values: All the lines read in interaction mode are saved in the historical record.                                                                                                                                                                                                                                                   |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   |    .. note::                                                                                                                                                                                                                                                                                                                                                               |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   |       **none** indicates that **HISTCONTROL** is not set.                                                                                                                                                                                                                                                                                                                  |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | HISTFILE                       | .. code-block::                                                   | Specifies the file for storing historical records. The default value is **~/.bash_history**.                                                                                                                                                                                                                                                                               |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |    \set HISTFILE filename                                         |                                                                                                                                                                                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | HISTSIZE                       | .. code-block::                                                   | Specifies the number of commands in the history command. The default value is **500**.                                                                                                                                                                                                                                                                                     |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |    \set HISTSIZE size                                             |                                                                                                                                                                                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | HOST                           | .. code-block::                                                   | Specifies the name of a connected host.                                                                                                                                                                                                                                                                                                                                    |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |    \set HOST hostname                                             |                                                                                                                                                                                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | IGNOREEOF                      | .. code-block::                                                   | -  If this variable is set to a number, for example, **10**, the first nine EOF characters (generally **Ctrl**\ +\ **C**) entered in gsql are neglected and the gsql program exits when the tenth **Ctrl**\ +\ **C** is entered.                                                                                                                                           |
      |                                |                                                                   | -  If this variable is set to a non-numeric value, the default value is **10**.                                                                                                                                                                                                                                                                                            |
      |                                |    \set IGNOREEOF variable                                        | -  If this variable is deleted, gsql exits when an EOF is entered.                                                                                                                                                                                                                                                                                                         |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | LASTOID                        | .. code-block::                                                   | Specifies the last OID, which is the value returned by an **INSERT** or **lo_import** command. This variable is valid only before the output of the next SQL statement is displayed.                                                                                                                                                                                       |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |    \set LASTOID oid                                               |                                                                                                                                                                                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ON_ERROR_ROLLBACK              | .. code-block::                                                   | -  If the value is **on**, an error that may occur in a statement in a transaction block is ignored and the transaction continues.                                                                                                                                                                                                                                         |
      |                                |                                                                   | -  If the value is **interactive**, the error is ignored only in an interactive session.                                                                                                                                                                                                                                                                                   |
      |                                |    \set  ON_ERROR_ROLLBACK  on | interactive | off                | -  If the value is **off** (the default value), the error triggers the rollback of the transaction block. In **on_error_rollback-on** mode, a **SAVEPOINT** is set before each statement of a transaction block, and an error triggers the rollback of the transaction block.                                                                                              |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ON_ERROR_STOP                  | .. code-block::                                                   | -  **on**: specifies that the execution stops if an error occurs. In interactive mode, gsql returns the output of executed commands immediately.                                                                                                                                                                                                                           |
      |                                |                                                                   | -  **off** (default value): specifies that an error, if occurring during the execution, is ignored, and the execution continues.                                                                                                                                                                                                                                           |
      |                                |    \set ON_ERROR_STOP on | off                                    |                                                                                                                                                                                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | PORT                           | .. code-block::                                                   | Specifies the port number of a connected database.                                                                                                                                                                                                                                                                                                                         |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |    \set PORT port                                                 |                                                                                                                                                                                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | USER                           | .. code-block::                                                   | Specifies the connected database user.                                                                                                                                                                                                                                                                                                                                     |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |    \set USER username                                             |                                                                                                                                                                                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | VERBOSITY                      | .. code-block::                                                   | This variable can be set to **terse**, **default**, or **verbose** to control redundant lines of error reports.                                                                                                                                                                                                                                                            |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |    \set VERBOSITY   terse | default | verbose                     | -  **terse**: Only critical and major error texts and text locations are returned (which is suitable for single-line error information).                                                                                                                                                                                                                                   |
      |                                |                                                                   | -  **default**: Critical and major error texts and text locations, error details, and error messages (possibly involving multiple lines) are all returned.                                                                                                                                                                                                                 |
      |                                |                                                                   | -  **verbose**: All error information is returned.                                                                                                                                                                                                                                                                                                                         |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | VAR_NOT_FOUND                  | \\set VAR_NOT_FOUND default \| null \| error                      | You can set this parameter to **default**, **null**, or **error** to control the processing mode when the referenced variable does not exist.                                                                                                                                                                                                                              |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | -  **default**: Do not replace the variable and retain the original character string.                                                                                                                                                                                                                                                                                      |
      |                                |                                                                   | -  **null**: Replace the original character string with an empty character string.                                                                                                                                                                                                                                                                                         |
      |                                |                                                                   | -  **error**: Output error information and retain the original character string.                                                                                                                                                                                                                                                                                           |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | VAR_MAX_LENGTH                 | \\set VAR_MAX_LENGTH *variable*                                   | Specifies the variable value length. The default value is 4096. If the length of a variable value exceeds the specified parameter value, the variable value is truncated and an alarm is generated.                                                                                                                                                                        |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ERROR_LEVEL                    | \\set ERROR_LEVEL transaction \| statement                        | Indicates whether a transaction or statement is successful or not. Value options: **transaction** or **statement**. Default value: **transaction**                                                                                                                                                                                                                         |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | -  **statement**: ERROR records whether the previous SQL statement is executed successfully.                                                                                                                                                                                                                                                                               |
      |                                |                                                                   | -  **transaction**: ERROR records whether the previous SQL statement is successfully executed or whether an error occurs during the execution of the previous transaction.                                                                                                                                                                                                 |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ERROR                          | \\set ERROR true \| false                                         | Indicates whether the previous SQL statement is successfully executed or whether an error occurs during the execution of the previous transaction. **false**: succeeded. **true**: failed. default value: **false** The setting can be updated by executing SQL statements. You are not advised to manually set this parameter.                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | LAST_ERROR_SQLSTATE            | \\set LAST_ERROR_SQLSTATE state                                   | Error code of the previously failed SQL statement execution. The default value is **00000**. The setting can be updated by executing SQL statements. You are not advised to manually set this parameter.                                                                                                                                                                   |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | LAST_ERROR_MESSAGE             | \\set LAST_ERROR_MESSAGE message                                  | Error message of the previously failed SQL statement execution. The default value is an empty string. The setting can be updated by executing SQL statements. You are not advised to manually set this parameter.                                                                                                                                                          |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | ROW_COUNT                      | \\set ROW_COUNT count                                             | -  If ERROR_LEVEL is set to **statement**, this parameter indicates the number of rows returned after the previous SQL statement is executed or the number of affected rows.                                                                                                                                                                                               |
      |                                |                                                                   | -  If ERROR_LEVEL is set to **transaction** and an internal error occurs when a transaction ends, this parameter indicates the number of rows returned by the last SQL statement of the transaction or the number of affected rows. Otherwise, this parameter indicates the number of rows returned by the last SQL statement or the number of affected rows.              |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | If the SQL statement fails to be executed, set this parameter to **0**. The default value is **0**. The setting can be updated by executing SQL statements. You are not advised to manually set this parameter.                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | SQLSTATE                       | \\set SQLSTATE state                                              | -  If ERROR_LEVEL is set to **statement**, this parameter indicates the status code of the previous SQL statement.                                                                                                                                                                                                                                                         |
      |                                |                                                                   | -  If ERROR_LEVEL is set to **transaction** and an internal error occurs when a transaction ends, this parameter indicates the status code of the last SQL statement in the transaction. Otherwise, this parameter indicates the status code of the previous SQL statement.                                                                                                |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | The default value is **00000**. The setting can be updated by executing SQL statements. You are not advised to manually set this parameter.                                                                                                                                                                                                                                |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | LAST_SYS_CODE                  | \\set LAST_SYS_CODE code                                          | Returned value of the previous system command execution. The default value is **0**. The setting can be updated by using the meta-command **\\!** to run the system command. You are not advised to manually set this parameter.                                                                                                                                           |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DYNAMIC_PARAM_ENABLE           | \\set DYNAMIC_PARAM_ENABLE true \| false                          | Controls the generation of variables and the variable referencing method **${}** during SQL statement execution. The default value is **false**.                                                                                                                                                                                                                           |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | -  **true**: Generate variables when executing SQL statements, and support the **${}** variable referencing method.                                                                                                                                                                                                                                                        |
      |                                |                                                                   | -  **false**: Do not generate variables when executing SQL statements, and the **${}** variable referencing method is not supported either.                                                                                                                                                                                                                                |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | CONVERT_QUOTE_IN_DYNAMIC_PARAM | \\set CONVERT_QUOTE_IN_DYNAMIC_PARAM true \| false                | Specifies whether to escape single quotation marks, double quotation marks, and backslashes during dynamic variable parsing. The default value is **true**.                                                                                                                                                                                                                |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | -  **true**: Indicates that during dynamic variable parsing, single quotation marks, double quotation marks, and backslashes (\\) need to be escaped, and SQL substitution automatically escapes quotation marks and backslashes in variables.                                                                                                                             |
      |                                |                                                                   | -  **false**: Indicates that during dynamic variable parsing single quotation marks, double quotation marks, and backslashes (\\) do not need to be escaped. SQL substitution does not process strings in variables. You need to manually escape them as needed.                                                                                                           |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | For details about the usage example, see :ref:`CONVERT_QUOTE_IN_DYNAMIC... <en-us_topic_0000001813599036__en-us_topic_0000001813438992_li148738487919>`.                                                                                                                                                                                                                   |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | RESULT_DELIMITER               | \\set RESULT_DELIMITER delimiter                                  | Controls the delimiter used for concatenating multiple records when variables are generated during SQL statement execution. The default delimiter is comma (,).                                                                                                                                                                                                            |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | COMPARE_STRATEGY               | \\set COMPARE_STRATEGY default \| natural \| equal                | Used to specify the value comparison policy of the **\\if** expression. The default value is **default**.                                                                                                                                                                                                                                                                  |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | -  **default**: Specifies the default comparison policy. Only strings or numbers can be compared, and strings cannot be compared with numbers. Parameters inside single quotation marks (') are identified as strings, and parameters outside single quotation marks (') are identified as numbers.                                                                        |
      |                                |                                                                   | -  **natural**: The default comparison policy is supported, and parameters that contain dynamic variables are identified as strings. When one side of the comparison operator is a number, try to convert the other side to a number, and then compare the numbers on both sides. If the conversion fails, an error is reported and the comparison result is false.        |
      |                                |                                                                   | -  **equal**: Only the equality comparison is supported. The comparison is performed based on strings.                                                                                                                                                                                                                                                                     |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | For details, see :ref:`\if conditional block comparison rules and examples <en-us_topic_0000001813598728__en-us_topic_0000001813598620_li928310446513>`.                                                                                                                                                                                                                   |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | COMMAND_ERROR_STOP             | \\set COMMAND_ERROR_STOP on \| off                                | Determines whether to report the error and stop executing the meta-command when an error occurs during meta-command execution. By default, the meta-command execution is not stopped.                                                                                                                                                                                      |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | For details, see the :ref:`example of using COMMAND_ERROR_STOP <en-us_topic_0000001813599036__en-us_topic_0000001813438992_li12508192661216>`.                                                                                                                                                                                                                             |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | INCOMPLETE_QUERY_ERROR         | \\set INCOMPLETE_QUERY_ERROR true \| false                        | Indicates whether incomplete SQL statements are checked before process control meta-commands, such as **\\if** **\\goto** and **\\for**, are executed. The default value is **false**.                                                                                                                                                                                     |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | If a statement contains the incomplete characters, such as **(**,\ **'**,\ **"**,and **$$**, or does not end with a semicolon (;), an error is reported and the system exits. For details about the usage example, see :ref:`An example of using the special variable INCOMPLETE_QUERY_ERROR <en-us_topic_0000001813599036__en-us_topic_0000001813438992_li189138562165>`. |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | INCOMPLETE_QUERY_ERROR_CODE    | \\set INCOMPLETE_QUERY_ERROR_CODE 12                              | If **INCOMPLETE_QUERY_ERROR** is set to **true** and an incomplete statement is detected, an error is reported and the statement exits. You can set **INCOMPLETE_QUERY_ERROR_CODE** to configure the exit code. The default value is **-1**.                                                                                                                               |
      |                                |                                                                   |                                                                                                                                                                                                                                                                                                                                                                            |
      |                                |                                                                   | For details about the usage example, see :ref:`An example of using the special variable INCOMPLETE_QUERY_ERROR_CODE <en-us_topic_0000001813599036__en-us_topic_0000001813438992_li562653084115>`.                                                                                                                                                                          |
      +--------------------------------+-------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   -  The following is an example of using the special variables ERROR_LEVEL and ERROR:

   When ERROR_LEVEL is set to **statement**, ERROR records whether the previous SQL statement is executed successfully. In the following example, when an SQL execution error occurs in a transaction and the transaction ends, the value of ERROR is **false**. In this case, ERROR only records whether the previous SQL statement ends successfully.

   ::

      \set ERROR_LEVEL statement
      begin;
      BEGIN
      select 1 as ;
      ERROR:  syntax error at or near ";"
      LINE 1: select 1 as ;
                          ^
      end;
      ROLLBACK
      \echo :ERROR
      false

   When ERROR_LEVEL is set to **transaction**, ERROR can be used to capture SQL execution errors in a transaction. In the following example, when an SQL execution error occurs in a transaction and the transaction ends, the value of ERROR is **true**.

   ::

      \set ERROR_LEVEL transaction
      begin;
      BEGIN
      select 1 as ;
      ERROR:  syntax error at or near ";"
      LINE 1: select 1 as ;
                          ^
      end;
      ROLLBACK
      \echo :ERROR
      true

   -  .. _en-us_topic_0000001813599036__en-us_topic_0000001813438992_li12508192661216:

      The following is an example of using the special variable **COMMAND_ERROR_STOP**:

   When **COMMAND_ERROR_STOP** is set to **on** and an error occurs during the meta-command execution, the error is reported and the meta-command execution is stopped. When this function is enabled, the execution error of the meta-command can be effectively detected.

   When **COMMAND_ERROR_STOP** is set to **off** and an error occurs during the meta-command execution, related information is printed and the script continues to be executed.

   ::

      \set COMMAND_ERROR_STOP on
      \i /home/omm/copy_data.sql

      select id, name from student;

   When **COMMAND_ERROR_STOP** in the preceding script is set to **on**, an error message is displayed after the error is reported, and the script execution is stopped.

   ::

      gsql:test.sql:2: /home/omm/copy_data.sql: Not a directory

   When **COMMAND_ERROR_STOP** is set to **off**, an error message is displayed after the error is reported, and the **SELECT** statement continues to be executed.

   ::

      gsql:test.sql:2: /home/omm/copy_data.sql: Not a directory
       id | name
      ----+------
        1 | Jack
      (1 row)

   -  .. _en-us_topic_0000001813599036__en-us_topic_0000001813438992_li189138562165:

      An example of using the special variable **INCOMPLETE_QUERY_ERROR**:

   When **INCOMPLETE_QUERY_ERROR** is set to **true**, incomplete SQL statements are detected before process control meta-commands such as \\if \\goto \\for, an error is reported, and the system exits.

   .. note::

      The following types of incomplete statements are detected:

      -  The SQL statement does not end with a semicolon (;).
      -  The brackets in the SQL statement do not match.
      -  The single quotation marks in the SQL statement do not match.
      -  The double quotation marks in the SQL statement do not match.
      -  **$$** in the SQL statement does not match.

      Detection method:

      -  If the command does not end with a semicolon (;) or if the brackets are mismatched, the system will analyze the character string before executing process control meta-commands such as **\\if**, **\\goto**, and **\\for**. Additionally, C-style comments (**/\* \*/**) and any whitespace characters will be removed. If any characters follow the escape sequences [\\f\\n\\r\\t\\v] (including spaces, tabs, and form feeds), the system will treat the statement as incomplete. In this case, an error will be reported, and the system exits.
      -  Single quotation marks, double quotation marks, and $$ do not match. If the \\if, \\elif, \\else, \\endif, \\goto, \\label, \\for, \\loop, \\exit-for, and \\end-for meta-commands are detected, an error is reported and the system exits.

   The following is an example of SQL statements that do not end with a semicolon (;):

   ::

      \set INCOMPLETE_QUERY_ERROR true
      select 1 as id
      \if ${ERROR}
          \echo 'find error'
          \q 12
      \endif

   In the preceding example, an SQL statement that does not end with a semicolon (;) exists before the \\if meta-command. In this case, an error is reported when the \\if command is executed.

   ::

      $ gsql -X -d postgres -p 13500 --dynamic-param -a -f test.sql
      \set INCOMPLETE_QUERY_ERROR true
      select 1 as id
      \if ${ERROR}
      gsql:test.sql:3: ERROR: An incomplete SQL statement exists before the \if command.
      gsql:test.sql:3: DETAIL: The SQL statement may not end with a semicolon. Please check.

   The following is an example of SQL statements in which the brackets do not match:

   ::

      \set INCOMPLETE_QUERY_ERROR true
      insert into student values (1, 'jack';
      \if ${ERROR}
          \echo 'find error'
          \q 12
      \endif

   In the preceding example, an SQL statement in which the brackets do not match exists before the \\if meta-command. In this case, an error is reported when the \\if command is executed.

   ::

      $ gsql -X -d postgres -p 13500 --dynamic-param -a -f test.sql
      \set INCOMPLETE_QUERY_ERROR true
      insert into student values (1, 'jack';
      \if ${ERROR}
      gsql:test.sql:3: ERROR: An incomplete SQL statement exists before the \if command.
      gsql:test.sql:3: DETAIL: There may be an unmatched ( in the SQL statement. Please check.

   The following is an example of SQL statements in which the single quotation marks do not match:

   ::

      \set INCOMPLETE_QUERY_ERROR true
      select 'jack as name;
      \if ${ERROR}
          \echo 'find error'
          \q 12
      \endif

   In the preceding example, an SQL statement in which the single quotation marks do not match exists before the \\if meta-command. In this case, an error is reported when the \\if command is executed.

   ::

      $ gsql -X -d postgres -p 13500 --dynamic-param -a -f test.sql
      \set INCOMPLETE_QUERY_ERROR true
      select 'jack as name;
      \if ${ERROR}
      gsql:test.sql:3: ERROR: An incomplete SQL statement exists before the \if command.
      gsql:test.sql:3: DETAIL: There may be an unmatched ' in the SQL statement. Please check.

   The following is an example of SQL statements in which the double quotation marks do not match:

   ::

      \set INCOMPLETE_QUERY_ERROR true
      select 10001 as "ID;
      \if ${ERROR}
          \echo 'find error'
          \q 12
      \endif

   In the preceding example, an SQL statement in which the double-brackets are incomplete exists before the **\\if** meta-command. In this case, an error is reported and the system exits when the **\\if** command is executed.

   ::

      $ gsql -X -d postgres -p 13500 --dynamic-param -a -f test.sql
      \set INCOMPLETE_QUERY_ERROR true
      select 10001 as "ID;
      \if ${ERROR}
      gsql:test.sql:3: ERROR: An incomplete SQL statement exists before the \if command.
      gsql:test.sql:3: DETAIL: There may be an unmatched " in the SQL statement. Please check.

   The following is an example of SQL statements in which $$ does not match:

   ::

      \set INCOMPLETE_QUERY_ERROR true
      create or replace function gsql_dollar_quote_test()
      returns integer
      as
      $BODY$
      declare
          query text;
          dest text;
      begin
          query := 'select count(*) from pg_class';
          execute immediate query into dest;
      end;
      $BODY
      language 'plpgsql' not fenced;
      call gsql_dollar_quote_test();
      \if ${ERROR}
          \echo 'find error'
          \q 12
      \endif

   In the preceding example, an SQL statement in which $$ does not match exists before the **\\if** meta-command. In this case, an error is reported and the system exits when the **\\if** command is executed.

   ::

      $ gsql -X -d postgres -p 13500  --dynamic-param -a -f test.sql
      \set INCOMPLETE_QUERY_ERROR true
      create or replace function gsql_dollar_quote_test()
      returns integer
      as
      $BODY$
      declare
          query text;
          dest text;
      begin
          query := 'select count(*) from pg_class';
          execute immediate query into dest;
      end;
      $BODY
      language 'plpgsql' not fenced;
      call gsql_dollar_quote_test();
      \if ${ERROR}
      gsql:test.sql:16: ERROR: An incomplete SQL statement exists before the \if command.
      gsql:test.sql:16: DETAIL: There may be an unmatched $$ in the SQL statement. Please check.

   -  .. _en-us_topic_0000001813599036__en-us_topic_0000001813438992_li562653084115:

      An example of using the special variable **INCOMPLETE_QUERY_ERROR_CODE**:

   If **INCOMPLETE_QUERY_ERROR** is **true**, you can set **INCOMPLETE_QUERY_ERROR_CODE** to configure the exit code when an incomplete statement is detected. The following is an example:

   ::

      \set INCOMPLETE_QUERY_ERROR true
      \set INCOMPLETE_QUERY_ERROR_CODE 20
      insert into student values (1, 'jack';
      \if ${ERROR}
          \echo 'find error'
          \q 12
      \endif

   In the preceding test case, if **INCOMPLETE_QUERY_ERROR_CODE** is set to **20**, the \\ifcommand will exit when an incomplete statement is detected, and the exit code is the value of **INCOMPLETE_QUERY_ERROR_CODE**.

   ::

      $ gsql -X -d postgres -p 13500  --dynamic-param -a -f test.sql
      \set INCOMPLETE_QUERY_ERROR true
      \set INCOMPLETE_QUERY_ERROR_CODE 20
      insert into student values (1, 'jack';
      \if ${ERROR}
      gsql:test.sql:4: ERROR: An incomplete SQL statement exists before the \if command.
      gsql:test.sql:4: DETAIL: There may be an unmatched ( in the SQL statement. Please check.
      $ echo $?
      20

-  .. _en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_l1817326f33bb4f84acfc1b7c840d1fed:

   SQL substitution

   gsql, like a parameter of a meta-command, provides a key feature that enables you to substitute a standard SQL statement for a gsql variable. gsql also provides a new alias or identifier for the variable. To replace the value of a variable using the SQL substitution method, add a colon (:) in front of the variable. For example:

   ::

      \set foo 'HR.areaS'
      select * from :foo;
       area_id |       area_name
      ---------+------------------------
             4 | Iron
             3 | Desert
             1 | Wood
             2 | Lake
      (4 rows)

   The above command queries the **HR.areaS** table.

   .. important::

      The value of a variable is copied character by character, and even an asymmetric quote mark or backslash (\\) is copied. Therefore, the input content must be meaningful.

   -  .. _en-us_topic_0000001813599036__en-us_topic_0000001813438992_li148738487919:

      The following is an example of using the special variable **CONVERT_QUOTE_IN_DYNAMIC_PARAM**:

   If **CONVERT_QUOTE_IN_DYNAMIC_PARAM** is set to **true**, quotation marks and backslashes in variables are automatically escaped during SQL substitution.

   ::

       \set DYNAMIC_PARAM_ENABLE true
       \set CONVERT_QUOTE_IN_DYNAMIC_PARAM true
       select '""abc''''\\' as "SpecialCharacters";
         test
      -----------
       ""abc''\\
      (1 row)

      -- Single quotation marks are escaped, but still displayed in the result.
       select '${SpecialCharacters}' as "test";
         test
      -----------
       ""abc''\\
      (1 row)

      -- Single quotation marks and backslashes are escaped, but still displayed in the result.
       select E'${SpecialCharacters}' as "test";
         test
      -----------
       ""abc''\\
      (1 row)

      -- Double quotation marks are escaped, but still displayed in the result.
      -- The column name contains characters other than letters, digits, and underscores (_). Therefore, an error occurred.
       select 'test' as "${SpecialCharacters}";
      error while saving the value of ""abc''\\, please check the column name which can only contain upper and lower case letters, numbers and '_'.
       ""abc''\\
      -----------
       test
      (1 row)

   When **CONVERT_QUOTE_IN_DYNAMIC_PARAM** is set to false, strings in the variable are not processed during SQL substitution. You need to manually escape the strings as needed

   .. note::

      You are advised to use the default value **true**.

      During SQL substitution, single quotation marks need to be escaped in **''**, single quotation marks and backslashes need to be escaped in **E''**, and double quotation marks need to be escaped in **""**. Quotation marks and backslashes need to be handled based on variable positions. This makes the variable logics in SQL substitution complex and error-prone.

   ::

       \set DYNAMIC_PARAM_ENABLE true
       \set CONVERT_QUOTE_IN_DYNAMIC_PARAM false
       select '""abc''''\\' as "SpecialCharacters";
         test
      -----------
       ""abc''\\
      (1 row)

      -- Single quotation marks are not escaped. The result contains only one single quotation mark.
       select '${SpecialCharacters}' as "test";
         test
      ----------
       ""abc'\\
      (1 row)

      -- Single quotation marks and backslashes are not escaped. The result contains only one single quotation mark and one backslash.
       select E'${SpecialCharacters}' as "test";
        test
      ---------
       ""abc'\
      (1 row)

      -- Double quotation marks are not escaped. The result contains only one double quotation mark.
      -- The column name contains characters other than letters, digits, and underscores (_). Therefore, an error occurred.
       select 'test' as "${SpecialCharacters}";
      error while saving the value of "abc''\\, please check the column name which can only contain upper and lower case letters, numbers and '_'.
       "abc''\\
      ----------
       test
      (1 row)

-  .. _en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_laa92fb04b10c4ba18fd3cb8b192e5756:

   Prompt

   The gsql prompt can be set using the three variables in :ref:`Table 3 <en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_tcbb1ddded09e46228348d96ff1af36e8>`. These variables consist of characters and special escape characters.

   .. _en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_tcbb1ddded09e46228348d96ff1af36e8:

   .. table:: **Table 3** Prompt variables

      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------+
      | Variable              | Description                                                                                                                                                                       | Example                                                   |
      +=======================+===================================================================================================================================================================================+===========================================================+
      | PROMPT1               | Specifies the normal prompt used when gsql requests a new command.                                                                                                                | *PROMPT1* can be used to change the prompt.               |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       | The default value of *PROMPT1* is:                                                                                                                                                | -  Change the prompt to **[local]**:                      |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       | .. code-block::                                                                                                                                                                   |    ::                                                     |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       |    %/%R%#                                                                                                                                                                         |       \set PROMPT1 %M                                     |
      |                       |                                                                                                                                                                                   |       [local:/tmp/gaussdba_mppdb]                         |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       |                                                                                                                                                                                   | -  Change the prompt to **name**:                         |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       |                                                                                                                                                                                   |    ::                                                     |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       |                                                                                                                                                                                   |       \set PROMPT1 name                                   |
      |                       |                                                                                                                                                                                   |       name                                                |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       |                                                                                                                                                                                   | -  Change the prompt to **=**:                            |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       |                                                                                                                                                                                   |    ::                                                     |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       |                                                                                                                                                                                   |       \set PROMPT1 %R                                     |
      |                       |                                                                                                                                                                                   |       =                                                   |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------+
      | PROMPT2               | Specifies the prompt displayed when more command input is expected. For example, it is expected if a command is not terminated with a semicolon (;) or a quote (") is not closed. | *PROMPT2* can be used to display the prompt:              |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       |                                                                                                                                                                                   | ::                                                        |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       |                                                                                                                                                                                   |    \set PROMPT2 TEST                                      |
      |                       |                                                                                                                                                                                   |    select * from HR.areaS TEST;                           |
      |                       |                                                                                                                                                                                   |     area_id |       area_name                             |
      |                       |                                                                                                                                                                                   |    ---------+--------------------                         |
      |                       |                                                                                                                                                                                   |           1 | Wood                                        |
      |                       |                                                                                                                                                                                   |           2 | Lake                                        |
      |                       |                                                                                                                                                                                   |           4 | Iron                                        |
      |                       |                                                                                                                                                                                   |           3 | Desert                                      |
      |                       |                                                                                                                                                                                   |    (4 rows))                                              |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------+
      | PROMPT3               | Specifies the prompt displayed when the **COPY** statement (such as **COPY FROM STDIN**) is run and data input is expected.                                                       | *PROMPT3* can be used to display the **COPY** prompt.     |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       |                                                                                                                                                                                   | ::                                                        |
      |                       |                                                                                                                                                                                   |                                                           |
      |                       |                                                                                                                                                                                   |    \set PROMPT3 '>>>>'                                    |
      |                       |                                                                                                                                                                                   |    copy HR.areaS from STDIN;                              |
      |                       |                                                                                                                                                                                   |    Enter data to be copied followed by a newline.         |
      |                       |                                                                                                                                                                                   |    End with a backslash and a period on a line by itself. |
      |                       |                                                                                                                                                                                   |    >>>>1 aa                                               |
      |                       |                                                                                                                                                                                   |    >>>>2 bb                                               |
      |                       |                                                                                                                                                                                   |    >>>>\.                                                 |
      +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------+

   The value of the selected prompt variable is printed literally. However, a value containing a percent sign (%) is replaced by the predefined contents depending on the character following the percent sign (%). For details about the defined substitutions, see :ref:`Table 4 <en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_t2d9953fd189b42ada46e619d59bcc909>`.

   .. _en-us_topic_0000001813599036__en-us_topic_0000001813438992_en-us_topic_0000001098650792_t2d9953fd189b42ada46e619d59bcc909:

   .. table:: **Table 4** Defined substitutions

      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Symbol                            | Description                                                                                                                                                                                                                                                                                                                        |
      +===================================+====================================================================================================================================================================================================================================================================================================================================+
      | %M                                | Specifies the full host name (with domain name). The full name is [local] if the connection is over a Unix domain socket, or [local:/dir/name] if the Unix domain socket is not at the compiled default location.                                                                                                                  |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %m                                | Specifies the host name truncated at the first dot. It is [local] if the connection is over a Unix domain socket.                                                                                                                                                                                                                  |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %>                                | Specifies the number of the port that the host is listening on.                                                                                                                                                                                                                                                                    |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %n                                | Specifies the database session user name.                                                                                                                                                                                                                                                                                          |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %/                                | Specifies the name of the current database.                                                                                                                                                                                                                                                                                        |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %~                                | Is similar to **%/**. However, the output is tilde (~) if the database is your default database.                                                                                                                                                                                                                                   |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %#                                | Uses **#** if the session user is the database administrator. Otherwise, uses **>**.                                                                                                                                                                                                                                               |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %R                                | -  Normally uses **=** for *PROMPT1*, but **^** in single-line mode and **!** if the session is disconnected from the database (which may occur if **\\connect** fails).                                                                                                                                                           |
      |                                   | -  For PROMPT2, the sequence is replaced by a hyphen (-), asterisk (*), single quotation mark ('), double quotation mark ("), or dollar sign ($), depending on whether gsql is waiting for more input, or the query is not terminated, or the query is in the ``/* ... */`` the comment, quotation mark, or dollar sign extension. |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %x                                | Specifies the transaction status.                                                                                                                                                                                                                                                                                                  |
      |                                   |                                                                                                                                                                                                                                                                                                                                    |
      |                                   | -  An empty string when it is not in a transaction block                                                                                                                                                                                                                                                                           |
      |                                   | -  An asterisk (*) when it is in a transaction block                                                                                                                                                                                                                                                                               |
      |                                   | -  An exclamation mark (!) when it is in a failed transaction block                                                                                                                                                                                                                                                                |
      |                                   | -  A question mark (?) when the transaction status is indeterminate (for example, indeterminate due to no connections).                                                                                                                                                                                                            |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %digits                           | Is replaced with the character with the specified byte.                                                                                                                                                                                                                                                                            |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %:name                            | Specifies the value of the *name* variable of gsql.                                                                                                                                                                                                                                                                                |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %command                          | Specifies command output, similar to ordinary "back-tick" ("^") substitution.                                                                                                                                                                                                                                                      |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | %[ . . . %]                       | Prompts can contain terminal control characters which, for example, change the color, background, or style of the prompt text, or change the title of the terminal window. For example:                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                                                                                                                                    |
      |                                   | potgres=> \\set PROMPT1 '%[%033[1;33;40m%]%n@%/%R%[%033[0m%]%#'                                                                                                                                                                                                                                                                    |
      |                                   |                                                                                                                                                                                                                                                                                                                                    |
      |                                   | The output is a boldfaced (1;) yellow-on-black (33;40) prompt on VT100-compatible, color-capable terminals.                                                                                                                                                                                                                        |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Environment Variables
---------------------

.. table:: **Table 5** Environment variables related to gsql

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name                              | Description                                                                                                                                                                                                                                                                                                                                                                     |
   +===================================+=================================================================================================================================================================================================================================================================================================================================================================================+
   | COLUMNS                           | If **\\set columns** is set to **0**, this parameter controls the width of the wrapped format. This width determines whether the width output mode is changed to a vertical bar format in automatic expansion mode.                                                                                                                                                             |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PAGER                             | If the query result cannot be displayed within one page, the query result will be redirected to the command. You can use the **\\pset** command to disable the pager. Typically, the **more** or **less** command is used for viewing the query result page by page. The default value is platform-associated.                                                                  |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                 |
   |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                       |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                 |
   |                                   |    Display of the **less** command is affected by the *LC_CTYPE* environmental variable.                                                                                                                                                                                                                                                                                        |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PSQL_EDITOR                       | The **\\e** and **\\ef** commands use the editor specified by the environment variables. Variables are checked according to the list sequence. The default editor on Unix is vi.                                                                                                                                                                                                |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | EDITOR                            |                                                                                                                                                                                                                                                                                                                                                                                 |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | VISUAL                            |                                                                                                                                                                                                                                                                                                                                                                                 |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PSQL_EDITOR_LINENUMBER_ARG        | When the **\\e** or **\\ef** command is used with a line number parameter, this variable specifies the command-line parameter used to pass the starting line number to the editor. For editors, such as Emacs or vi, this is a plus sign. A space is added behind the value of the variable if whitespace is required between the option name and the line number. For example: |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                 |
   |                                   | .. code-block::                                                                                                                                                                                                                                                                                                                                                                 |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                 |
   |                                   |    PSQL_EDITOR_LINENUMBER_ARG = '+'                                                                                                                                                                                                                                                                                                                                             |
   |                                   |    PSQL_EDITOR_LINENUMBER_ARG='--line '                                                                                                                                                                                                                                                                                                                                         |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                 |
   |                                   | A plus sign (+) is used by default on Unix.                                                                                                                                                                                                                                                                                                                                     |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PSQLRC                            | Specifies the location of the user's .gsqlrc file.                                                                                                                                                                                                                                                                                                                              |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SHELL                             | Has the same effect as the **\\!** command.                                                                                                                                                                                                                                                                                                                                     |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TMPDIR                            | Specifies the directory for storing temporary files. The default value is **/tmp**.                                                                                                                                                                                                                                                                                             |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
