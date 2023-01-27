:original_name: DWS_DS_98.html

.. _DWS_DS_98:

Exporting Table Data
====================

Perform the following steps to export table data:

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
   -  **Encoding** (optional): This option will be pre-populated with the encoding options made in **Preferences** > **Session Setting**.
   -  **Delimiter**: Use this option to define delimiters. You can select the provided delimiters or customize delimiters in **Delimiter** > **Other**. The default delimiter for CSV and TXT formats is commas (,). The **Other** field can contain a maximum of 10 bytes. By default, this option is selected when a CSV or TXT file is exported, but it is not mandatory. This option will be disabled for Excel (xlsx/xls) and binary formats. It is mandatory when the **Other** field is selected.
   -  **All Columns**: Use this option to quickly select all columns. This option is selected by default. To manually select columns, deselect this option and select the columns to export from the **Available Columns** list.

      -  **Available Columns**: Use this option to select the columns to export.
      -  **Selected Columns**: This option displays the selected columns to export. The column sequence can be adjusted. By default, all columns are displayed.

         .. note::

            :ref:`Column/Row Size <en-us_topic_0000001098993156__en-us_topic_0185264547_li25618341380>` in :ref:`FAQ <dws_ds_155>` describes the row and column size supported by xlsx and xls.

   -  **File Name**: Use this option to specify the name of the exported file. By default, the table name is displayed in this option.

      .. note::

         The file name follows the Windows file naming convention.

   -  **Output Path**: Use this option to select the location where the exported file is saved. The **Output Path** field is auto-populated with the selected path.
   -  **Security Disclaimer**: This option displays the security disclaimer. To continue the export operation, you need to read and agree to the disclaimer.

      -  **I Agree**: By default this option is selected. You cannot proceed if this option is deselected.
      -  **Do not show again**: You can select this option to hide the **Security Disclaimer** for the subsequent table data export from the current Data Studio instance.

   .. note::

      -  Character, double, date, calendar, and Boolean data types will be stored in the Excel format. All other data types will be converted into strings and stored in the Excel format.
      -  To export an Excel file, if a cell contains more than 32,767 characters, the data exported to the cell will be truncated.

#. Complete the required fields and click **OK**.

   The **Save As** dialog box is displayed.

#. Click **Save** to save the exported data in the selected format. The status bar displays the operation progress.

   The **Data Exported Successfully** dialog box and status bar displays the status of the completed operation.

   .. note::

      -  If the disk is full during table export, Data Studio displays an I/O error. Perform the following operations to rectify this error:

         a. Click **OK** to disable the database connection.
         b. Clean up disk space.
         c. Create the connection again and export the table data.

      -  If the file name contains characters that are not supported by Windows, the file name will be different from the table name.

.. _en-us_topic_0000001098673338__en-us_topic_0185264892_ref471314989:

Canceling Table Data Export
---------------------------

Perform the following steps to cancel table data export:

#. Double-click the status bar to open the **Progress View** tab.

#. In the **Progress View** tab, click |image1|.

#. In the **Cancel Operation** dialog box, click **Yes**.

   The **Messages** tab and status bar display the status of the canceled operation.

.. |image1| image:: /_static/images/en-us_image_0000001098833474.jpg
