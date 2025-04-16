:original_name: DWS_DS_040.html

.. _DWS_DS_040:

FAQs
====

#. **What do I need to check if my connection fails?**

   **Answer**: Check the following items:

   -  Check whether **Connection Properties** are properly configured.
   -  Check whether the server version is compatible with the client version.
   -  Check whether the **database\\pg_hba.conf** file is correctly configured.
   -  Check whether the **Data Studio.ini** file is correctly configured.

#. **Why does the connection succeed when I try connecting to another server using an SSL certificate?**

   **Answer**: If the same SSL certificates are used by different servers, then the second connection will succeed because the certificates are cached.

   When you establish a connection with a different server using different SSL certificates, the connection will fail due to certificate mismatch.

#. **When I right-click on a function/procedure and refresh it in** **Object Browser, why does the function/procedure disappear?**

   **Answer**: This problem may occur if you drop a function/procedure and recreate it. In this case, refresh the parent folder to view the function/procedure in **Object Browser**.

#. **What do I do if a critical error occurs in a database session and operations cannot proceed?**

   **Answer**: Critical error may occur in some of the following cases. Check whether:

   -  The connection is left idle for a long time and has timed out.
   -  The server is running.
   -  The server has sufficient memory and whether the Out Of Memory (OOM) error is reported to the server.

#. **What is a constraint?**

   **Answer**: Constraints are used to deny the insertion of unwanted data in columns. You can create restrictions on one or more columns in any table. It maintains the data integrity of the table.

   The following constraints are supported:

   -  Primary Key constraint
   -  Unique Key constraint
   -  Check constraint

#. **What is an index?**

   **Answer**: An index is a copy of selected columns of data, from a table, that is designed to enable very efficient search. It also includes a low level disk block address or a direct link to the complete row of data it was copied from.

#. **What is the default encoding for Data Studio's files?**

   **Answer**: Exported, imported, and system files are encoded with the system's default encoding as configured in **Settings** > **Preferences**. The default encoding is UTF-8.

#. **When I try to open Data Studio, the system displays a message indicating that Data Studio does not support opening multiple instances. Why do I get this error message?**

   **Answer**: A user cannot open multiple instances in Data Studio.

#. **What do I do if a DDL statement running indefinitely and cannot be canceled?**

   **Answer**: This problem may occur if other DML/DDL operations are being performed on the same object. In this case, stop all the DML/DDL operations on the object and try again. If the problem persists, there may be another user performing DML/DDL operations on the object. Try again later. You can customize table data and check the operations in a transaction by following the instructions provided in :ref:`Data Studio GUI <en-us_topic_0000001860318645__section18536183110235>`.

#. **Why is the exported query result different from the data available on the Results tab?**

   **Answer**: When a result set data is exported, a new connection is used to execute the query again. The exported results may be different from the data on the **Result** tab.

#. **Why does last login information show "Last login details not available"?**

   **Answer**: This message is displayed when you connect to the database server of an earlier version or log in to the database for the first time after it is created.

#. **Why is the error marked incorrectly in SQL Terminal?**

   **Answer**: This problem occurs when the server returns an incorrect line number. You can view the error message on the **Message** tab and locate the correct row to rectify the fault.

#. **Will deleted columns be displayed after "Show DDL" or "Export DDL" operations?**

   **Answer**: Yes.

#. **Why cannot Data Studio be started after the -Xmx parameter is modified?**

   **Answer**: The value of **-Xmx** may be invalid. For details, see :ref:`Configuring Data Studio <dws_ds_007>`.

#. **How do I quickly switch to the desired tab if there are multiple tabs open?**

   **Answer**: If the number of opened tabs reaches a certain limit (depending on your screen resolution), the |image1| icon will be displayed at the end of the tab list. Click this icon and select the required tab from the drop-down list. If this icon is not available, use the tooltip to identify the tabs. You also search for a **SQL Terminal** tab by its name. For example:

   -  \*s, this displays all Terminal names that start with s.
   -  test, this displays all Terminal names that start with test.
   -  \*2, this displays all Terminal names that contain 2 in them.

#. **Why does the language not change after I change the language setting and restart Data Studio?**

   **Answer**: If the language you selected does not change after restarting, you can restart Data Studio manually to make the change take effect.

#. **Why does the last login details information not display?**

   **Answer**: At times the server returns an error while trying to fetch last login details. In such scenarios the last login pop-up message does not display.

