:original_name: DWS_DS_141.html

.. _DWS_DS_141:

Result Management
=================

This section describes how to customize the settings in the **Query Results** pane, including the column width, number of records to be obtained, and copy of column headers or row numbers.

.. _en-us_topic_0000001145713115__en-us_topic_0185264923_section28611419201210:

Query Results
-------------

Configure the column width of query results:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

2. Choose **Result Management** > **Query Results**.

   The **Query Results** pane is displayed.

3. Select the required option.

   The options of configuring the column width are as follows.

   +-----------------------------------+-------------------------------------------------------------------------------+
   | Option                            | Outcome                                                                       |
   +===================================+===============================================================================+
   | Content Length                    | You can set the column width based on the content length of the query result. |
   +-----------------------------------+-------------------------------------------------------------------------------+
   | Custom Length                     | You can customize the column width.                                           |
   |                                   |                                                                               |
   |                                   | .. note::                                                                     |
   |                                   |                                                                               |
   |                                   |    This value ranges from 100 to 500.                                         |
   +-----------------------------------+-------------------------------------------------------------------------------+

4. Click **OK**.

   .. note::

      Click **Restore Defaults** in **Query Results** to restore the default value. The default value is **Content Length**.

Set the number of records to be obtained in the query results:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Result Management** > **Query Results**.

   The **Query Results** pane is displayed.

#. Select the required option.

   +-----------------------------------+------------------------------------------------------------------------+
   | Option                            | Outcome                                                                |
   +===================================+========================================================================+
   | Fetch All records                 | You can obtain all records in the query results.                       |
   +-----------------------------------+------------------------------------------------------------------------+
   | Fetch custom number of records    | You can set the number of records to be obtained in the query results. |
   |                                   |                                                                        |
   |                                   | .. note::                                                              |
   |                                   |                                                                        |
   |                                   |    This value ranges from 100 to 5000.                                 |
   +-----------------------------------+------------------------------------------------------------------------+

#. Click **OK**.

   .. note::

      Click **Restore Defaults** in **Query Results** to restore the default value. The default value is **Fetch custom number of records (1000)**.

Copy column headers or row numbers from query results:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Result Management** > **Query Results**.

   The **Query Results** pane is displayed.

#. Select the required option.

   +-----------------------+-------------------------------------------------------------------------------------+
   | Option                | Outcome                                                                             |
   +=======================+=====================================================================================+
   | Include column header | You can copy column headers from the query results.                                 |
   +-----------------------+-------------------------------------------------------------------------------------+
   | Include row number    | You can copy the selected content along with the row number from the query results. |
   +-----------------------+-------------------------------------------------------------------------------------+

#. Click **OK**.

   .. note::

      Click **Restore Defaults** in **Query Results** to restore the default value. The default value is **Include column header**.

Determine how the result set window is opened:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Result Management** > **Result Window**.

#. Select the required option.

   +---------------------+----------------------------------------------------------------------------------------+
   | Option              | Outcome                                                                                |
   +=====================+========================================================================================+
   | Overwrite Resultset | After an opened result set window is closed, a new result set window will be opened.   |
   +---------------------+----------------------------------------------------------------------------------------+
   | Retain Current      | After a new result set window is opened, the opened result set windows are not closed. |
   +---------------------+----------------------------------------------------------------------------------------+

#. Click **OK**.

.. _en-us_topic_0000001145713115__en-us_topic_0185264923_section17814637103210:

Edit Table Data
---------------

Perform the following steps to determine the data saving mode:

#. Choose **Settings** > **Preferences** from the main menu. The **Preferences** dialog box is displayed.

#. Choose **Result Management** > **Edit Table Data**. The **Edit Table Data** pane is displayed.

   Select the required option:

   .. _en-us_topic_0000001145713115__en-us_topic_0185264923_table169131912115:

   .. table:: **Table 1** Editing table data

      +---------------+-------------+------------------+------------------------+------------------------------------------------------------------------------------------------------------+
      | Database Type | Auto Commit | Reuse Connection | Table Data Save Option | Behavior                                                                                                   |
      +===============+=============+==================+========================+============================================================================================================+
      | GaussDB(DWS)  | ON          | ON               | Save Valid Data        | Only the valid data will be saved and committed.                                                           |
      +---------------+-------------+------------------+------------------------+------------------------------------------------------------------------------------------------------------+
      |               | ON          | ON               | Do Not Save            | No data will be saved when an error occurs.                                                                |
      +---------------+-------------+------------------+------------------------+------------------------------------------------------------------------------------------------------------+
      |               | ON          | OFF              | Save Valid Data        | Only the valid data will be saved and committed.                                                           |
      +---------------+-------------+------------------+------------------------+------------------------------------------------------------------------------------------------------------+
      |               | ON          | OFF              | Do Not Save            | No data will be saved when an error occurs.                                                                |
      +---------------+-------------+------------------+------------------------+------------------------------------------------------------------------------------------------------------+
      |               | OFF         | ON               | Save Valid Data        | No data will be saved when an error occurs. Execute the **COMMIT** or **ROLLBACK** statement to save data. |
      +---------------+-------------+------------------+------------------------+------------------------------------------------------------------------------------------------------------+
      |               | OFF         | ON               | Do Not Save            | No data will be saved when an error occurs. Execute the **COMMIT** or **ROLLBACK** statement to save data. |
      +---------------+-------------+------------------+------------------------+------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

   .. note::

      Click **Restore Defaults** in **Edit Table Data** to restore the default value. The default value is **Save Valid Data**.

.. _en-us_topic_0000001145713115__en-us_topic_0185264923_section7921102619295:

Result Data Encoding
--------------------

Perform the following steps to

configure whether data encoding type is displayed in the **Query Results**, **View Table Data**, and **Edit Table Data** panes.

#. .. _en-us_topic_0000001145713115__en-us_topic_0185264923_li1083312422094:

   Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Result Management** > **Query Results**.

   The **Query Results** pane is displayed.

#. Select **Include result data encoding** to display the **Encoding** drop-down list in the **Query Results**, **View Table Data**, and **Edit Table Data** panes.

#. Click **OK**.

   .. note::

      -  Click **Restore Defaults** in **Result Management** to restore the default value. **Include result data encoding** will be unselected by default.
      -  To make the change take effect, you need to edit a table, view table properties, or execute a query again.
