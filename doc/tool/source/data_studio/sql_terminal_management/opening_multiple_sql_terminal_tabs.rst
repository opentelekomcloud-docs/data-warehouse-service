:original_name: DWS_DS_026.html

.. _DWS_DS_026:

Opening Multiple SQL Terminal Tabs
==================================

You can open multiple **SQL Terminal** tabs in Data Studio to execute multiple SQL statements for query in the current **SQL Terminal** tab. Perform the following steps to open a new **SQL Terminal** tab.

You can also open multiple **SQL Terminal** tabs on different connection templates.

#. In the **Object Browser** pane, right-click the desired database and choose **Open Terminal** from the shortcut menu. Alternatively, click **Open Terminal** in the toolbar or press **Ctrl+T** to open a new **SQL Terminal** tab.

   The **SQL Terminal** tab is displayed.

   The name format of the new **SQL Terminal** tab is as follows:

   *Database name*\ **@**\ *Connection information*\ **(**\ *Tab number*\ **)**, for example, *postgres*\ **@**\ *IDG_1*\ **(**\ *2*\ **)**. The number of each **SQL Terminal** tab in the same connection information is unique.

.. note::

   -  In Data Studio, a maximum of 100 **SQL Terminal** tabs can be opened for each connected database. Based on the number of query times, each **SQL Terminal** tab contains multiple **Result** tabs and one **Message** tab. If the database connection is lost, the corresponding **SQL Terminal** tab is still available.
   -  The restoration operation applies to all minimized **SQL Terminal** tabs. You cannot restore a single tab.
   -  After all terminals are shut down, Data Studio resets the counter of the SQL terminal.
   -  After all **Result Set** tabs are closed, Data Studio resets the counter of the result set.
   -  Data Studio allows you to reset counters in the following pages: **Display DDL Tablespace**, **Display DDL User/Role**, **Batch Delete**, **Result Set**, and **Execution Plan**.

Data Studio displays an error message indicating that no result is found in the status bar. The **Result** tab displays the successful execution results.

Right-Click Menus in the Result Tab
-----------------------------------

You can copy or export cell data to an Excel file and generate a SQL query file.

After the SQL query result is displayed in the **Result** tab, right-click the result. The following menu is displayed:

|image1|

Perform the following steps to add a row number and column header to the result set:

#. On the menu bar of Data Studio, click **Settings**.
#. Choose **Preferences**. Expand the **Result Management** tab and choose **Query Results**.
#. In the **Result Advanced Copy** area, select **Include column header** and **Include row number**.

The following table describes the right-click options.

.. table:: **Table 1** Right-click options

   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+
   | Option        | Sub-Item      | Description                                                                                                        |
   +===============+===============+====================================================================================================================+
   | Copy Data     | Copy          | Copies data in the selected cell.                                                                                  |
   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+
   |               | Advanced Copy | Copies data in the selected cell, row number, and column header based on the preference settings.                  |
   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+
   | Copy to Excel | Copy as xls   | Exports data of selected cells to an xls file, which contains a maximum of 64,000 rows and 256 columns.            |
   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+
   |               | Copy as xlsx  | Exports data of selected cells to an xlsx file, which contains a maximum of 1 million rows.                        |
   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+
   | Export        | Current Page  | Exports the table data on the current page.                                                                        |
   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+
   |               | All Pages     | Exports all tables.                                                                                                |
   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+
   | Generate SQL  | Selected Line | Selects data from the target table of the statement for inserting data to generate a SQL file.                     |
   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+
   |               | Current Page  | Selects data of the current page from the target table of the statement for inserting data to generate a SQL file. |
   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+
   |               | All Pages     | Selects all table data from the target table of the statement for inserting data to generate a SQL file.           |
   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+
   | Set Null      | ``-``         | Sets the cell data to **null**.                                                                                    |
   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+
   | Search        | ``-``         | Searches for data in the selected cell and displays all data that meets the search criteria.                       |
   +---------------+---------------+--------------------------------------------------------------------------------------------------------------------+

.. note::

   The preceding SQL files do not take effect for the result sets generated by queries that use **JOIN**, expressions, views, **SET** operators, aggregate functions, **GROUP BY** clauses, or column aliases.

Result Tab (Text View)
----------------------

You can view data in text mode in the **Result** tab.

In addition to the grid view, you can also copy and search for data in the text view.

Click |image2| to obtain the result in text mode.

.. note::

   Searching for data in multiple cells may cause the system to display incorrect text results, because all information needs to be copied to the **Search** pane in plain text.

Displaying Execution Progress Bar
---------------------------------

When a query is executed in the **SQL Terminal** pane, a progress bar is displayed to dynamically display the execution duration. After the query is complete, the time bar disappears. The total execution duration is displayed next to the time bar.

If you want to cancel the query, click **Cancel** next to the time bar.

The procedure is shown in the following figure.

|image3|

.. note::

   -  The **Cancel** button is deleted from the toolbar.
   -  The progress bar is also displayed when you compile or debug an object in the PL/SQL editor.
   -  The time format displayed in the progress bar is *w* **hour** *x* **minute** *y* **second** *z* **millisecond**.
   -  When queries are performed in batches in **SQL Terminal**, the progress bar displays the total time consumed by the queries.

Debugging Duration
------------------

During debugging, Data Studio displays the status bar, showing the last operation time and total debugging time of each debugging statement.

During the debugging, the last operation time and total debugging time of the terminal are updated continuously. The value of the total debugging time is the sum of the values of the last operation time.

This simplifies the search for time-consuming statements in the debugged object.

The procedure is shown in the following figure.

|image4|

.. note::

   Functions and procedures can be debugged.

.. |image1| image:: /_static/images/en-us_image_0000001813599324.png
.. |image2| image:: /_static/images/en-us_image_0000001860199401.png
.. |image3| image:: /_static/images/en-us_image_0000001813439536.jpg
.. |image4| image:: /_static/images/en-us_image_0000001860319241.png
