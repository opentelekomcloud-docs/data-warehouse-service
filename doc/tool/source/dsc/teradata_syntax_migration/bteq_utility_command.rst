:original_name: dws_16_0100.html

.. _dws_16_0100:

BTEQ Utility Command
====================

GaussDB(DWS) provides gsql meta-commands that can be used to replace common BTEQ tool commands. The mostly used replacements are as follows:

.QUIT \| .EXIT \| .RETURN
-------------------------

The meta-command **\\q [**\ *value*\ **]** can be used to exit the gsql program, and **value** specifies the exit code. The .QUIT, .EXIT, and .RETURN commands can replace each other with the **\\q** command.

======= ======
Input   Output
======= ======
.QUIT 0 \\q 0
.EXIT   \\q
.RETURN \\q
======= ======

.LABEL and .GOTO
----------------

The Teradata command .LABEL is used to create tags and is usually used together with .GOTO. .GOTO skips all intermediate BTEQ commands and SQL statements, instructs you to reach the specified label position, and performs corresponding restoration processing.

gsql meta-command **\\goto LABEL...** and **\\label LABEL** can be replaced with each other with no constraints.

+---------------------------------------+-----------------------------------+
| Input                                 | Output                            |
+=======================================+===================================+
| .. code-block::                       | .. code-block::                   |
|                                       |                                   |
|    .IF CHECK_PK='' THEN .GOTO NOCHECK |    \if ${CHECK_PK} == ''          |
|    ${CHECK_PK};                       |    \goto NOCHECK                  |
|    .LABEL NOCHECK                     |    \endif                         |
|    .QUIT 0                            |    ${CHECK_PK}                    |
|                                       |    \label NOCHECK                 |
|                                       |    \q 0                           |
+---------------------------------------+-----------------------------------+

.RUN FILE
---------

Executing the SQL request of a specified file can be implemented by the gsql meta-command **\\i**.

+-----------------------------------+-----------------------------------+
| Input                             | Output                            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    .RUN FILE=sampfile             |    \i 'sampfile'                  |
+-----------------------------------+-----------------------------------+

.EXPORT FILE
------------

Exporting the SQL statement execution result to a specified file can be implemented by the gsql meta-command **\\o**.

+-----------------------------------+-----------------------------------+
| Input                             | Output                            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    .EXPORT REPORT FILE=resultfile |    \o 'resultfile'                |
+-----------------------------------+-----------------------------------+

..SET ..END
-----------

To set a variable to specified value, both a single-line command or a multi-line command ended with ..END can be used. You can run the SELECT command to query a specified variable name or run the **\\set** command to convert the variable name.

+-----------------------------------+-----------------------------------+
| Input                             | Output                            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    .SET SEPARATOR ' | '           |    \set SEPARATOR '|'             |
+-----------------------------------+-----------------------------------+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    ..SET NAME                     |    SELECT 'Jack' AS "NAME";       |
|    'Jack'                         |                                   |
|    ..END                          |                                   |
+-----------------------------------+-----------------------------------+

.IF
---

.IF is one of the important process control commands used to execute parts of an input script. It can be a single-branch command, which is used in pairs with the THEN clause. It can also be a multi-branch command. The multi-branch IF structure allows multiple nested layers, but each layer must start with the IF command and end with the ENDIF command.

The flow control meta-commands **\\if...** \\else ... **\\endif** can be used to replace this BTEQ command.

+--------------------------------------+-----------------------------------+
| Input                                | Output                            |
+======================================+===================================+
| .. code-block::                      | .. code-block::                   |
|                                      |                                   |
|    .IF ERRORCODE <> 0 THEN .QUIT 12; |    \if ${ERROR} != 'false'        |
|                                      |    \q 12                          |
|                                      |    \endif                         |
+--------------------------------------+-----------------------------------+

..FOR
-----

In the loop control command, when the loop condition is met, the script of the loop body can be executed continuously until .QUIT exits the loop. GaussDB(DWS) provides **\\for...** \\loop ... \\exit-for ... **\\end-for** structure command implements the loop logic.

+----------------------------------------+---------------------------------------------------+
| Input                                  | Output                                            |
+========================================+===================================================+
| .. code-block::                        | .. code-block::                                   |
|                                        |                                                   |
|    ..IF ACTIVITYCOUNT > 0   THEN       |    \if ${ERROR} != 'false'                        |
|    ..FOR                               |    \q 12                                          |
|    SEL SqlStr AS V_SqlStr              |    \endif                                         |
|    FROM ${ ETL_DATA}.TB_DWDATA_UPDATE  |    \if ${ACTIVITYCOUNT} != 0                      |
|    WHERE JobName = '${JOB_NAME}'       |    \for                                           |
|    AND TXDATE = ${ TX_DATE} - 19000000 |    SELECT                                         |
|    ORDER BY ExcuteSeq ASC;             |    SqlStr AS V_SqlStr                             |
|    ..DO                                |    FROM                                           |
|    ${V_SqlStr}                         |    ${ETL_DATA}.TB_DWDATA_UPDATE                   |
|    .IF ERRORCODE<>0 THEN .QUIT 12      |    WHERE                                          |
|    ..END-FOR                           |    JobName = '${JOB_NAME}'                        |
|    ..END-IF                            |    AND TXDATE = to_date( ${TX_DATE} ,'yyyymmdd' ) |
|                                        |    ORDER BY                                       |
|                                        |    ExcuteSeq ASC                                  |
|                                        |    \loop                                          |
|                                        |    ${V_SqlStr} ;                                  |
|                                        |    \if ${ERROR} != 'false'                        |
|                                        |    \q 12                                          |
|                                        |    \endif                                         |
|                                        |    \end-for                                       |
|                                        |    \endif                                         |
+----------------------------------------+---------------------------------------------------+
