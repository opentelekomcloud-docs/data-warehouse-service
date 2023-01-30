:original_name: DWS_DS_128.html

.. _DWS_DS_128:

Using SQL Terminals
===================

Auto Commit
-----------

The **Auto Commit** option is available in the **Preferences** pane. For details, see :ref:`Transaction <en-us_topic_0000001145913107__en-us_topic_0185264581_section210212814444>`.

-  If **Auto Commit** is enabled, the **Commit** and **Rollback** functions are disabled. Transactions are committed automatically.
-  If **Auto Commit** is disabled, **Commit** and **Rollback** functions are enabled. Transactions are committed or rolled back manually.

   .. note::

      -  For OLAP, the server will open a transaction for all SQL statements, such as **SELECT**, **EXPLAIN SELECT**, and **SET PARAMETER**.
      -  For OLTP, the server will open a transaction for only DML statements, such as **INSERT**, **UPDATE**, and **DELETE**.

**Reuse Connection**

The **Reuse Connection** option allows you to select the same SQL terminal connection or new connection for the result set. The selection affects the record visibility due to the isolation levels defined in the database server.

-  When **Reuse Connection** is set to **ON**, terminal connection will be used to perform operations on data and refresh the **Result** tab.

For some databases, the temporary tables created or used by the terminal can be edited in the **Result** tab.

-  When **Reuse Connection** is set to **OFF**, new connection will be used to perform operations on data and refresh the **Result** tab.

For some databases, the temporary tables can be edited in the **Result** tab.

|image1|: displayed when **Reuse Connection** is set to **ON**

|image2|: displayed when **Reuse Connection** is set to **OFF**

|image3|: displayed when **Reuse Connection** is disabled

Perform the following steps to set **Reuse Connection** to **OFF**:

#. Click |image4| in the **SQL Terminal** toolbar.

   **Reuse Connection** is set to **OFF** for the terminal. |image5| is displayed.

   .. note::

      -  The **Reuse Connection** function is set to **ON** by default. You can set it to **OFF** as required. If you enable **Auto Commit**, **Reuse Connection** is set to **ON** automatically.
      -  If you disable **Auto Commit**, **Reuse Connection** is set to **OFF** automatically. However, this function is still displayed as **ON** on the GUI, and the status cannot be modified.

For details about **Auto Commit** and **Reuse Connection**, see :ref:`Table 1 <en-us_topic_0000001145713115__en-us_topic_0185264923_table169131912115>`.

.. _en-us_topic_0000001145833051__en-us_topic_0185264856_section16147111413113:

Executing SQL Queries
---------------------

Perform the following steps to execute a function/procedure or SQL query:

Enter a function/procedure or SQL statement in the **SQL Terminal** tab and click |image6|, or press **Ctrl+Enter**, or choose **Run** > **Compile/Execute Statement** in the main menu.

Alternatively, you can right-click in the **SQL Terminal** tab and select **Execute Statement**.

.. note::

   You can check the status bar to view the status of a query being executed.

After the function/procedure or SQL query is executed, the result is generated and displayed in the **Result** tab.

If the connection is lost during execution but the database remains connected in **Object Browser**, the **Connection Error** dialog box is displayed with the following options:

-  **Reconnect**: The database is reconnected.
-  **Reconnect and Execute**: With **Auto Commit** set to **ON**, execution will continue from the failed statement. With **Auto Commit** set to **OFF**, execution will continue from the position of the cursor.
-  **Cancel**: The database is disconnected in **Object Browser**.

If the reconnection fails after three attempts, the database will be disconnected in **Object Browser**. Connect to the database in **Object Browser** and try the execution again.

