:original_name: DWS_DS_003.html

.. _DWS_DS_003:

Constraints and Limitations
===========================

This section describes the constraints and limitations for using Data Studio.

Character Encoding
------------------

If the SQL statement, DDL, object name, or data to be viewed contains Chinese characters, set the character encoding to **GBK** if it is supported by the OS.

Connection Management
---------------------

On the **Advanced** tab of the **New Connection** and **Edit Connection** pages, commas (,) are used as separators in the **include** and **exclude** columns. Therefore, a schema name that contains a comma (,) is not supported.

Database Tables
---------------

-  On the **Index** tab of the table creation wizard, the sequence of the selected columns in the list view cannot be retained after columns are deleted.
-  When an operation has completed, and if the Data Studio window is not the active window of the operating system, then the message dialog is shown only when the Data Studio window becomes active.
-  The following limitations are applicable to operations in :ref:`Editing Table Data <en-us_topic_0000001860318649__section1226872111129>`:

   -  Entering expression values in the **Edit Table Data** tab is not supported.
   -  Data Studio allows editing of fetched records only.
   -  The Editing table filter feature will not highlight search words within HTML tags such as <, &, >.
   -  A cell containing single '&' in it will not be displayed in the tooltip. A cell containing two consecutive '&' will display as a single '&' in the tooltip.
   -  Row focus is not retained on a newly added row. You need to click the desired cell.

Function/Procedure
------------------

Functions/Procedures created in the SQL Terminal or the **Create Function/Procedure** wizard must end with "/" to indicate the end of functions/procedures. Statements entered after a function/procedure without "/" at the end will be treated as a single query and may trigger errors during execution.

General
-------

-  A maximum of 100 tabs can be opened in the editor area. Tabs are based on available resources of the host machine.
-  A maximum of 64 characters (text only) are allowed for database object names (database, schema, function, procedure, table, sequence, constraint, index, view, and tablespace). There is no limit to the number of characters that can be used in expressions and descriptions in Data Studio.
-  A maximum of 300 result tabs can be opened on a logged instance of Data Studio.
-  If there are large objects loaded in Object Browser and Search Object window, expanding of objects in Object Browser may be slow and Data Studio may become unresponsive.
-  Adjusting the cell width may cause Data Studio to fail to respond.
-  When the data in a table cell is more than 1,000 characters, it will be trimmed to 1,000 characters with "..." at the end.

   -  If you copy the data from a cell in a table or the **Result** tab to any editor, such as SQL terminal/PLSQL source editor, notepad, or any other external editor application, all data is pasted.
   -  If you copy the data from a cell in a table or the **Result** tab to an editable cell, the cell displays only the first 1,000 characters and the excess characters are displayed as **...**.
   -  When the table/**Result** tab data is exported, the exported file contains the whole data.

Security
--------

Data Studio validates SSL connection parameters only for the first time of connection. If **Enable SSL** is selected, the same SSL connection parameters are used when a new connection is opened.

.. note::

   -  If **Enable SSL** is not selected during connecting to Data Studio, data is not encrypted by default.
   -  If the security file is damaged during the SSL connection, Data Studio cannot perform any database operations. To resolve this problem, delete the security folder in the corresponding configuration folder and restart Data Studio.

SQL Terminal
------------

-  Opening an SQL file containing a large number of queries may result in an 'Insufficient Memory' error. For details, see :ref:`Troubleshooting <dws_ds_039>`.
-  Data Studio does not disable the auto-suggest and hyperlink features in the commented text in the **SQL Terminal**.
-  The Hyperlink feature is not supported if the schema or table name have either spaces or dots (.) in them.
-  Auto-suggest is not supported if the object name contains single or double quotes in it.
-  DS supports basic formatting of simple SELECT statements only and may not work as expected for complex queries.
