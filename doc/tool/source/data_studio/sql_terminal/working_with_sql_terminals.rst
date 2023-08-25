:original_name: DWS_DS_128.html

.. _DWS_DS_128:

Working with SQL Terminals
==========================

In **SQL Terminal**, you can

-  :ref:`Automatically Commit a Transaction <en-us_topic_0000001234200573__en-us_topic_0185264856_section20334134104119>`
-  :ref:`Execute SQL Queries <en-us_topic_0000001234200573__en-us_topic_0185264856_section16147111413113>`
-  :ref:`Multi-Column Sort <en-us_topic_0000001234200573__en-us_topic_0185264856_section5488201717246>`
-  :ref:`Backuping Unsaved Queries/Functions/Procedures <en-us_topic_0000001234200573__en-us_topic_0185264856_section7637071471>`
-  :ref:`Error Locator <en-us_topic_0000001234200573__en-us_topic_0185264856_section0486740204318>`
-  :ref:`Search in PL/SQL Viewer or SQL Terminal <en-us_topic_0000001234200573__en-us_topic_0185264856_section942319263275>`
-  :ref:`Go to Line in PL/SQL Viewer or SQL Terminal <en-us_topic_0000001234200573__en-us_topic_0185264856_section295082372218>`
-  :ref:`Comment/Uncomment <en-us_topic_0000001234200573__en-us_topic_0185264856_section134691211285>`
-  :ref:`Indent/Un-indent Lines <en-us_topic_0000001234200573__en-us_topic_0185264856_section2611012152811>`
-  :ref:`Insert Space <en-us_topic_0000001234200573__en-us_topic_0185264856_section16493161471713>`
-  :ref:`Execute Multiple Functions/Procedures or Queries <en-us_topic_0000001234200573__en-us_topic_0185264856_section1691612421411>`
-  :ref:`Rename SQL Terminal <en-us_topic_0000001234200573__en-us_topic_0185264856_section46225611153>`
-  SQL Assistant
-  :ref:`Using Templates <en-us_topic_0000001234200573__en-us_topic_0185264856_section2303144411114>`

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section20334134104119:

Auto Commit
-----------

The **Auto Commit** option can be switched on or off based on the **Preferences** settings. Refer to :ref:`Transaction <en-us_topic_0000001188521052__en-us_topic_0185264581_section210212814444>` for more details.

-  If **Auto Commit** option is enabled, **Commit** and **Rollback** buttons are disabled. Transactions are committed automatically.
-  If **Auto Commit** option is disabled, **Commit** and **Rollback** buttons are enabled. You can use the buttons manually to commit or revert the changes.

   .. note::

      -  For OLAP, the server will open a transaction for all SQL statements, such as **select**, **explain select**, and **set parameter**.
      -  For OLTP, the server will open a transaction for only DML statements, such as **INSERT**, **UPDATE**, and **DELETE**.

**Reuse Connection**

It enables the user to choose the same SQL terminal connection or new connection for the result set. The selection affects the record visibility due to the isolation levels defined in the database server.

-  When **Reuse Connection** is **ON**, terminal connection will be used for data manipulation and refresh of the result window.

For some data base temp tables that are created or used by the terminal can be edited in the result window.

-  When **Reuse Connection** is **OFF**, new connection will be used for data manipulation and refresh of the result window.

For some databases, the temporary tables can be edited in the **Result** tab.

|image1|: displayed when **Reuse Connection** is set to **ON**

|image2|: displayed when **Reuse Connection** is set to **OFF**

|image3|: displayed when **Reuse Connection** is disabled

Perform the following steps to set **Reuse Connection** to **OFF**:

#. Click |image4| on the **SQL Terminal** toolbar.

   **Reuse Connection** is disabled for the terminal. |image5|

   .. note::

      -  The **Reuse Connection** function is enabled by default. You can disable it as required. If you enable **Auto Commit**, the system automatically enables the **Reuse Connection** function.
      -  If you disable **Auto Commit**, the system automatically disables the **Reuse Connection** function. However, this function is still displayed as **Enabled** on the GUI, and the status cannot be modified.

