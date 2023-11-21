:original_name: DWS_DS_126.html

.. _DWS_DS_126:

Viewing the Query Execution Plan and Cost
=========================================

The execution plan shows how the table(s) referenced by the SQL statement will be scanned (plain sequential scan and index scan).

The SQL statement execution cost is the estimate at how long it will take to run the statement (measured in cost units that are arbitrary, but conventionally mean disk page fetches).

Follow the steps below to view the plan and cost for a required SQL query:

#. Enter the query or use an existing query in the **SQL Terminal** and click |image1| on the SQL Terminal toolbar to view explain plan.

   To view explain plan with analyze, click the drop-down from |image2|, select **Include Analyze**, and click |image3|.

   The **Execution Plan** opens in tree view format as a new tab at the bottom by default. The display mode has a tree shape and text style.

   .. note::

      The data shown in tree explain plan and visual explain may vary, since the execution parameters considered by both are not the same.

   Following are the parameters selected for explain plan with/without analyze and the columns displayed:

   .. table:: **Table 1** Explain plan options

      +---------------------------------------------+------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
      | Explain Plan Type                           | Parameters Selected                      | Columns                                                                                                                                  |
      +=============================================+==========================================+==========================================================================================================================================+
      | Include Analyze unchecked (default setting) | Verbose, Costs                           | Node type, startup cost, total cost, rows, width, and additional Info                                                                    |
      +---------------------------------------------+------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
      | Include Analyze checked                     | Analyze, Verbose, Costs, Buffers, Timing | Node type, startup cost, total cost, rows, width, Actual startup time, Actual total time, Actual Rows, Actual loops, and Additional Info |
      +---------------------------------------------+------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+

   Additional Info column includes, predicate information (filter predicate, hash condition), distribution key and output information along with the node type information.

   The tree view of plan categorizes nodes into 16 types. In tree view, each node will be preceded with corresponding type of icon. Following is the list of node categories with icons:

   .. table:: **Table 2** Node categories with Icon

      ================== =========
      Node Category      Icon
      ================== =========
      Aggregate          |image4|
      Group Aggregate    |image5|
      Function           |image6|
      Hash               |image7|
      Hash Join          |image8|
      Nested Loop        |image9|
      Nested Loop Join   |image10|
      Modify Table       |image11|
      Partition Iterator |image12|
      Row Adapter        |image13|
      Seq Scan on        |image14|
      Set Operator       |image15|
      Sort               |image16|
      Stream             |image17|
      Union              |image18|
      Unknown            |image19|
      ================== =========

   Hover over the highlighted cells to identify the heaviest, costliest, and slowest node. Cells will be highlighted only for tree view.

   If multiple queries are selected, explain plan with/without analyze will be displayed only for last query selected.

   Each time execution plan is executed, the plan opens in a new tab.

   If the connection is lost and the database is still connected in Object Browser, then **Connection Error** dialog box is displayed:

   -  **Yes** - The connection is reestablished and retrieves explain plan and cost.
   -  **No** - Disconnects database in Object Browser.

   Toolbar menu in the **Execution Plan** window:

   +--------------+--------------+--------------------------------------------------------------------------------------------------------+
   | Toolbar Name | Toolbar Icon | Description                                                                                            |
   +==============+==============+========================================================================================================+
   | Tree Format  | |image24|    | This icon is used view explain plan in tree format.                                                    |
   +--------------+--------------+--------------------------------------------------------------------------------------------------------+
   | Text Format  | |image25|    | This icon is used view explain plan in text format.                                                    |
   +--------------+--------------+--------------------------------------------------------------------------------------------------------+
   | Copy         | |image26|    | This icon is used to copy selected content from result window to clipboard. Shortcut key - **Ctrl+C**. |
   +--------------+--------------+--------------------------------------------------------------------------------------------------------+
   | Save         | |image27|    | This icon is used to save the explain plan in text format.                                             |
   +--------------+--------------+--------------------------------------------------------------------------------------------------------+

   Refer to :ref:`Execute SQL Queries <en-us_topic_0000001234200573__en-us_topic_0185264856_section16147111413113>` for information refresh, SQL preview, and search bar.

   Refresh operation re-executes the explain/analyze query and refreshes the plan in the existing tab.

   The result is displayed in the **Messages** tab.

.. |image1| image:: /_static/images/en-us_image_0000001188521388.jpg
.. |image2| image:: /_static/images/en-us_image_0000001188521374.jpg
.. |image3| image:: /_static/images/en-us_image_0000001234200909.jpg
.. |image4| image:: /_static/images/en-us_image_0000001233800973.jpg
.. |image5| image:: /_static/images/en-us_image_0000001233800981.jpg
.. |image6| image:: /_static/images/en-us_image_0000001234042411.jpg
.. |image7| image:: /_static/images/en-us_image_0000001188202872.jpg
.. |image8| image:: /_static/images/en-us_image_0000001233800977.jpg
.. |image9| image:: /_static/images/en-us_image_0000001188202874.jpg
.. |image10| image:: /_static/images/en-us_image_0000001233922465.jpg
.. |image11| image:: /_static/images/en-us_image_0000001234200895.jpg
.. |image12| image:: /_static/images/en-us_image_0000001188202858.jpg
.. |image13| image:: /_static/images/en-us_image_0000001233800983.jpg
.. |image14| image:: /_static/images/en-us_image_0000001188362832.jpg
.. |image15| image:: /_static/images/en-us_image_0000001234200905.jpg
.. |image16| image:: /_static/images/en-us_image_0000001188362824.jpg
.. |image17| image:: /_static/images/en-us_image_0000001234200903.jpg
.. |image18| image:: /_static/images/en-us_image_0000001233922473.jpg
.. |image19| image:: /_static/images/en-us_image_0000001188521378.jpg
.. |image20| image:: /_static/images/en-us_image_0000001188362834.jpg
.. |image21| image:: /_static/images/en-us_image_0000001188521386.jpg
.. |image22| image:: /_static/images/en-us_image_0000001188362838.jpg
.. |image23| image:: /_static/images/en-us_image_0000001188362828.jpg
.. |image24| image:: /_static/images/en-us_image_0000001188362834.jpg
.. |image25| image:: /_static/images/en-us_image_0000001188521386.jpg
.. |image26| image:: /_static/images/en-us_image_0000001188362838.jpg
.. |image27| image:: /_static/images/en-us_image_0000001188362828.jpg
