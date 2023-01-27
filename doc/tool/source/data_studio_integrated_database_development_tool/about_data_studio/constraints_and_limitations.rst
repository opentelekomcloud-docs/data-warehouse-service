:original_name: DWS_DS_12.html

.. _DWS_DS_12:

Constraints and Limitations
===========================

This section describes the constraints and limitations for using Data Studio.

Object Browser Filter Tree
--------------------------

The filter count and filter status are not displayed in the filter tree.

Character Encoding
------------------

If the SQL statement, DDL, object name, or data to be viewed contains Chinese characters, and the OS supports GBK, set the encoding mode to **GBK**. For details, see :ref:`Session Setting <en-us_topic_0000001099153186__en-us_topic_0185264709_section212012207470>`.

Connection Management
---------------------

On the **Advanced** tab of the **New Connection** and **Edit Connection** pages, commas (,) are used as separators in the **include** and **exclude** columns. Therefore, a schema name that contains a comma (,) is not supported.

Database Tables
---------------

-  On the **Index** tab of the table creation wizard, the sequence of the selected columns in the list view cannot be retained after columns are deleted.
-  The message dialog box is displayed only when the Data Studio window is the active window of the current operating system (OS).
-  The limitations of :ref:`editing table data <dws_ds_102>` are as follows:

   -  Expression values cannot be entered on the **Edit Table Data** tab.
   -  Only obtained records can be edited.
   -  When you edit the filter criteria of a table, the search content in the HTML tags, such as **<**, **&**, and **>**, is not highlighted.
   -  In the prompt message, cells containing an ampersand (&) are not displayed, and cells containing two consecutive ampersands (&) are displayed as one &.
   -  The cursor does not move to the new row. You need to click the desired cell.

Function/Procedure
------------------

A function or procedure created in **SQL Terminal** or **Create Function/Procedure** wizard must end with a slash (/). Otherwise, the statement is considered as single query and an error may be reported during execution.

General
-------

-  A maximum of 100 tab pages can be opened at a time in the editing area. The actual number of tab pages displayed depends on the available resources of the host.
-  The name of a database object can contain a maximum of 64 characters (only in TXT format). Database objects include databases, schemas, functions, stored procedures, tables, sequences, constraints, indexes, and views. But there is no limit to the number of characters used in expressions and descriptions of Data Studio.
-  A maximum of 300 **Result** tabs can be opened on the instance that you have logged in to in Data Studio.
-  If large objects are loaded in the **Object Browser** and **Search Object** windows, the object loading may be slow and Data Studio may fail to respond.
-  Adjusting the cell width may cause Data Studio to fail to respond.
-  A maximum of 1000 characters can be displayed in a cell of the table. The excess characters are displayed as ellipsis (...).

   -  If you copy the data from a cell in a table or the **Result** tab to any editor, such as SQL terminal/PLSQL source editor, notepad, or any other external editor application, all data is pasted.
   -  If you copy the data from a cell in a table or the **Result** tab to an editable cell, the cell displays only the first 1000 characters and the excess characters are displayed as **...**.
   -  When the data of a table or the **Result** tab is exported, the exported file contains all the data.

Security
--------

Data Studio validates SSL connection parameters only for the first time of connection. If **Enable SSL** is selected, the same SSL connection parameters are used when a new connection is opened.

.. note::

   -  If **Enable SSL** is not selected during connecting to Data Studio, data is not encrypted by default.
   -  If the security file is damaged during the SSL connection, Data Studio cannot perform any database operations. To resolve this problem, delete the security folder in the corresponding configuration folder and restart Data Studio.

SQL Terminal
------------

-  If a SQL file containing a large number of SQL statements is opened, the error of insufficient memory may occur. For details, see :ref:`Troubleshooting <dws_ds_145>`.
-  Data Studio does not disable the **Auto Suggest** and hyperlink features in the commented text in **SQL Terminal**.
-  The hyperlink feature is not supported if the schema or table name contains spaces or periods (.).
-  The **Auto Suggest** feature is not supported if the object name contains single quotation marks (') or double quotation marks (").
-  Data Studio only supports the basic formatting of simple **SELECT** statements, and may fail to process complex queries.