Refer to :ref:`Table 1 <en-us_topic_0000001188202558__en-us_topic_0185264923_table169131912115>` for more details about **Auto Commit** and **Reuse Connection**.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section16147111413113:

Execute SQL Queries
-------------------

Perform the following steps to execute a function/procedure or SQL query.

Enter a function/procedure(s) or SQL query(s) in the **SQL Terminal** tab and click |image6| in the **SQL Terminal** tab, or press **Ctrl+Enter**, or choose **Run > Compile/Execute Statement** from the main menu.

Alternatively, you can right-click in the **SQL Terminal** tab and select **Execute Statement**.

.. note::

   You can check the status bar to view the status of a query being executed.

The **Result** tab displays the results after executing the function/procedure(s) or SQL queries along with the query statement executed.

If the connection is lost during execution and the database is still connected in Object Browser, then **Connection Error** dialog box is displayed:

-  **Reconnect** - The connection is reestablished.
-  **Reconnect and Execute** - With Auto commit on, execution will continue from failure statement. With Auto commit off, execution will continue from position of cursor.
-  **Cancel** - Disconnects database in Object Browser.

Failure to reconnect after three attempts will disconnect the database in Object Browser. Connect to the database in Object Browser and retry execution.

.. note::

   -  For long running queries, result set can be edited only after the complete results are fetched.
   -  Editing of query results are only allowed in following scenarios:

      -  Selected targets are from a single table
      -  Either select all columns or subset of columns [No aliases, aggregate functions, expressions on columns]
      -  All WHERE condition
      -  All ORDER BY clause
      -  On regular, partition, and temporary tables.

   -  Committing an empty row assigns Null to all columns.
   -  Only result set of queries executed on tables available in Object Browser is editable.
   -  Editing of query results is allowed only for queries executed in SQL Terminal.

The column width definition can be set using **Settings > Preferences** option. Refer to :ref:`Query Results <en-us_topic_0000001188202558__en-us_topic_0185264923_section28611419201210>` to set this parameter.

**Column Reorder**

Column reordering can be performed by clicking and dragging the selected column header to the desired position.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section5488201717246:

Multi-Column Sort
-----------------

This feature allows the user to sort table data of some pages by multiple columns. In addition, you can set the priority of columns for sorting.

The feature is available for the following pages:

-  Result Set Tab
-  Edit Table Data Window
-  View Table Data Window
-  Batch Drop Result Window

Follow the steps below to access Multi-column sort:

#. Click |image7| in the toolbar.

   **Multi-Column Sort** pop-up is displayed.

   |image8|

#. Click **Add Column**. Choose the column to be sorted from the drop-down list.

   |image9|

#. Select the required sort order.

#. Click **Apply**.

Multi-sort pop up has following elements:

.. table:: **Table 1** Elements of multi-column pop-up:

   +----------------+-------------------------------------------------------------------+--------------------------------------------------------------------+
   | Attribute Name | UI Element Type                                                   | Description/Action                                                 |
   +================+===================================================================+====================================================================+
   | Priority       | Read only text field                                              | Shows column priority in multi sort.                               |
   +----------------+-------------------------------------------------------------------+--------------------------------------------------------------------+
   | Column Name    | Combo field having all column names of the table as its value set | Column name of the column added for sorting.                       |
   +----------------+-------------------------------------------------------------------+--------------------------------------------------------------------+
   | Data Type      | Read only text field                                              | Shows data type of the column selected.                            |
   +----------------+-------------------------------------------------------------------+--------------------------------------------------------------------+
   | Sort Order     | Combo field having values {sort_ascending, sort_descending}       | Sort order of the column.                                          |
   +----------------+-------------------------------------------------------------------+--------------------------------------------------------------------+
   | Add Column     | Button                                                            | Adds new row to multi-sort table.                                  |
   +----------------+-------------------------------------------------------------------+--------------------------------------------------------------------+
   | Delete Column  | Button                                                            | Deletes selected column from multi-sort table.                     |
   +----------------+-------------------------------------------------------------------+--------------------------------------------------------------------+
   | Up             | Button                                                            | Moves selected column up by 1 step, thus changing sort priority.   |
   +----------------+-------------------------------------------------------------------+--------------------------------------------------------------------+
   | Down           | Button                                                            | Moves selected column down by 1 step, this changing sort priority. |
   +----------------+-------------------------------------------------------------------+--------------------------------------------------------------------+
   | Apply          | Button                                                            | Apply prepared sort configuration.                                 |
   +----------------+-------------------------------------------------------------------+--------------------------------------------------------------------+