.. note::

   -  For time-consuming queries, the result set can be edited only after the complete results are obtained.
   -  Query results can be edited in the following scenarios:

      -  The selected objects are in the same table.
      -  All or some columns are selected without aliases, aggregate functions, or column expressions.
      -  The query contains the **WHERE** clause.
      -  The query contains the **ORDER BY** clause.
      -  The table is an ordinary, partitioned, or temporary table.

   -  If an empty row is committed, **Null** is assigned to all columns.
   -  Result sets of queries executed on tables available in **Object Browser** are editable.
   -  Results of queries executed in **SQL Terminal** can be edited.

You can choose **Settings** > **Preferences** to set the column width. For details, see :ref:`Query Results <en-us_topic_0000001145713115__en-us_topic_0185264923_section28611419201210>`.

**Column Reorder**

You can click a column header and drag the column to the desired position.

Multi-Column Sort
-----------------

This feature allows you to sort table data of some pages by multiple columns, as well as to set the priority of columns to be sorted.

This feature is available for the following tabs:

-  **Result Set**
-  **Edit Table Data**
-  **View Table Data**
-  **Batch Drop Result**

Perform the following steps to enable **Multi-Column Sort**:

#. Click |image7| in the toolbar.

   The **Multi-Column Sort** dialog box is displayed.

   |image8|

#. Click **Add Column**. Choose the column to be sorted from the drop-down list.

   |image9|

#. Set the sort order.

#. Click **Apply**.

The **Multi-Column Sort** dialog box contains the following elements.

.. table:: **Table 1** Elements of the Multi-Column Sort dialog box

   +---------------+--------------------------------------------------------------------------+------------------------------------------------------------------------+
   | Name          | UI Element Type                                                          | Description/Operation                                                  |
   +===============+==========================================================================+========================================================================+
   | Priority      | Read-only text field                                                     | Shows the column priority in **Multi-Column Sort**                     |
   +---------------+--------------------------------------------------------------------------+------------------------------------------------------------------------+
   | Column Name   | Concatenated field, which can be all column names of the table           | Shows the name of the column added for sorting                         |
   +---------------+--------------------------------------------------------------------------+------------------------------------------------------------------------+
   | Data Type     | Read-only text field                                                     | Shows data type of the selected column                                 |
   +---------------+--------------------------------------------------------------------------+------------------------------------------------------------------------+
   | Sort Order    | Concatenated field, which can be in either ascending or descending order | Shows the sort order of the selected column                            |
   +---------------+--------------------------------------------------------------------------+------------------------------------------------------------------------+
   | Add Column    | Button                                                                   | Adds new columns to a table for multi-column sort                      |
   +---------------+--------------------------------------------------------------------------+------------------------------------------------------------------------+
   | Delete Column | Button                                                                   | Deletes selected columns from a table for multi-column sort            |
   +---------------+--------------------------------------------------------------------------+------------------------------------------------------------------------+
   | Up            | Button                                                                   | Moves the selected column up by one step to change the sort priority   |
   +---------------+--------------------------------------------------------------------------+------------------------------------------------------------------------+
   | Down          | Button                                                                   | Moves the selected column down by one step to change the sort priority |
   +---------------+--------------------------------------------------------------------------+------------------------------------------------------------------------+
   | Apply         | Button                                                                   | Applies the sort priority                                              |
   +---------------+--------------------------------------------------------------------------+------------------------------------------------------------------------+

.. note::

   Data types will be sorted in an alphabetical order, except the following ones:

   **TINYINT**, **SMALLINT**, **INTEGER**, **BIGINT**, **FLOAT**, **REAL**, **DOUBLE**, **NUMERIC**, **BIT**, **BOOLEAN**, **DATE**, **TIME**, **TIME_WITH_TIMEZONE**, **TIMESTAMP**, and **TIMESTAMP_WITH_TIMEZONE**

The **Multi-Column Sort** dialog box contains the following icons.

