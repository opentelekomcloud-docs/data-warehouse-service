:original_name: DWS_DS_141.html

.. _DWS_DS_141:

Result Management
=================

This section provides details on how to personalize the column width, number of records to be fetched in the query results, and result copy of column header or row number using the **Query Results** setting.

.. _en-us_topic_0000001188202558__en-us_topic_0185264923_section28611419201210:

Query Results
-------------

Set column width of query results:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

2. Choose **Result Management > Query Results**.

   The **Query Results** pane is displayed.

3. Select the required option.

   Column Width customization options:

   +-----------------------------------+------------------------------------------------------------------------------------------------------+
   | Option                            | Outcome                                                                                              |
   +===================================+======================================================================================================+
   | Content Length                    | Selecting this option enables you to set the column width based on the content length of the column. |
   +-----------------------------------+------------------------------------------------------------------------------------------------------+
   | Custom Length                     | Selecting this option enables you to set the column width based on the value entered in this field.  |
   |                                   |                                                                                                      |
   |                                   | .. note::                                                                                            |
   |                                   |                                                                                                      |
   |                                   |    This column accepts value between 100 and 500.                                                    |
   +-----------------------------------+------------------------------------------------------------------------------------------------------+

4. Click **OK**.

   .. note::

      Click **Restore Defaults** from **Query Results** pane to reset to default values. The default value is **Content Length**.

Set the number of records to be fetched in the query results:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Result Management > Query Results**.

   The **Query Results** pane is displayed.

#. Select the required option.

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------+
   | Option                            | Outcome                                                                                                       |
   +===================================+===============================================================================================================+
   | Fetch All records                 | Selecting this option enables you to fetch all the records in the query results.                              |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------+
   | Fetch custom number of records    | Selecting this option enables you to set the number of records that needs to be fetched in the query results. |
   |                                   |                                                                                                               |
   |                                   | .. note::                                                                                                     |
   |                                   |                                                                                                               |
   |                                   |    This column accepts value between 100 and 5000.                                                            |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

   .. note::

      Click **Restore Defaults** from **Query Results** pane to reset to default values. The default value is **Fetch custom number of records (1000)**.

Set preference to copy column name and row number from query results:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Result Management > Query Results**.

   The **Query Results** pane is displayed.

#. Select the required option.

   +-----------------------+------------------------------------------------------------------------------------------------------------------+
   | Option                | Outcome                                                                                                          |
   +=======================+==================================================================================================================+
   | Include column header | Selecting this option enables you to copy column headers from the query results.                                 |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+
   | Include row number    | Selecting this option enables you to copy the selected content along with the row number from the query results. |
   +-----------------------+------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

   .. note::

      Click **Restore Defaults** from **Query Results** pane to reset to default values. The default value is **Include column header**.

Set preference to decide the behavior of opening up result set window/s:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Go to **Result Management > Result Window**.

#. Select the required option.

   +---------------------+------------------------------------------------------------------------------------+
   | Option              | Outcome                                                                            |
   +=====================+====================================================================================+
   | Overwrite Resultset | Current result set opened window/s are closed and new result set window is opened. |
   +---------------------+------------------------------------------------------------------------------------+
   | Retain Current      | New result set window/s are opened retaining already opened result set window/s.   |
   +---------------------+------------------------------------------------------------------------------------+

#. Click **OK**.

Edit Table Data
---------------

Set save behavior of edit table data operation:

#. Choose **Settings** > **Preferences** from the main menu. The **Preferences** dialog box is displayed.

#. Choose **Result Management > Edit Table Data**. The **Edit Table Data** pane is displayed.

   Select the required option:

   .. _en-us_topic_0000001188202558__en-us_topic_0185264923_table169131912115:

   .. table:: **Table 1** Edit table data

      +--------------+-------------+------------------+------------------------+----------------------------------------------------------------------------------------+
      | Server Type  | Auto Commit | Reuse Connection | Table Data Save Option | Behavior                                                                               |
      +==============+=============+==================+========================+========================================================================================+
      | GaussDB(DWS) | ON          | ON               | Save Valid Data        | All the valid data will be saved and committed. Incorrect data will be omitted.        |
      +--------------+-------------+------------------+------------------------+----------------------------------------------------------------------------------------+
      |              | ON          | ON               | Do Not Save            | If an error occurs , no data will be saved.                                            |
      +--------------+-------------+------------------+------------------------+----------------------------------------------------------------------------------------+
      |              | ON          | OFF              | Save Valid Data        | All the valid data will be saved and committed. Incorrect data will be omitted.        |
      +--------------+-------------+------------------+------------------------+----------------------------------------------------------------------------------------+
      |              | ON          | OFF              | Do Not Save            | If an error occurs, no data will be saved.                                             |
      +--------------+-------------+------------------+------------------------+----------------------------------------------------------------------------------------+
      |              | OFF         | ON               | Save Valid Data        | If an error occurs, no data will be saved. Perform Commit/Rollback to proceed further. |
      +--------------+-------------+------------------+------------------------+----------------------------------------------------------------------------------------+
      |              | OFF         | ON               | Do Not Save            | If an error occurs, no data will be saved. Perform Commit/Rollback to proceed further. |
      +--------------+-------------+------------------+------------------------+----------------------------------------------------------------------------------------+

#. Click **OK**.

   .. note::

      Click **Restore Defaults** from **Edit Table Data** pane to reset to default values. The default value is **Save Valid Data**.

.. _en-us_topic_0000001188202558__en-us_topic_0185264923_section7921102619295:

Result Data Encoding
--------------------

You can enable/disable to display the data encoding type in edit, view, and query results window.

Follow the steps to modify display of encoding option:

#. .. _en-us_topic_0000001188202558__en-us_topic_0185264923_li1083312422094:

   Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Result Management > Query Results**.

   The **Query Results** pane is displayed.

#. Select **Include result data encoding** to include the **Encoding** drop-down in edit, view, and query results table.

#. Click **OK**.

   .. note::

      -  Click **Restore Defaults** from **Result Management** pane to reset to default values. **Include result data encoding** will be unselected by default.
      -  Edit table, view table properties and query execution again to apply the changes.