.. note::

   Except following data types, all the other data types will be sorted by their string value (Alphabetical order):

   TINYINT, SMALLINT, INTEGER, BIGINT, FLOAT, REAL, DOUBLE, NUMERIC, BIT, BOOLEAN, DATE, TIME, TIME_WITH_TIMEZONE, TIMESTAMP, TIMESTAMP_WITH_TIMEZONE.

Elements of Multi-Column Pop-up:

.. table:: **Table 2** Icons of multi-column Pop-up

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Icon                  | Description           | Action                                                                                                                                                       |
   +=======================+=======================+==============================================================================================================================================================+
   | |image10|             | Not Sorted            | This icon in column header indicates that the column is not sorted. You can click this icon to sort the column in ascending order.                           |
   |                       |                       |                                                                                                                                                              |
   |                       |                       | Alternatively, use **Alt+Click** to select the column header.                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | |image11|             | Ascending Sort        | This icon in column header indicates that the column is sorted in ascending order. If you click on this icon, the column will be sorted in descending order. |
   |                       |                       |                                                                                                                                                              |
   |                       |                       | Alternatively, use **Alt+Click** to select the column header.                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | |image12|             | Descending Sort       | This icon in column header indicates that the column is sorted in descending order. You can click this icon to cancel the column sorting.                    |
   |                       |                       |                                                                                                                                                              |
   |                       |                       | Alternatively, use **Alt+Click** to select the column header.                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+

Icons for the sort priority are as follows:

|image13|: Icon with three dots indicates the highest priority.

|image14|: Icon with two dots indicates the second highest priority.

|image15|: Icon with one dot indicates the lowest priority.

