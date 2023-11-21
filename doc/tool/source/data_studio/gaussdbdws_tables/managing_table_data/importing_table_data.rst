:original_name: DWS_DS_100.html

.. _DWS_DS_100:

Importing Table Data
====================

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

Canceling the Table Data Import Operation
-----------------------------------------

Perform the following steps to cancel the table data import operation:

#. Double-click the status bar to open the **Progress View** tab.

#. In the **Progress View** tab, click |image1|.

#. In the **Cancel Operation** dialog box, click **Yes**.

   The **Messages** tab and status bar display the status of the canceled operation.

.. |image1| image:: /_static/images/en-us_image_0000001188521198.jpg