#. **When viewing/exporting DDL, why does the Chinese text not show properly?**

   **Answer**: This happens if the SQL, DDL, object names or data contains Chinese text and the Data Studio file encoding is not set to **GBK**. To solve this, go to **Settings** > **Preferences** > **Environment** > **File Encoding** and set the encoding to **GBK**. The supported combinations of Database and Data Studio encoding for export operation are shown in :ref:`Table1 Supported combinations of file encoding <en-us_topic_0000001813598528__en-us_topic_0185264547_table061484013587>`.

   **To open/view the exported files in Windows Explorer**: Files exported with UTF-8 encoding can be opened/viewed by double-clicking it or by right-clicking on the file and selecting **Open**. Files exported with GBK encoding must be opened in Excel using the import external data feature (**Data** > **Get External Data** > From **Text**).

   .. _en-us_topic_0000001813598528__en-us_topic_0185264547_table061484013587:

   .. table:: **Table 1** Supported combinations of file encoding

      +-------------------+---------------------------+-----------------------------------------+-----------------------------------------+
      | Database Encoding | Data Studio File Encoding | Support for Chinese Text in Table Names | Support for English Text in Table Names |
      +===================+===========================+=========================================+=========================================+
      | GBK               | GBK                       | Yes                                     | Yes                                     |
      +-------------------+---------------------------+-----------------------------------------+-----------------------------------------+
      | GBK               | UTF-8                     | No - Incorrect details                  | No - Incorrect details                  |
      +-------------------+---------------------------+-----------------------------------------+-----------------------------------------+
      | UTF-8             | GBK                       | No - Export Fails                       | No - Incorrect details                  |
      +-------------------+---------------------------+-----------------------------------------+-----------------------------------------+
      | UTF-8             | UTF-8                     | Yes                                     | Yes                                     |
      +-------------------+---------------------------+-----------------------------------------+-----------------------------------------+
      | UTF-8             | LATIN1                    | No - Export Fails                       | Yes                                     |
      +-------------------+---------------------------+-----------------------------------------+-----------------------------------------+
      | SQL_ASCII         | GBK                       | Yes                                     | Yes                                     |
      +-------------------+---------------------------+-----------------------------------------+-----------------------------------------+
      | SQL_ASCII         | UTF-8                     | No - Incorrect details                  | No - Incorrect details                  |
      +-------------------+---------------------------+-----------------------------------------+-----------------------------------------+

#. **Why do I get the error message "Conversion between GBK and LATIN1 is not supported"?**

   **Answer**: This message occurs if the Data Studio and Database encoding selected are incompatible. To solve this, select the compatible encoding. Compatible encoding is shown in :ref:`Table 2 <en-us_topic_0000001813598528__en-us_topic_0185264547_table987163010538>`.

   .. _en-us_topic_0000001813598528__en-us_topic_0185264547_table987163010538:

   .. table:: **Table 2** Compatible encoding formats

      ========================= ================= =================
      Data Studio File Encoding Database Encoding Compatible or Not
      ========================= ================= =================
      UTF-8                     GBK               Yes
      \                         LATIN1            Yes
      \                         SQL_ASCII         Yes
      GBK                       UTF-8             Yes
      \                         LATIN1            No
      \                         SQL_ASCII         Yes
      SQL_ASCII                 UTF-8             Yes
      \                         LATIN1            Yes
      \                         GBK               Yes
      ========================= ================= =================

#. **Why is the PL/SQL procedure I compiled and executed is saved as PL/SQL function?**

   **Answer**: The database does not differentiate between PL/SQL function and procedure. All procedures in databases are functions. Hence PL/SQL procedure is saved as PL/SQL function.

#. **Why is that I am not able to edit the distribution key?**

   **Answer**: The database allows you to edit the distribution key only for the first insert operation.

#. **While editing table data if I do not enter a value for default value column, will the value be added by the database server?**

   **Answer**: Yes, the database server will add the value but the value will not be visible after save in the **Edit Table Data** tab. Use the refresh option from the **Edit Table Data** tab or re-open the table again to view the added default value(s).

#. **While modifying/deleting table data why do I get a pop-up stating that more than one matching row found?**

   **Answer**: This happens because there are additional rows detected for modification/deletion based on Custom Unique Key or All Columns selection. If Custom Unique Key is selected, then it will delete/modify the rows that have exact match of the data in the column selected for deletion/modification. If All Columns is selected, then it will delete/modify the rows that match data in all columns. Hence the duplicate records matching the Custom Unique Key or All Columns will be deleted/modified if Yes is selected. If No is selected, the row that is not saved will be marked for correction.

#. **When I right-click on a text box I see additional context menu options. Why does this happen?**

   **Answer**: The additional context menu options like Right to left Reading order, Show Unicode control characters and so on are provided by Windows 7 in case the keyboard you are using supports right to left and left to right input.

#. .. _en-us_topic_0000001813598528__en-us_topic_0185264547_li1037472864716:

   **What are the objects that are not supported for batch export DDL & DDL and Data operations?**

   **Answer**: Following objects are not supported for DDL & DDL and Data operations.

   **Export DDL:**

   Connection, database, foreign table, sequence, column, index, constraint, partition, function/procedure group, regular tables group, views group, schemas group, and system catalog group.

   **Export DDL and Data**

   Connection, database, namespace, foreign table, sequence, column, index, constraint, partition, function/procedure, view, regular tables group, schemas group, and system catalog group.

