:original_name: DWS_DS_98.html

.. _DWS_DS_98:

Managing Table Data
===================

After creating a table, you can query, edit, and analyze the table and table data.

.. _en-us_topic_0000001860318649__section13064991419:

Viewing Table Data
------------------

Right-click the selected table and select **View Table Data**. The **View Table Data** tab is displayed where you can view the table data information.

|image1|

Toolbar menu in the **View Table Data** window:

+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Toolbar Name         | Description                                                                                                                                                                                                                                                                        |
+======================+====================================================================================================================================================================================================================================================================================+
| Copy                 | Click the icon to copy selected content from **View Table Data** window to clipboard. Shortcut key - **Ctrl+C**.                                                                                                                                                                   |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Advanced Copy        | Click the icon to copy content from result window to the clipboard. Results can be copied to include the row number and/or column header. Refer to :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>` to set this preference. The shortcut key is **Ctrl+Shift+C**. |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Show/Hide Search Bar | Click the icon to display/hide the search text field. This is a toggle button.                                                                                                                                                                                                     |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Encoding             | Refer to :ref:`Execute SQL Queries <en-us_topic_0000001860318949__en-us_topic_0185264856_section16147111413113>` for information on encoding selection.                                                                                                                            |
+----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Icons in the Search field:

+-------------------+----------------------------------------------------------------------------------------------------------------+
| Icon Name         | Description                                                                                                    |
+===================+================================================================================================================+
| Search            | Click the icon to search the table data displayed based on the criteria defined. The text is case-insensitive. |
+-------------------+----------------------------------------------------------------------------------------------------------------+
| Clear Search Text | Click the icon to clear the search texts entered in the search field.                                          |
+-------------------+----------------------------------------------------------------------------------------------------------------+

Refer to :ref:`Execute SQL Queries <en-us_topic_0000001860318949__en-us_topic_0185264856_section16147111413113>` for column reordering and sorting options.

-  **Query Submit Time**: Provides the query submitted time.
-  Number of rows fetched with execution time is displayed. The default number of rows is displayed. If there are additional rows to be fetched, then it will be denoted with the word "more". You can scroll to the bottom of the table to fetch and display all rows.

   .. important::

      -  When you view table data, Data Studio automatically adjusts the column widths for an optimal table view. You can resize the columns as needed. If the text content of a cell exceeds the total available display area, then resizing the cell column may cause DS to become unresponsive.

      -  When the data in a table cell is more than 1,000 characters, it will be trimmed to 1,000 characters with "..." at the end.

.. note::

   -  Individual table data window is displayed for each table.
   -  If the data of the table that is already opened is modified, then refresh and open the table data again to view the updated information on the same opened window.
   -  While the data is loading a message displays at the bottom stating "fetching".
   -  If the text of a column contains spaces, word wrapping is applied to fit the column width. Word wrapping is not applied to columns without spaces.
   -  Select part of cell content and press **Ctrl**\ +\ **C**.
   -  The size of a column is determined by the maximum content length in the column.

.. _en-us_topic_0000001860318649__section1226872111129:

Editing Table Data
------------------

Right-click the selected table and select **Edit Table Data**. The **Edit Table** data tab is displayed.

Refer to :ref:`Viewing Table Data <en-us_topic_0000001860318649__section13064991419>` for description on copy and search toolbar options.

|image2|

Data Studio validates only the following data types entered into cells: Bigint, bit, boolean, char, date, decimal, double, float, integer, numeric, real, smallint, tinyint, and varchar.

Editing of array data, time with time zone, and time stamp with time zone are not supported.

Any related errors during this operation reported by database will be displayed in Data Studio.

Reindexing a Table
------------------

Index facilitates lookup of records. You need to reindex tables in the following scenarios:

-  The index is corrupted and no longer contains valid data. Although in theory this will never happen, in practice, indexes can become corrupted due to software bugs or hardware failures. Reindexing provides a recovery method.
-  An index in the GaussDB(DWS) database may have numerous empty or nearly empty pages when accessed in an unconventional manner. By using the REINDEX mode, the space used by the index can be reduced. This mode creates a new index without any empty pages.
-  You have altered a storage parameter (such as the fill factor) for an index, and wish to ensure that the change has taken full effect.

Follow the steps below to reindex a table:

Right-click the selected table and select **Reindex Table**. A pop-up message and the status bar display the status of the completed operation.

.. note::

   This operation is not supported for Partition ORC tables.

Analyzing a Table
-----------------

The analyzing table operation collects statistics about tables and table indexes and stores the collected information in internal tables of the database where the query optimizer can access the information and use it to help make better query planning choices.

Right-click the selected table and select **Analyze Table**. The **Analyze Table** message and status bar displays the status of the completed operation.

Truncating a Table
------------------

This operation deletes the all data from an existing table. Exercise caution when performing this operation.

Right-click the selected table and select **Truncate Table**. Data Studio prompts you to confirm this operation. In the confirmation dialog box, click **OK** to complete the operation successfully.

A pop-up message and the status bar display the status of the completed operation.

ER Diagrams
-----------

View table relationships, including primary keys, foreign keys, and associated tables with ER diagram.

|image3|

Vacuuming a Table
-----------------

Vacuuming table operation reclaims space and makes it available for re-use.

Right-click the selected table and select **Vacuum Table**. The **Vacuum Table** message and status bar display the status of the completed operation.

Setting the Table Description
-----------------------------

Right-click the selected table and select **Set Table Description**. The **Update Table Description** dialog box is displayed. It prompts you to set the table description.

|image4|

Enter the description and click **OK**. The status bar displays the status of the completed operation.

Setting a Schema
----------------

Right-click the selected table and select **Set Schema**. The **Set Schema** dialog box is displayed, prompting you to select the new schema for the selected table.

Select the schema name from the drop-down list and click **OK**. The selected table will be moved to the new schema.

|image5|

The status bar displays the status of the completed operation.

.. note::

   -  This operation is not supported for partitioned ORC tables.
   -  If the required schema contains a table with the same name as the current table, then Data Studio does not allow setting the schema for the table.

.. _en-us_topic_0000001860318649__section32742219385:

Exporting Table Data
--------------------

#. Right-click the selected table and select **Export Table Data**.

   The **Export Table Data** dialog box is displayed with the following options:

   -  **Format**: Table data can be exported in Excel (xlsx/xls), CSV, TXT, or binary format. The default format is Excel (xlsx/xls).
   -  **Include Header**: This option is available for CSV and TXT files. If this option is selected, the exported data will include the column headers. By default, this option is selected when a CSV or TXT file is exported, but it is not mandatory. This option will be disabled for Excel (xlsx/xls) and binary formats.
   -  **Quotes**: Use this option to define quotes. You can enter only a single-byte character for this option, and the value of **Quotes** should be different from that of **Delimiter**. By default, this option is selected when a CSV or TXT file is exported, but it is not mandatory. This option will be disabled for Excel (xlsx/xls) and binary formats.

      -  If table data includes delimiters, the character specified in this option will be used.
      -  If the value includes a quote, it will not be escaped by the same quote.
      -  If the result contains the values of multiple rows, it will be quoted by quotes.

   -  **Escape**: Use this option to define escape values. You can enter only a single-byte character for this option, and the value of **Escape** must be different from that of **Quotes**. By default, this option is selected when a CSV or TXT file is exported, but it is not mandatory. This option will be disabled for Excel (xlsx/xls) and binary formats.
   -  **Replace NULL with**: Use this option to replace the null value in the table with a specified string. This option contains a maximum of 100 characters, and cannot contain newline characters or carriage return characters. The value of this option must be different from the values of **Delimiter** and **Quotes**. By default, this option is selected when a CSV or TXT file is exported, but it is not mandatory. This option will be disabled for Excel (xlsx/xls) and binary formats.
   -  **Encoding** (optional): This option will be pre-populated with the encoding options made in the **Preferences > Session Setting** tab.
   -  **Delimiter**: Use this option to define delimiters. You can select the provided delimiters or customize delimiters in **Delimiter** > **Other**. The default delimiter for CSV and TXT formats is commas (,). The **Other** field can contain a maximum of 10 bytes. By default, this option is selected when a CSV or TXT file is exported, but it is not mandatory. This option will be disabled for Excel (xlsx/xls) and binary formats. It is mandatory when the **Other** field is selected.
   -  **All Columns**: Use this option to quickly select all columns. This option is selected by default. To manually select columns, deselect this option and select the columns to export from the **Available Columns** list.

      -  **Available Columns**: Use this option to select the columns to export.
      -  **Selected Columns**: This option displays the selected columns to export. The column sequence can be adjusted. By default, all columns are displayed in this option.

         .. note::

            The .xlsx format supports a maximum of 1 million rows and 16,384 columns. The .xls format supports a maximum of 64,000 rows and 256 columns.

   -  **File Name**: Use this option to specify the name of the exported file. By default, the table name is displayed in this option.

      .. note::

         The file name follows the Windows file naming convention.

   -  **Output Path**: Use this option to select the location where the exported file is saved. The **Output Path** field is auto-populated with the selected path.
   -  **Security Disclaimer**: This option displays the security disclaimer. To continue the export operation, you need to read and agree to the disclaimer.

      -  **I Agree**: By default this option is selected. You cannot proceed if this option is deselected.
      -  **Do not show again**: You can select this option to hide the **Security Disclaimer** for the subsequent table data export from the current Data Studio instance.

   .. note::

      -  String, double, date, calendar, and Boolean data will be stored in the Excel format. All other data types will be converted into strings and stored in the Excel format.
      -  To export an Excel file, if a cell contains more than 32767 characters, the data exported to the cell will be truncated.

#. Complete the required fields and click **OK**.

   The **Save As** dialog box is displayed.

#. Click **Save** to save the exported data in the selected format. The status bar displays the progress of the operation.

   The **Data Exported Successfully** dialog box and status bar displays the status of the completed operation.

   .. note::

      -  If the disk is full during table export, Data Studio displays an I/O error. Perform the following operations to rectify this error:

         a. Click **OK** to close the connection profile.
         b. Clean the disk.
         c. Re-establish the connection and export the table data.

      -  The exported file name will not be the same as table name, if the table name contains characters which are not supported by Windows.

Importing Table Data
--------------------

Prerequisites:

-  If the definition of the source file does not match that of the destination table, modify the properties of the destination table in the **Import Table Data** dialog box. Additional columns of the destination table will be inserted with default values.
-  You should know the export properties of the file to be imported, such as **Delimiter**, **Quotes**, and **Escape**. Export properties saved during data export cannot be changed when a file is being imported.

Perform the following steps to import table data:

#. Right-click the selected table and select **Import Table Data**.

   The **Import Table Data** dialog box is displayed with the following options:

   -  **Import Data File**: This option displays the path of the imported file. Click **Browse** to select other files.
   -  **Format**: Table data can be imported in CSV, TXT, or binary format. CSV is the default format.
   -  **Include Header**: Select this option if the imported file contains a column header. By default, this option is selected when a CSV or TXT file is exported, but it is not mandatory. This option will be disabled for the binary format.
   -  **Quotes**: You can enter only a single-byte character for this option, and the value of **Quotes** should be different from the null value of **Delimiter**. By default, this option is selected when a CSV or TXT file is exported, but it is not mandatory. This option will be disabled for the binary format.
   -  **Escape**: You can enter only a single-byte character for this option. If the value of **Escape** is the same as that of **Quotes**, the value of **Escape** will be replaced with **\\0**. This option defaults to double quotation marks (") when a CSV or TXT file is exported, but it is not mandatory. This option will be disabled for the binary format.
   -  **Replace with Null**: You can configure this option to replace the null value in the table with a string. The null string used for exporting data should be used for importing data, and the null string needs to be specified. By default, this option is selected when a CSV or TXT file is exported, but it is not mandatory. This option will be disabled for the binary format.
   -  **Encoding** (optional): This option will be pre-populated with the encoding options made in the **Preferences > Session Setting** tab.
   -  **Delimiter**: You can select the provided delimiters or customize delimiters in **Delimiter** > **Other**. The default delimiter for CSV and TXT formats is commas (,). The value of this option should be different from those of **Quotes** and **Replace with Null**. By default, this option is selected when a CSV or TXT file is exported, but it is not mandatory. This option will be disabled for the binary format. It is mandatory when the **Other** field is selected.
   -  **All Columns**: Use this option to quickly select all columns. This option is selected by default. To manually select columns, deselect this option and select the columns to export from the **Available Columns** list.

      -  **Available Columns**: Use this option to select the columns to export.
      -  **Selected Columns**: This option displays the selected columns to export. The column sequence can be adjusted. By default, all columns are displayed in this option.

#. Click **Browse** next to the **Import Data File** field.

   The **Open** dialog box is displayed.

#. In the **Open** dialog box, select the file to import and click **Open**.

#. Complete the required fields and click **OK**.

   The status bar displays the operation progress. The imported data will be added to the existing table data.

   The **Data Imported Successfully** dialog box and status bar display the status of the completed operation.

Show DDL
--------

Right-click the table, and then select **Show DDL**. Data Studio displays the DDL of the selected table.

.. note::

   -  A new terminal window is opened each time you select to show DDL.
   -  MS Visual C runtime file (msvcrt100.dll) is required to complete this operation. For details, see :ref:`Troubleshooting <en-us_topic_0000001813439016__en-us_topic_0185264982_li1161793674918>`.

Exporting Table DDL and Data
----------------------------

Follow the steps below to export the table DDL:

#. In the **Object Browser** pane, right-click the selected table and select **Export DDL and Data**.

   You need to customize the export path. To compress data, select **.zip**

   |image6|

   You must click **I agree** under **Security Disclaimer**, then click **OK**. You can disable the security disclaimer. After the disclaimer is disabled, it will not be displayed when you export the DDL. For details, see :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>`.