.. table:: **Table 2** Icons of the Multi-Column Sort dialog box

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Icon                  | Description           | Operation                                                                                                                                              |
   +=======================+=======================+========================================================================================================================================================+
   | |image10|             | Not Sorted            | If this icon is displayed in a column header, the column is not sorted. You can click this icon to sort the column in ascending order.                 |
   |                       |                       |                                                                                                                                                        |
   |                       |                       | Alternatively, use **Alt+Click** to select the column header.                                                                                          |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | |image11|             | Ascending Sort        | If this icon is displayed in a column header, the column is sorted in ascending order. You can click this icon to sort the column in descending order. |
   |                       |                       |                                                                                                                                                        |
   |                       |                       | Alternatively, use **Alt+Click** to select the column header.                                                                                          |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | |image12|             | Descending Sort       | If this icon is displayed in a column header, the column is sorted in descending order. If you click on this icon the column will be in no sort order. |
   |                       |                       |                                                                                                                                                        |
   |                       |                       | Alternatively, use **Alt+Click** to select the column header.                                                                                          |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+

Icons for the sort priority are as follows:

|image13|: Icon with three dots indicates the highest priority.

|image14|: Icon with two dots indicates the second highest priority.

|image15|: Icon with one dot indicates the lowest priority.

.. table:: **Table 3** Toolbar icons

   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Toolbar Name               | Toolbar Icon          | Description                                                                                                                                                                                                                                                                                                                                                                                                                               |
   +============================+=======================+===========================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | Copy                       | |image16|             | This icon is used to copy selected data from the **Result** pane to clipboard. The shortcut key is **Ctrl+C**.                                                                                                                                                                                                                                                                                                                            |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Advanced Copy              | |image17|             | This icon is used to copy selected data from the **Result** pane to clipboard. The copied data includes column headers. See :ref:`Query Results <en-us_topic_0000001145713115__en-us_topic_0185264923_section28611419201210>` to set this preference. The shortcut key is **Ctrl+Shift+C**.                                                                                                                                               |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Export all data            | |image18|             | This icon is used to export all data to files in Excel (xlsx/xls), CSV, text, or binary format. For details, see :ref:`Exporting Table Data <dws_ds_98>`.                                                                                                                                                                                                                                                                                 |
   |                            |                       |                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |                       | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                                 |
   |                            |                       |                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |                       |    -  The columns involved in the query are automatically populated in the **Selected Columns** area. The **Available Columns** area is empty.                                                                                                                                                                                                                                                                                            |
   |                            |                       |    -  To export the query results, execute the query again using a new connection. The exported results may be different from the data in the **Result** pane.                                                                                                                                                                                                                                                                            |
   |                            |                       |    -  This function is not available for queries using the **EXPLAIN** or **ANALYZE** statement. To export such queries, select **Export current page data**.                                                                                                                                                                                                                                                                             |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Export current page data   | |image19|             | This icon is used to export current page data to files in Excel (xlsx/xls) or CSV format.                                                                                                                                                                                                                                                                                                                                                 |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Paste                      | |image20|             | This icon is used to paste copied information. For details, see :ref:`Paste <en-us_topic_0000001098993150__en-us_topic_0185264734_section3927320420>`.                                                                                                                                                                                                                                                                                    |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Add                        | |image21|             | This icon is used to add a row to the result set. For details, see :ref:`Insert <en-us_topic_0000001098993150__en-us_topic_0185264734_section5333590620118>`.                                                                                                                                                                                                                                                                             |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Delete                     | |image22|             | This icon is used to delete a row from the result set. For details, see :ref:`Delete <en-us_topic_0000001098993150__en-us_topic_0185264734_section2976164920345>`.                                                                                                                                                                                                                                                                        |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Save                       | |image23|             | This icon is used to save the changes made in the result set. For details, see :ref:`Editing Table Data <dws_ds_102>`.                                                                                                                                                                                                                                                                                                                    |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Rollback                   | |image24|             | This icon is used to roll back the changes made in the result set. For details, see :ref:`Editing Table Data <dws_ds_102>`.                                                                                                                                                                                                                                                                                                               |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Refresh                    | |image25|             | This icon is used to refresh information in the result set. If multiple result sets are open for the same table, changes made in one result set will take effect in other result sets after refresh. If the table is edited, the result sets will be updated after refresh.                                                                                                                                                               |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Clear Unique Key selection | |image26|             | This icon is used to clear the previously selected unique key. For details, see :ref:`Editing Table Data <dws_ds_102>`.                                                                                                                                                                                                                                                                                                                   |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Show/Hide Query bar        | |image27|             | This icon is used to display or hide the query executed for a specified result set. This is a toggle button.                                                                                                                                                                                                                                                                                                                              |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Show/Hide Search bar       | |image28|             | This icon is used to display or hide the **Search** field. This is a toggle button.                                                                                                                                                                                                                                                                                                                                                       |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Encoding                   | |image29|             | Whether you can configure this field depends on the settings in **Preferences** > **Result Management** > **Query Results** > **Result Data Encoding**. In this drop-down list, you can select the appropriate encoding to view the data accurately. The value defaults to UTF-8. For details about the encoding preference, see :ref:`Result Data Encoding <en-us_topic_0000001145713115__en-us_topic_0185264923_section7921102619295>`. |
   |                            |                       |                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |                       | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                                 |
   |                            |                       |                                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                            |                       |    Data editing operations, except data insertion, are restricted after the default encoding is modified.                                                                                                                                                                                                                                                                                                                                 |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Multi Sort                 | |image30|             | This icon is used to display the **Multi Sort** dialog box.                                                                                                                                                                                                                                                                                                                                                                               |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Clear Sort                 | |image31|             | This icon is used to reset all sorted columns.                                                                                                                                                                                                                                                                                                                                                                                            |
   +----------------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Icons in the **Search** field are as follows:

+-------------------+-----------+--------------------------------------------------------------------------------------------------------------+
| Icon Name         | Icon      | Description                                                                                                  |
+===================+===========+==============================================================================================================+
| Search            | |image34| | This icon is used to search for result sets according to the criteria defined. The text is case-insensitive. |
+-------------------+-----------+--------------------------------------------------------------------------------------------------------------+
| Clear Search Text | |image35| | This icon is used to clear the text entered in the **Search** field.                                         |
+-------------------+-----------+--------------------------------------------------------------------------------------------------------------+

Right-click options in the **Result** pane are as follows:

+-------------------------+------------------------------------------------------------+
| Option                  | Description                                                |
+=========================+============================================================+
| Close                   | Closes only the active **Result** pane                     |
+-------------------------+------------------------------------------------------------+
| Close Others            | Closes all other **Result** panes except the active one    |
+-------------------------+------------------------------------------------------------+
| Close Tabs to the Right | Closes all **Result** panes to the right of the active one |
+-------------------------+------------------------------------------------------------+
| Close All               | Closes all **Result** panes including the active one       |
+-------------------------+------------------------------------------------------------+
| Detach                  | Opens only the active **Result** pane                      |
+-------------------------+------------------------------------------------------------+

Status information displayed in the **Result** pane is as follows:

-  **Query Submit Time**: indicates time when a query is submitted
-  The number of rows obtained and the execution time are displayed, as well as the default number of rows. If there are additional rows to be obtained, the icon is displayed with the word **more**. You can scroll to the bottom of the table to obtain and display all rows.

   .. important::

      When you are viewing table data, Data Studio automatically adjusts the column width for better display. You can adjust the column width as required. If the text length exceeds the column width and you adjust the column width, Data Studio may fail to respond.