.. table:: **Table 3** Toolbar Menus

   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Toolbar Name               | Toolbar Icon          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   +============================+=======================+================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | Copy                       | |image16|             | This button is used to copy selected content from result window to clipboard. Shortcut key - **Ctrl+C**.                                                                                                                                                                                                                                                                                                                                       |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Advanced Copy              | |image17|             | This button is used to copy content from result window to clipboard. Results can be copied to include column header. Refer to :ref:`Query Results <en-us_topic_0000001188202558__en-us_topic_0185264923_section28611419201210>` to set this preference. The shortcut key is **Ctrl+Shift+C**.                                                                                                                                                  |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Export all data            | |image18|             | This button is used to export all data in Excel (xlsx/xls), CSV, text, or binary format. For details, see :ref:`Exporting Table Data <dws_ds_98>`.                                                                                                                                                                                                                                                                                             |
   |                            |                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                |
   |                            |                       | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                                      |
   |                            |                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                |
   |                            |                       |    -  The columns involved in the query are automatically populated in the **Selected Columns** area. The **Available Columns** area is empty.                                                                                                                                                                                                                                                                                                 |
   |                            |                       |    -  To export the query results, the query is re-executed using a new connection. The exported results may differ from the data in the results tab.                                                                                                                                                                                                                                                                                          |
   |                            |                       |    -  Disabled for explain/analyze queries. To export explain/analyze queries use the **Export current page data** option.                                                                                                                                                                                                                                                                                                                     |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Export current page data   | |image19|             | This button is used to export current page data in Excel (xlsx/xls) or CSV format.                                                                                                                                                                                                                                                                                                                                                             |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Paste                      | |image20|             | This button is used to paste copied information. For details, see :ref:`Paste <en-us_topic_0000001233800701__en-us_topic_0185264734_section3927320420>`.                                                                                                                                                                                                                                                                                       |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Add                        | |image21|             | This button is used to add a row to the result set. For details, see :ref:`Insert <en-us_topic_0000001233800701__en-us_topic_0185264734_section5333590620118>`.                                                                                                                                                                                                                                                                                |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Delete                     | |image22|             | This button is used to delete a row from the result set. For details, see :ref:`Delete <en-us_topic_0000001233800701__en-us_topic_0185264734_section2976164920345>`.                                                                                                                                                                                                                                                                           |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Save                       | |image23|             | This button is used to save the changes made in the result set. For details, see :ref:`Editing Table Data <dws_ds_102>`.                                                                                                                                                                                                                                                                                                                       |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Rollback                   | |image24|             | This button is used to roll back the changes made to the result set. For details, see :ref:`Editing Table Data <dws_ds_102>`.                                                                                                                                                                                                                                                                                                                  |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Refresh                    | |image25|             | This button is used to refresh information in the result set. If multiple result sets are open for the same table, then changes made to one result set will reflect on the other post refresh. Similarly if the same table is edited, then the result set will be updated post refresh.                                                                                                                                                        |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Clear Unique Key selection | |image26|             | This button is used to clear the previous unique key selection. For details, see :ref:`Editing Table Data <dws_ds_102>`.                                                                                                                                                                                                                                                                                                                       |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Show/Hide Query bar        | |image27|             | This button is used to display/hide the query executed for that particular result set. This is a toggle button.                                                                                                                                                                                                                                                                                                                                |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Show/Hide Search bar       | |image28|             | This button is used to display/hide the search text field. This is a toggle button.                                                                                                                                                                                                                                                                                                                                                            |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Encoding                   | |image29|             | Whether you can configure this field depends on the settings in **Preferences** > **Result Management** > **Query Results** > **Result Data Encoding**. In this drop-down list, you can select the appropriate code to view the data accurately. By default, the text is encoded using UTF-8. Refer to :ref:`Result Data Encoding <en-us_topic_0000001188202558__en-us_topic_0185264923_section7921102619295>` to set the encoding preference. |
   |                            |                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                |
   |                            |                       | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                                      |
   |                            |                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                |
   |                            |                       |    Data editing except for data insertion is restricted once the default encoding is modified.                                                                                                                                                                                                                                                                                                                                                 |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Multi Sort                 | |image30|             | This button brings up multi-sort pop up.                                                                                                                                                                                                                                                                                                                                                                                                       |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Clear Sort                 | |image31|             | This button is used to reset all the sorted column.                                                                                                                                                                                                                                                                                                                                                                                            |
   +----------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Icons in Search field:

+-------------------+-----------+---------------------------------------------------------------------------------------------------------+
| Icon Name         | Icon      | Description                                                                                             |
+===================+===========+=========================================================================================================+
| Search            | |image34| | This icon is used to search the result set based on the criteria defined. The text is case-insensitive. |
+-------------------+-----------+---------------------------------------------------------------------------------------------------------+
| Clear Search Text | |image35| | This icon is used to clear the search text entered in the search field.                                 |
+-------------------+-----------+---------------------------------------------------------------------------------------------------------+

Right-click options in the **Result** window:

+-------------------------+----------------------------------------------------------------------+
| Option                  | Description                                                          |
+=========================+======================================================================+
| Close                   | Closes only the active result window.                                |
+-------------------------+----------------------------------------------------------------------+
| Close Others            | Closes all other result windows except for the active result window. |
+-------------------------+----------------------------------------------------------------------+
| Close Tabs to the Right | Closes only the right active result window.                          |
+-------------------------+----------------------------------------------------------------------+
| Close All               | Closes all result windows including the active result window.        |
+-------------------------+----------------------------------------------------------------------+
| Detach                  | Detach from current active result window.                            |
+-------------------------+----------------------------------------------------------------------+

Status information displayed in the **Result** window:

-  **Query Submit Time** - Provides the query submitted time.
-  Number of rows fetched with execution time is displayed. The default number of rows is displayed. If there are additional rows to be fetched, then it will be denoted with the word "more". You can scroll to the bottom of the table to fetch and display all rows.

   .. important::

      When viewing table data, Data Studio automatically adjusts the column widths for a good table view. Users can resize the columns as needed. If the text length exceeds the column width and you adjust the column width, Data Studio may fail to respond.

.. note::

   -  Each time a query is run in **SQL Terminal** tab, a new result window opens. To view the results in the new window, you must select the newly opened window.
   -  Set the **focusOnFirstResult** configuration parameter to **false** to automatically set focus to the newly opened **Result** window. For details, see :ref:`Installing and Configuring Data Studio <dws_ds_16>`.
   -  Each row, column and selected cells can be copied from the result set.
   -  Export all data operation will be successful even after the connection is removed.
   -  If the text of a column contains spaces, word wrapping is applied to fit the column width. Word wrapping is not applied to columns without spaces.
   -  Select part of cell content and press **Ctrl+C** or click |image36| to copy selected text from a cell.
   -  The size of the column is determined by the maximum content length column.
   -  You can save preference to define:

      -  Number of records to be fetched

      -  Column width

      -  Copy option from result set

         For details, see :ref:`Query Results <en-us_topic_0000001188202558__en-us_topic_0185264923_section28611419201210>`.

   -  If any column of resultset tab has Lock Image icon in it, then values are not editable.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section7637071471:

Backuping Unsaved Queries/Functions/Procedures
----------------------------------------------

Data Studio creates back up of unsaved data in SQL Terminal and PL/SQL Viewer periodically based on the time interval defined in the **Preferences** tab. The data can be encrypted and saved based on **Preference** settings. Refer to :ref:`Query/Function/Procedure Backup <en-us_topic_0000001234200631__en-us_topic_0185264709_section1980415371926>` to turn on/off backup, define time interval to save the data, and encrypt the saved data.

Unsaved changes of each SQL Terminal/PL/SQL Viewer are taken as backup and stored in **DataStudio\\UserData\\<user name>\\Autosave folder.** Backup files saved before unexpected shutdown of Data Studio will be available at next login.

In case there are unsaved data in SQL Terminal/PL/SQL Viewer, during graceful exit, Data Studio will wait for backup to complete before closing.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section0486740204318:

Error Locator
-------------

During execution of query/function/procedure in case of an error the error locator message is displayed.

**Yes** - Click **Yes** to continue with the execution.

**No** - Click **No** to stop the execution.

You can select **Do not display other errors that occur during the execution** to hide the error messages and proceed with the current SQL query.

Line number and position of error displays in **Messages** tab. The corresponding line number is marked with |image37| icon along with red underline at the position of the error in the Terminal/PL/SQL Viewer. Hovering over |image38| displays the error message. For details about why the line number does not match the error detail, see :ref:`FAQs <dws_ds_155>`.

.. note::

   If the query/function/procedure is modified while execution is in progress, then error locator may not display the correct line and position number.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section942319263275:

Search in PL/SQL Viewer or SQL Terminal
---------------------------------------

Follow the steps below to search in PL/SQL Viewer or SQL Terminal:

**F3** key is used to search next word and **Shift+F3** key is used to search previous word. These shortcut keys will be enabled only after **Ctrl+F** is used to search a text. These keys will be active with the current search word until a new word is searched. The value searched using **Ctrl+F** and **F3/Shift+F3** will be applicable only for the current instance.

#. Choose **Edit > Find** **and Replace** from the main menu.

   Alternatively press **Ctrl+F**.

   **Find and Replace** dialog box is displayed.

#. Enter the text to be searched for in the **Find what** field, and click the **Find Next** button.

   The desired text is highlighted.

   **F3** and **Shift+F3** key will now be enabled for forward and backward search.

   .. note::

      Select **Wrap around** option to continue the search after reaching the last line in the SQL queries or PL/SQL statements.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section295082372218:

Go to Line in PL/SQL Viewer or SQL Terminal
-------------------------------------------

Go to line option is used to skip to a specific line in the terminal.

Follow the steps below to go to a line in PL/SQL Viewer or SQL Terminal:

#. Choose **Edit** > **Go To Line** from the main menu or press **Ctrl+G**.

   The **Go To Line** dialog box is displayed.

#. Enter the desired number in the **Enter the line number** field, and then click the **OK** button.

   The cursor moves to the beginning of the line entered in the **Go to Line** dialog box.

   .. note::

      Below are invalid inputs to this field.

      -  Non-numeric value
      -  Special characters
      -  Line number entered does not exist in the editor.
      -  More than 10 digits is entered.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section134691211285:

Comment/Uncomment
-----------------

Comment/uncomment option is used to comment/uncomment lines or block of lines.

Follow the steps below to comment/uncomment lines in PL/SQL Viewer or SQL Terminal:

#. Select the lines to comment/uncomment.

#. Choose **Edit** option. Choose **Comment/Uncomment Lines** from the main menu.

   Alternatively, press **Ctrl+/** or right-click a line and select **Comment/Uncomment Lines**.

Follow the steps below to comment/uncomment block of lines/content in PL/SQL Viewer or SQL Terminal:

#. Select the lines/content to comment/uncomment.

#. Choose **Edit** option. Choose **Comment/Uncomment Block** from the main menu.

   Alternatively, press **Ctrl+Shift+/** or right-click a line or the entire block and select **Comment/Uncomment Block**.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section2611012152811:

Indent/Un-indent Lines
----------------------

The indent/un-indent option is used to shift lines as per the indent size defined in the **Preferences** tab.

Follow the steps to indent lines in PL/SQL Viewer or SQL Terminal:

#. Select the lines to indent.

#. Press **Tab** or click |image39|.

   Shift the selected line as per the indent size defined in the **Preferences** tab. For details about modifying the indent size, see :ref:`Formatter <en-us_topic_0000001188521052__en-us_topic_0185264581_section64411944205615>`.

Follow the steps to un-indent lines in PL/SQL Viewer or SQL Terminal:

#. Select the lines to un-indent.

#. Press **Shift+Tab** or click |image40|.

   Move the selected lines according to the indent size defined in **Preferences**. For details about modifying the indent size, see :ref:`Formatter <en-us_topic_0000001188521052__en-us_topic_0185264581_section64411944205615>`.

   .. note::

      Only selected lines that have available tab space will be un-indented. For example, if multiple lines are selected, and one of the selected lines starts at position 1, then pressing **Shift+Tab** will un-indent all the lines except for the one starting at position 1.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section16493161471713:

Insert Space
------------

The **Insert Space** option is used to replace a tab with spaces based on the indent size defined in the **Preferences** tab.

Follow the steps below to replace a tab with spaces in PL/SQL Viewer or SQL Terminal:

#. Select the lines to replace tab with spaces.

#. Press **Tab** or **Shift+Tab**.

   Replaces the tab with spaces as per the indent size defined in the **Preferences** tab. For details about modifying the indent size, see :ref:`Formatter <en-us_topic_0000001188521052__en-us_topic_0185264581_section64411944205615>`.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section1691612421411:

Execute Multiple Functions/Procedures or Queries
------------------------------------------------

Follow the steps below to execute multiple functions/procedures:

Insert a forward slash (/) in a new line after the function/procedure in the **SQL Terminal**.

Add the new function/procedure in the next line.

|image41|

Follow the steps below to execute multiple SQL queries:

#. Enter multiple SQL queries in the **SQL Terminal** tab as follows:

   |image42|

#. Click |image43| in the **SQL Terminal** tab, or press **Ctrl+Enter**, or choose **Run > Compile/Execute Statement** from the main menu.

   .. note::

      -  If the queries are not selected for execution, then only the query in the line where cursor is placed will be executed.
      -  If the cursor is placed next to an empty line, then the next available query statement will be executed.
      -  If the cursor is placed at the last line which is blank, then no query will be executed.
      -  If a single query is written in multiples lines and the cursor is placed at any line of the query, then that query is executed. Queries are separated using semicolon (;).

Do as follow to execute an SQL query after a function/procedure:

Insert a forward slash (/) in a new line after the function/procedure and click |image44| in the **SQL Terminal** tab.

Do as follow to execute PL/SQL statements and SQL queries on different connections:

In the toolbar, select the required connection from the connection profiles drop-down list and click |image45| in the **SQL Terminal** tab.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section46225611153:

Rename SQL Terminal
-------------------

Follow the steps below to rename SQL Terminal:

#. In the **SQL Terminal** tab right-click and select **Rename** **Terminal**.

   A **Rename** **Terminal** dialog box is displayed prompting you to provide the new name for the Terminal.

#. Enter the new name and select **OK** to rename the Terminal.

   .. note::

      -  Terminal name follows Windows file naming convention.
      -  **Rename Terminal** allows a maximum of 150 characters.
      -  Restore option is not available to revert to the default name. You must manually rename the Terminal to default name.
      -  Tool tip of the renamed Terminal will display the old name.

SQL Assistant
-------------

The **SQL Assistant** tool provides suggestion or reference for the information entered in **SQL Terminal** and **PL/SQL Viewer**. Follow the steps to open SQL Assistant:

When Data Studio is launched **SQL Assistant** panel displays with related syntax topics. As you type a query in the SQL Terminal topics related to the query is displayed. It also provides precautions, examples, syntax, function, and parameter description. Select the text and use the right-click option to copy selected information or copy and paste to SQL Terminal.

.. note::

   -  Choose **Settings** > **Preferences** > **Environment** > **Session Setting**. In the **SQL Assistant** area, enable or disable the **SQL Assistant** function permanently. By default, the **SQL Assistant** function is enabled permanently.
   -  After the **SQL Assistant** function is enabled, you can click the **SQL Assistant** icon (|image46|) on the toolbar to open the **SQL Assistant** window. If the **SQL Assistant** icon is gray after the **SQL Assistant** is enabled, the **SQL Assistant** is invalid.

.. _en-us_topic_0000001234200573__en-us_topic_0185264856_section2303144411114:

Using Templates
---------------

Data Studio provides an option to insert frequently used SQL statements in **SQL Terminal** or **PL/SQL Viewer** using the **Templates** option. Some of the commonly used SQL statements are saved for ease of use. You can create, modify existing templates or remove templates. Refer to :ref:`Adding/Modifying Templates <en-us_topic_0000001188521052__en-us_topic_0185264581_section116501350276>` section for information on adding, removing, and creating new templates.

The following table lists the default templates:

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

Follow the steps to use the **Templates** option:

#. Enter the name of the template in SQL Terminal/PL/SQL Viewer.

#. Press **Alt+Ctrl+Space**.

   A list of saved template information is displayed. The list displayed is based on the following criteria:

   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
   | Exact Match                       | Display List                                                                                                             |
   +===================================+==========================================================================================================================+
   | On                                | Displays all entries that match the input text case.                                                                     |
   |                                   |                                                                                                                          |
   |                                   | **Example:** Entering "SF" in SQL Terminal/PL/SQL Viewer displays all entries that start with "SF".                      |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
   | Off                               | Displays all entries that match the input irrespective of the text case.                                                 |
   |                                   |                                                                                                                          |
   |                                   | **Example:** Entering "SF" in SQL Terminal/PL/SQL Viewer displays all entries that start with "SF", "Sf", "sF", or "sf". |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------+

   +-------------------------------------------------+-------------------------------------------------------------------------------------------------------+
   | Text Selection/Cursor Location                  | Display List                                                                                          |
   +=================================================+=======================================================================================================+
   | A text is selected and the shortcut key is used | Displays entries that match the text before the selection to the nearest space or new line character. |
   +-------------------------------------------------+-------------------------------------------------------------------------------------------------------+
   | No text selected and the shortcut key is used   | Displays entries that match the text before the cursor to the nearest space or new line character.    |
   +-------------------------------------------------+-------------------------------------------------------------------------------------------------------+

   .. note::

      -  Using the shortcut key without entering text in SQL Terminal/PL/SQL Viewer displays all entries in the **Templates**.
      -  If the text entered in SQL Terminal/PL/SQL Viewer has only a single match, then it will be replaced directly in the SQL Terminal/PL/SQL Viewer without listing them out.

.. |image1| image:: /_static/images/en-us_image_0000001188521188.png
.. |image2| image:: /_static/images/en-us_image_0000001234200699.png
.. |image3| image:: /_static/images/en-us_image_0000001234200713.png
.. |image4| image:: /_static/images/en-us_image_0000001234042223.png
.. |image5| image:: /_static/images/en-us_image_0000001188362636.png
.. |image6| image:: /_static/images/en-us_image_0000001234042399.png
.. |image7| image:: /_static/images/en-us_image_0000001233922257.png
.. |image8| image:: /_static/images/en-us_image_0000001188362624.png
.. |image9| image:: /_static/images/en-us_image_0000001188362626.png
.. |image10| image:: /_static/images/en-us_image_0000001188521174.png
.. |image11| image:: /_static/images/en-us_image_0000001234042219.png
.. |image12| image:: /_static/images/en-us_image_0000001233800775.png
.. |image13| image:: /_static/images/en-us_image_0000001188202656.png
.. |image14| image:: /_static/images/en-us_image_0000001188681098.png
.. |image15| image:: /_static/images/en-us_image_0000001188681110.png
.. |image16| image:: /_static/images/en-us_image_0000001188202662.jpg
.. |image17| image:: /_static/images/en-us_image_0000001188202670.jpg
.. |image18| image:: /_static/images/en-us_image_0000001233922259.jpg
.. |image19| image:: /_static/images/en-us_image_0000001188681108.jpg
.. |image20| image:: /_static/images/en-us_image_0000001234200711.png
.. |image21| image:: /_static/images/en-us_image_0000001234042217.jpg
.. |image22| image:: /_static/images/en-us_image_0000001233922269.png
.. |image23| image:: /_static/images/en-us_image_0000001234042225.png
.. |image24| image:: /_static/images/en-us_image_0000001188521184.png
.. |image25| image:: /_static/images/en-us_image_0000001188362634.jpg
.. |image26| image:: /_static/images/en-us_image_0000001188202660.png
.. |image27| image:: /_static/images/en-us_image_0000001188362628.png
.. |image28| image:: /_static/images/en-us_image_0000001188202658.png
.. |image29| image:: /_static/images/en-us_image_0000001233922263.png
.. |image30| image:: /_static/images/en-us_image_0000001188681100.png
.. |image31| image:: /_static/images/en-us_image_0000001188362640.png
.. |image32| image:: /_static/images/en-us_image_0000001234042221.png
.. |image33| image:: /_static/images/en-us_image_0000001188681102.png
.. |image34| image:: /_static/images/en-us_image_0000001234042221.png
.. |image35| image:: /_static/images/en-us_image_0000001188681102.png
.. |image36| image:: /_static/images/en-us_image_0000001234200705.jpg
.. |image37| image:: /_static/images/en-us_image_0000001188202848.png
.. |image38| image:: /_static/images/en-us_image_0000001188681304.png
.. |image39| image:: /_static/images/en-us_image_0000001234042415.jpg
.. |image40| image:: /_static/images/en-us_image_0000001233800781.jpg
.. |image41| image:: /_static/images/en-us_image_0000001188681094.jpg
.. |image42| image:: /_static/images/en-us_image_0000001188521186.jpg
.. |image43| image:: /_static/images/en-us_image_0000001234042407.png
.. |image44| image:: /_static/images/en-us_image_0000001188521366.png
.. |image45| image:: /_static/images/en-us_image_0000001234200907.png
.. |image46| image:: /_static/images/en-us_image_0000001234042209.png