#. Click **OK**. The operation progress is displayed on the status bar in the lower right corner.

   .. note::

      -  If the table name contains characters which are not supported by Windows, the exported file name will not be the same as table name.
      -  MS Visual C runtime file (msvcrt100.dll) is required to complete this operation. For details, see :ref:`Troubleshooting <en-us_topic_0000001813439016__en-us_topic_0185264982_li1161793674918>`.

   The **Export** message and status bar display the status of the completed operation.

   .. table:: **Table 1** Supported DDL encoding formats

      ================= ============= ===========================
      Database Encoding File Encoding Support for Exporting a DDL
      ================= ============= ===========================
      UTF-8             UTF-8         Yes
      \                 GBK           Yes
      \                 LATIN1        Yes
      GBK               GBK           Yes
      \                 UTF-8         Yes
      \                 LATIN1        No
      LATIN1            LATIN1        Yes
      \                 GBK           No
      \                 UTF-8         Yes
      ================= ============= ===========================

   .. note::

      You can select multiple objects from regular and partitioned tables to export DDL and data, including columns, rows, indexes, constraints, and partitions.

Renaming a table
----------------

Right-click the selected table and select **Rename Table**. The **Rename Table** dialog box is displayed prompting you to provide the new name.

Enter the table name and click **OK**. You can view the updated table name in **Object Browser**.