.. note::

   -  A new **Result** pane is opened each time a query is executed. To view the results in the new pane, select the pane.
   -  Set the **focusOnFirstResult** parameter to **false** to automatically locate the newly opened **Result** pane. For details, see :ref:`Installing and Configuring Data Studio <dws_ds_16>`.
   -  Selected rows, columns, and cells can be copied from the result set.
   -  All data can be exported even after the connection is removed.
   -  If the text of a column contains spaces, word wrapping is applied to fit the column width. Word wrapping is not applied to columns without space.
   -  To copy part of a cell, select the desired part and press **Ctrl+C** or click |image36|.
   -  The column size depends on the column with the longest text.
   -  You can save preferences to define:

      -  Number of records to be obtained

      -  Column width

      -  Copying options from a result set

         For details, see :ref:`Query Results <en-us_topic_0000001145713115__en-us_topic_0185264923_section28611419201210>`.

   -  If the lock icon is displayed in any column of the **Result Set** tab, parameters cannot be edited.

.. _en-us_topic_0000001145833051__en-us_topic_0185264856_section7637071471:

Backing up Unsaved Queries/Functions/Procedures
-----------------------------------------------

Data Studio backs up unsaved data in **SQL Terminal** and **PL/SQL Viewer** periodically based on the time interval defined in the **Preferences** pane. Data is encrypted and saved based on the **Preference** settings. See :ref:`Query/Function/Procedure Backup <en-us_topic_0000001099153186__en-us_topic_0185264709_section1980415371926>` to enable or disable the backup function, set time interval of data saving, and encrypt the saved data.

Unsaved changes in **SQL Terminal** and **PL/SQL Viewer** are backed up and saved in the **DataStudio\\UserData\\**\ *Username*\ **\\Autosave** folder. If these backup files have been saved before Data Studio is shut down unexpectedly, these files will be available upon the next login.

If unsaved data exists in **SQL Terminal** and **PL/SQL Viewer** during graceful exit, Data Studio will not be closed until the backup is complete.

Locating Errors
---------------

When an error occurs during the execution of queries/functions/procedures, an error locating message will be displayed.

**Yes**: Click **Yes** to proceed with the execution.

**No**: Click **No** to stop the execution.

You can select **Do not display other errors that occur during the execution** to hide the error messages and proceed with the current SQL query.

The line number and position of an error message is displayed in the **Messages** pane. In **SQL Terminal** or **PL/SQL Viewer**, the corresponding line is marked with |image37| and a red underline at the position of the error. You can hover over |image38| to display the error message. For details about why the line number does not match with the error detail, see :ref:`FAQs <dws_ds_155>`.

.. note::

   If a query/function/procedure is modified during execution, the error locator may not display the correct line number and the position of the error.

Searching in the PL/SQL Viewer or SQL Terminal Pane
---------------------------------------------------

Perform the following steps to search in the **PL/SQL Viewer** or **SQL Terminal** pane:

Press **F3** to search for the next line or **Shift+F3** to search for the previous line. You can use these shortcut keys after pressing **Ctrl+F** to search for text and key words. **Ctrl+F**, **F3**, and **Shift+F3** will be available only when you search for keywords in the current instance.

#. Choose **Edit** > **Find and Replace** from the main menu.

   Alternatively press **Ctrl+F**.

   The **Find and Replace** dialog box is displayed.

#. Enter the text to be searched for in the **Find what** field, and click the **Find Next** button.

   The desired text is highlighted.

   You can press **F3** for forward search or **Shift+F3** for backward search.

   .. note::

      When reaching the last line in a SQL query or PL/SQL statement, select **Wrap around** to proceed with the search.

Locating a Specific Line in the PL/SQL Viewer or SQL Terminal Pane
------------------------------------------------------------------

Perform the following steps to locate a specific line in the **PL/SQL Viewer** or **SQL Terminal** pane:

Perform the following steps to go to a line in **PL/SQL Viewer** or **SQL Terminal**:

#. Choose **Edit** > **Go To Line** from the main menu or press **Ctrl+G**.

   The **Go To Line** dialog box is displayed, allowing you to skip to a specific line in **SQL Terminal**.