#. .. _en-us_topic_0000001813598528__en-us_topic_0185264547_li18661844113712:

   **Will the queries in SQL Terminal be committed if the resultset is modified and saved with Reuse Connection on and Auto Commit off?**

   **Answer**: No. Queries will only be committed when COMMIT command is executed in the Terminal.

   =========== ================ ===============
   Auto Commit Reuse Connection Resultset Save
   =========== ================ ===============
   On          On               Commit
   On          Off              Commit
   Off         On               Does not commit
   Off         Off              Not supported
   =========== ================ ===============

#. **When I query a temp table from a new SQL Terminal the resultset displays incorrect table details. Why does this happen?**

   **Answer**: When you query a temp table from a new SQL Terminal or with the **Reuse Connection** off, the resultset displays information of a regular/partition/foreign table, if a table with the same name as the temp table exists.

   .. note::

      If the **Reuse Connection** is **On**, the resultset displays information of the temp table even if another table with the same name exists.

#. **Which are the operations that are performed on a locked object does not run in the background but needs to be manually closed?**

   **Answer**: Following are the operations that do not run in background while the object is locked in another operation:

   ============================ =====================
   Operations
   ============================ =====================
   Renaming a table             Creating a constraint
   Setting schema on table      Creating an index
   Setting description in table Adding column
   Renaming a partition         ``-``
   ============================ =====================

#. **Do we have a limit on the column and row size while exporting table data to excel?**

   **Answer**: Yes, xlsx format supports maximum of 1 million rows and 16384 columns and xls format supports maximum of 64,000 rows and 256 columns.

#. **How Do I Delete Objects in Batches?**

   The batch drop operation allows you to drop multiple objects. This operation also applies to searched objects.

   .. note::

      -  Batch drop is allowed only for databases.
      -  An error is reported on batch dropping system objects, which cannot be dropped.

   Perform the following steps to batch drop objects:

   Press **Ctrl** and select objects one by one or press **Shift** and select objects in a bunch to select the objects to be dropped.

   Right-click and select **Drop Objects**.

   The **Drop Objects** tab displays the list of objects to be dropped.

   +-----------------------------------+------------------------------------------------------------+
   | Column Name                       | Description                                                |
   +===================================+============================================================+
   | Type                              | Displays information about the object type.                |
   +-----------------------------------+------------------------------------------------------------+
   | Name                              | Displays the name of the object.                           |
   +-----------------------------------+------------------------------------------------------------+
   | Query                             | Displays the query that will be executed to drop objects.  |
   +-----------------------------------+------------------------------------------------------------+
   | Type                              | Displays the status of the drop operation.                 |
   |                                   |                                                            |
   |                                   | -  To start: The drop operation has not been started.      |
   |                                   | -  In progress: The object is being dropped.               |
   |                                   | -  Completed: The drop operation has been completed.       |
   |                                   | -  Error: The object has not been dropped due to an error. |
   +-----------------------------------+------------------------------------------------------------+
   | Error Message                     | Displays the failure cause of a drop operation.            |
   +-----------------------------------+------------------------------------------------------------+

   Select the required parameters.

   +--------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Option       | Description                                                                                                                                                                                            |
   +==============+========================================================================================================================================================================================================+
   | Cascade      | The cascade drop operation is performed to drop dependent objects and attributes. The dropped dependent objects will be removed from **Object Browser** only after the refresh operation is performed. |
   +--------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Atomic       | The atomic drop operation is performed to drop all objects. If the operation fails, no objects will be dropped.                                                                                        |
   +--------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No selection | If neither **Cascade** nor **Atomic** is selected, no dependent objects are dropped.                                                                                                                   |
   +--------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   Click **Start**.

   **Runs**: displays the number of objects that are dropped from the object list

   **Errors**: displays the number of objects that are not dropped due to errors

   Click **Stop** or close the **Drop Objects** dialog box to stop the drop operation.

   For details about copy, advanced copy, show/hide search bar, sort, and column reorder options, see :ref:`Execute SQL Queries <en-us_topic_0000001860318949__en-us_topic_0185264856_section16147111413113>`.

   .. note::

      -  Select part of a cell and press **Ctrl+C** or click **Copy** to copy the selected text in the cell.
      -  When you select multiple objects in **Object Browser** to drop, a batch drop window is displayed and the object icons are enabled in the menu bar. If you disconnect the database, the icons will remain disabled even after reconnection. In this case, you need to reselect the objects to drop and the selected objects will be displayed in the new batch drop window.

#. **How Do I Grant or Revoke Permissions in Batches for a Specified Object?**

   You can select multiple objects at a time or search for the target objects.

   Press **Ctrl** and select objects one by one, or press **Shift** and select objects in batches, and choose **Grant/Revoke** from the shortcut menu.

   .. note::

      -  Only objects with the same schema and type can be granted or revoked in batches.
      -  This feature is only supported in OLAP, not in OLTP.

.. |image1| image:: /_static/images/en-us_image_0000001813439260.png