.. note::

   This operation is not supported for partitioned ORC tables.

Dropping a Table
----------------

Right-click the selected table and select **Drop Table**. Press **Ctrl+left-click** (select objects one by one) or **Shift+left-click** (select objects in batches) to select the objects to be dropped.

This operation removes the complete table structure (including the table definition and index information) from the database and you have to re-create this table once again to store data.

Viewing Table Properties
------------------------

Right-click the selected table and select **Properties**.

Data Studio displays the properties (**General**, **Columns**, **Constraints**, and **Index**) of the selected table in different tabs. The following table lists the operations that can be performed on each tab along with data editing and refreshing operation. Edit operation is performed by double-clicking the cell.

.. important::

   When viewing table data, Data Studio automatically adjusts the column widths for table view. You can resize the columns as needed. If the text content of a cell exceeds the total available display area, then resizing the cell column may cause Data Studio to become unresponsive.

.. note::

   -  Individual property window is displayed for each table.
   -  If the property of a table is modified for a table that is already opened, refresh and open the properties of the table again to view the updated information on the same opened window.
   -  If the content of the column has spaces between the words, then word wrapping is applied to fit the column within the display area. Word wrapping is not applied if the content does not have any spaces between the words.
   -  The size of the column is determined by the maximum content length column.
   -  Any change made to the table properties from **Object Browser** will be reflected after refreshing the **Properties** tab.
   -  Pasting operation is not allowed in the **Data Type** column.

Grant/Revoke Privilege
----------------------

#. Right-click the selected regular/partitioned table and select **Grant/Revoke**.

   The **Grant/Revoke** dialog box is displayed.

   |image7|

#. Select the objects to grant/revoke privilege from the **Object Selection** tab and click **Next**.

   |image8|

#. Select the **role** from the Role drop-down list in the **Privilege Selection** tab. Select **Grant/Revoke** in the **Privilege Selection** tab.

#. On the **SQL Preview** tab, you can check the automatically generated SQL query.

#. Click **Finish**.

.. |image1| image:: /_static/images/en-us_image_0000001813439376.png
.. |image2| image:: /_static/images/en-us_image_0000001860319081.png
.. |image3| image:: /_static/images/en-us_image_0000001860319073.png
.. |image4| image:: /_static/images/en-us_image_0000001860199249.png
.. |image5| image:: /_static/images/en-us_image_0000001813439372.png
.. |image6| image:: /_static/images/en-us_image_0000001860319077.png
.. |image7| image:: /_static/images/en-us_image_0000001860199241.png
.. |image8| image:: /_static/images/en-us_image_0000001860199225.png