#. Enter the desired line number in the **Enter the line number** field, and click **OK**.

   The cursor moves to the beginning of the line entered in the **Go to Line** dialog box.

   .. note::

      You cannot enter the following characters in this field:

      -  Non-numeric character
      -  Special characters
      -  Line numbers that do not exist in the editor
      -  Number with more than 10 digits

Commenting or Uncommenting
--------------------------

Data Studio allows you to comment or uncomment lines or blocks.

Perform the following steps to comment or uncomment lines in **PL/SQL Viewer** or **SQL Terminal**:

#. Select the lines to comment or uncomment.

#. Choose **Edit** > **Comment/Uncomment Lines** from the main menu to comment or uncomment each selected line.

   Alternatively, press **Ctrl+/** or right-click a line and select **Comment/Uncomment Lines**.

Perform the following steps to comment or uncomment blocks in **PL/SQL Viewer** or **SQL Terminal**:

#. Select the lines or a block to comment or uncomment.

#. Choose **Edit** > **Comment/Uncomment Block** from the main menu to comment or uncomment each selected line or the entire block.

   Alternatively, press **Ctrl+Shift+/** or right-click a line or the entire block and select **Comment/Uncomment Block**.

.. _en-us_topic_0000001145833051__en-us_topic_0185264856_section2611012152811:

Indenting or Un-indenting Lines
-------------------------------

You can indent or un-indent lines according to the indent size defined in **Preferences**.

Perform the following steps to indent lines in **PL/SQL Viewer** or **SQL Terminal**:

#. Select the desired lines.

#. Press **Tab** or click |image39|.

   Move the selected lines according to the indent size defined in **Preferences**. For details about modifying the indent size, see :ref:`Formatter <en-us_topic_0000001145913107__en-us_topic_0185264581_section64411944205615>`.

Perform the following steps to un-indent lines in **PL/SQL Viewer** or **SQL Terminal**:

#. Select the desired lines.

#. Press **Shift+Tab** or click |image40|.

   Move the selected lines according to the indent size defined in **Preferences**. For details about modifying the indent size, see :ref:`Formatter <en-us_topic_0000001145913107__en-us_topic_0185264581_section64411944205615>`.

   .. note::

      Only selected lines that have available tab space will be un-indented. For example, if multiple lines are selected and one of the selected line starts at position 1, pressing **Shift+Tab** will un-indent all lines except the one starting at position 1.

Inserting Spaces
----------------

The **Insert Space** option is used to replace a tab with spaces according to the indent size defined in **Preferences**.

Perform the following steps to replace a tab with spaces in **PL/SQL Viewer** or **SQL Terminal**:

#. Select the desired lines.

#. Press **Tab** or **Shift+Tab**.

   A tab is replaced with spaces according to the indent size defined in **Preferences**. For details about modifying the indent size, see :ref:`Formatter <en-us_topic_0000001145913107__en-us_topic_0185264581_section64411944205615>`.

Executing Multiple Functions/Procedures or Queries
--------------------------------------------------

Perform the following steps to execute multiple functions/procedures:

Insert a forward slash (/) in a new line under the function/procedure in **SQL Terminal**.

Add the new function/procedure in the next line.

|image41|

Perform the following steps to execute multiple SQL queries:

#. Enter multiple SQL queries in **SQL Terminal** as follows:

   |image42|

#. Click |image43| in **SQL Terminal**, or press **Ctrl+Enter**, or choose **Run** > **Compile/Execute Statement** from the main menu.

   .. note::

      -  If no query is selected, only the query in the line where the cursor is placed will be executed.
      -  If the cursor is placed in an empty line, the next available query statement will be executed.
      -  If the cursor is placed in the last empty line, no query will be executed.
      -  If a single query is written in multiples lines and the cursor is placed in any line of the query, the query will be executed. Queries are separated using a semicolon (;).

Perform the following steps to execute a SQL query after executing a function/procedure:

Insert a forward slash (/) in a new line under the function/procedure in **SQL Terminal**. Then add new query or function/procedure statements.

Perform the following steps to execute PL/SQL statements and SQL queries on different connections:

Select the required connection from the **Connection** drop-down list and click |image44| in **SQL Terminal**.

Renaming a SQL Terminal
-----------------------

Perform the following steps to rename a SQL Terminal:

#. Right-click in **SQL Terminal** and select **Rename Terminal**.

   The **Rename Terminal** dialog box is displayed prompting you to enter the new terminal name.

#. Enter the new name and click **OK**.

   .. note::

      -  The terminal name must follow the Windows file naming convention.
      -  The **Rename Terminal** dialog box allows a maximum of 150 characters.
      -  The **Restore** option cannot be used to restore to the default name. You must manually rename a terminal with its default name.
      -  Tool tip of the renamed terminal will display the previous terminal name.

SQL Assistant
-------------

The SQL Assistant tool provides suggestion or reference for the information entered in **SQL Terminal** and **PL/SQL Viewer**. Perform the following steps to open SQL Assistant:

When Data Studio is started, related syntax is displayed in the **SQL Assistant** panel. After you enter a query in **SQL Terminal**, related syntax details are displayed, including precautions, examples, and description of syntax, functions, and parameters. Select the text and right-click to copy the selected text or copy and paste it to **SQL Terminal**.

.. note::

   -  Choose **Settings** > **Preferences** > **Environment** > **Session Setting**. In the **SQL Assistant** area displayed on the right, enable or disable the **SQL Assistant** function permanently. By default, the **SQL Assistant** function is enabled permanently.
   -  After the **SQL Assistant** function is enabled, you can click the **SQL Assistant** icon (|image45|) on the toolbar to open the **SQL Assistant** window. If the **SQL Assistant** icon is gray after the **SQL Assistant** function is enabled, the operation is invalid.

.. _en-us_topic_0000001145833051__en-us_topic_0185264856_section2303144411114:

Using Templates
---------------

The **Templates** option of Data Studio allows you to insert frequently used SQL statements in **SQL Terminal** or **PL/SQL Viewer**. Some frequently used SQL statements have been saved in Data Studio. You can create, edit, or remove a template. For details, see :ref:`Adding/Editing/Removing a Template <en-us_topic_0000001145913107__en-us_topic_0185264581_section116501350276>`.

The following table lists the default templates.

==== ================
Name Description
==== ================
df   delete from
is   insert into
o    order by
s\*  select from
sc   select row count
sf   select from
sl   select
==== ================

Perform the following steps to use the **Templates** option:

#. Enter a template name in **SQL Terminal** or **PL/SQL Viewer**.

#. Press **Ctrl+Alt+Space**.

   A list of existing template information is displayed. For details, see the following tables.

   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Exact Match                       | Display                                                                                                                                                  |
   +===================================+==========================================================================================================================================================+
   | On                                | Displays all entries that start with the input text (case-sensitive).                                                                                    |
   |                                   |                                                                                                                                                          |
   |                                   | For example, if **SF** is entered in **SQL Terminal** or **PL/SQL Viewer**, all entries that start with **SF** are displayed.                            |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Off                               | Displays all entries that start with the input text (case-insensitive).                                                                                  |
   |                                   |                                                                                                                                                          |
   |                                   | For example, if **SF** is entered in **SQL Terminal** or **PL/SQL Viewer**, all entries that start with **SF**, **Sf**, **sF**, or **sf** are displayed. |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+

   +---------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Text Selection/Cursor Location                    | Display                                                                                                                                               |
   +===================================================+=======================================================================================================================================================+
   | Text is selected and the shortcut key is used.    | Displays entries that match the text between the leftmost character of the selected text and the space or newline character nearest to the character. |
   +---------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No text is selected and the shortcut key is used. | Displays entries that match the text between the cursor position and the space or newline character nearest to that position.                         |
   +---------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      -  If you press **Ctrl+Alt+Space** without entering text in **SQL Terminal** or **PL/SQL Viewer**, all entries in **Templates** will be displayed.
      -  If the text entered in **SQL Terminal** or **PL/SQL Viewer** matches with only one entry, this entry will replace the text entered and the template list will not be displayed.

.. |image1| image:: /_static/images/en-us_image_0000001098993294.png
.. |image2| image:: /_static/images/en-us_image_0000001099153270.png
.. |image3| image:: /_static/images/en-us_image_0000001098673462.png
.. |image4| image:: /_static/images/en-us_image_0000001145833145.png
.. |image5| image:: /_static/images/en-us_image_0000001098673454.png
.. |image6| image:: /_static/images/en-us_image_0000001145513285.png
.. |image7| image:: /_static/images/en-us_image_0000001099153268.png
.. |image8| image:: /_static/images/en-us_image_0000001098833292.png
.. |image9| image:: /_static/images/en-us_image_0000001098993282.png
.. |image10| image:: /_static/images/en-us_image_0000001145913253.png
.. |image11| image:: /_static/images/en-us_image_0000001145713209.png
.. |image12| image:: /_static/images/en-us_image_0000001098993288.png
.. |image13| image:: /_static/images/en-us_image_0000001145713351.png
.. |image14| image:: /_static/images/en-us_image_0000001145913395.png
.. |image15| image:: /_static/images/en-us_image_0000001099153406.png
.. |image16| image:: /_static/images/en-us_image_0000001145713205.jpg
.. |image17| image:: /_static/images/en-us_image_0000001098673456.jpg
.. |image18| image:: /_static/images/en-us_image_0000001099153272.jpg
.. |image19| image:: /_static/images/en-us_image_0000001145913249.jpg
.. |image20| image:: /_static/images/en-us_image_0000001098993290.png
.. |image21| image:: /_static/images/en-us_image_0000001098993292.jpg
.. |image22| image:: /_static/images/en-us_image_0000001099153264.png
.. |image23| image:: /_static/images/en-us_image_0000001145513283.png
.. |image24| image:: /_static/images/en-us_image_0000001145913255.png
.. |image25| image:: /_static/images/en-us_image_0000001145833147.jpg
.. |image26| image:: /_static/images/en-us_image_0000001098833286.png
.. |image27| image:: /_static/images/en-us_image_0000001145913251.png
.. |image28| image:: /_static/images/en-us_image_0000001099153258.png
.. |image29| image:: /_static/images/en-us_image_0000001145513293.png
.. |image30| image:: /_static/images/en-us_image_0000001098833288.png
.. |image31| image:: /_static/images/en-us_image_0000001098993284.png
.. |image32| image:: /_static/images/en-us_image_0000001145913247.png
.. |image33| image:: /_static/images/en-us_image_0000001098833280.png
.. |image34| image:: /_static/images/en-us_image_0000001145913247.png
.. |image35| image:: /_static/images/en-us_image_0000001098833280.png
.. |image36| image:: /_static/images/en-us_image_0000001098673458.jpg
.. |image37| image:: /_static/images/en-us_image_0000001145833267.png
.. |image38| image:: /_static/images/en-us_image_0000001098833290.png
.. |image39| image:: /_static/images/en-us_image_0000001099153266.jpg
.. |image40| image:: /_static/images/en-us_image_0000001145713203.jpg
.. |image41| image:: /_static/images/en-us_image_0000001145713197.jpg
.. |image42| image:: /_static/images/en-us_image_0000001145913245.jpg
.. |image43| image:: /_static/images/en-us_image_0000001145833141.png
.. |image44| image:: /_static/images/en-us_image_0000001145513285.png
.. |image45| image:: /_static/images/en-us_image_0000001145513291.png
