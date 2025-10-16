:original_name: dws_06_0001.html

.. _dws_06_0001:

GaussDB(DWS) SQL Overview
=========================

What Is SQL?
------------

SQL is a standard computer language used to control the access to databases and manage data in databases.

SQL provides different statements to enable you to:

-  Query data.
-  Insert, update, and delete rows.
-  Create, replace, modify, and delete objects.
-  Control the access to a database and its objects.
-  Maintain the consistency and integrity of a database.

SQL consists of commands and functions that are used to manage databases and database objects. SQL can also forcibly implement the rules for data types, expressions, and texts. Therefore, section "SQL Reference" describes data types, expressions, functions, and operators in addition to SQL syntax.

Development of SQL Standards
----------------------------

Released SQL standards are as follows:

-  1986: ANSI X3.135-1986, ISO/IEC 9075:1986, SQL-86
-  1989: ANSI X3.135-1989, ISO/IEC 9075:1989, SQL-89
-  1992: ANSI X3.135-1992, ISO/IEC 9075:1992, SQL-92 (SQL2)
-  1999: ISO/IEC 9075:1999, SQL:1999 (SQL3)
-  2003: ISO/IEC 9075:2003, SQL:2003 (SQL4)
-  2011: ISO/IEC 9075:200N, SQL:2011 (SQL5)

Supported SQL Standards
-----------------------

GaussDB(DWS) is compatible with Postgres-XC features and supports the major features of SQL2, SQL3, and SQL4 by default, and some features of SQL5.

Languages Supported by GaussDB(DWS)
-----------------------------------

GaussDB(DWS) supports PL/pgSQL and PL/Java.

SQL Syntax Text Conventions
---------------------------

To better understand the syntax usage, you can refer to the SQL syntax text conventions described as follows:

+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
| Format                     | Description                                                                                                                                  |
+============================+==============================================================================================================================================+
| Uppercase characters       | Indicates that keywords (the part that remains unchanged in a statement and must be consistent with the syntax format) must be in uppercase. |
+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
| Lowercase characters       | Indicates that parameters must be in lowercase.                                                                                              |
+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
| [ ]                        | Indicates optional syntaxes. Indicates that the items in brackets [] are optional.                                                           |
+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
| { }                        | Indicates mandatory syntaxes.                                                                                                                |
+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
| ...                        | Indicates that preceding elements can appear repeatedly.                                                                                     |
+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
| [ x \| y \| ... ]          | Indicates that one item is selected from two or more options or no item is selected.                                                         |
+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
| { x \| y \| ... }          | Indicates that one item is selected from two or more options.                                                                                |
+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
| [x \| y \| ... ] [ ... ]   | Indicates that multiple parameters or no parameter can be selected. If multiple parameters are selected, separate them with spaces.          |
+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
| [ x \| y \| ... ] [ ,... ] | Indicates that multiple parameters or no parameter can be selected. If multiple parameters are selected, separate them with commas (,).      |
+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
| { x \| y \| ... } [ ... ]  | Indicates that at least one parameter can be selected. If multiple parameters are selected, separate them with spaces.                       |
+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
| { x \| y \| ... } [ ,... ] | Indicates that at least one parameter can be selected. If multiple parameters are selected, separate them with commas (,).                   |
+----------------------------+----------------------------------------------------------------------------------------------------------------------------------------------+

SQL Example Description
-----------------------

SQL examples in this manual are developed based on the TPC-DS model. Before you execute the examples, install the TPC-DS benchmark by following the instructions on the official website https://www.tpc.org/tpcds/.
