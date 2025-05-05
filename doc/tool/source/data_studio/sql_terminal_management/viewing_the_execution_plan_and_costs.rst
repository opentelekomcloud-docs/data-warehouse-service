:original_name: DWS_DS_033.html

.. _DWS_DS_033:

Viewing the Execution Plan and Costs
====================================

The execution plan shows how the table referenced by the SQL statement will be scanned (sequential scan or index scan).

The SQL statement execution cost indicates the duration for executing a statement (measured in cost units that are arbitrary, but conventionally mean disk page fetches).

Follow the steps below to view the plan and cost for a required SQL query:

#. Enter the query or use an existing query in the **SQL Terminal** and click |image1| on the SQL Terminal toolbar to view explain plan.

   To view explain plan with analyze, click the drop-down from |image2|, select **Include Analyze**, and click |image3|.

   The **Execution Plan** opens in tree view format as a new tab at the bottom by default. The display mode has a tree shape and text style.

   .. note::

      The data shown in tree explain plan and visual explain may vary, since the execution parameters considered by both are not the same.

   Following are the parameters selected for explain plan with/without analyze and the columns displayed:

   .. table:: **Table 1** Explain plan options

      +--------------------------------------------------+------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Explain Plan Type                                | Parameter                                | Column                                                                                                                               |
      +==================================================+==========================================+======================================================================================================================================+
      | **Include Analyze** deselected (default setting) | Verbose, Costs                           | Node type, startup cost, total cost, rows, width, additional Info                                                                    |
      +--------------------------------------------------+------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
      | **Include Analyze** selected                     | Analyze, Verbose, Costs, Buffers, Timing | Node type, startup cost, total cost, rows, width, Actual startup time, Actual total time, Actual Rows, Actual loops, Additional Info |
      +--------------------------------------------------+------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+

   The **Additional Info** column contains predicate information (filter predicate and hash condition), distribution key, output information, and node type.

   In **Tree Format**, nodes are categorized into 16 types. Each node is indicated with an icon of the corresponding type. The following table lists the node types and icons.

   .. table:: **Table 2** Node types and icons

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

   You can hover over highlighted cells to identify the heaviest, costliest, and slowest nodes. Cells can only be highlighted in **Tree Format**.

   If multiple queries are selected, the explain plan with/without **Include Analyze** selected is displayed only for the last query.

   Each execution plan generated from executing a query is opened on a new tab page.

   If the connection is lost during execution but the database remains connected in **Object Browser**, the **Connection Error** dialog box is displayed with the following options:

   -  **OK**: Connects to the database again to obtain the execution plan and cost.
   -  **Cancel**: Disconnects the database from **Object Browser**.

   Toolbar menu in the **Execution Plan** window:

   +--------------+-----------+-----------------------------------------------------------------------------------------------------------------------+
   | Toolbar Name | Icon      | Description                                                                                                           |
   +==============+===========+=======================================================================================================================+
   | Tree Format  | |image24| | This icon is used view explain plan in tree format.                                                                   |
   +--------------+-----------+-----------------------------------------------------------------------------------------------------------------------+
   | Text Format  | |image25| | This icon is used to display explain plan in text format.                                                             |
   +--------------+-----------+-----------------------------------------------------------------------------------------------------------------------+
   | Copy         | |image26| | This icon is used to copy selected data from the **Result** tab to clipboard. The shortcut key is **Ctrl**\ +\ **C**. |
   +--------------+-----------+-----------------------------------------------------------------------------------------------------------------------+
   | Commit       | |image27| | This icon is used to save the explain plan in text format.                                                            |
   +--------------+-----------+-----------------------------------------------------------------------------------------------------------------------+

   For information about refresh, SQL preview, and search bar, see :ref:`Execute SQL Queries <en-us_topic_0000001860318949__en-us_topic_0185264856_section16147111413113>`.

   After you click **Refresh**, the **EXPLAIN ANALYZE** query is executed again, and the execution plan is updated.

   The result is displayed on the **Messages** tab.

.. |image1| image:: /_static/images/en-us_image_0000001813599224.png
.. |image2| image:: /_static/images/en-us_image_0000001813599764.png
.. |image3| image:: /_static/images/en-us_image_0000001813599764.png
.. |image4| image:: /_static/images/en-us_image_0000001813439452.jpg
.. |image5| image:: /_static/images/en-us_image_0000001860319149.jpg
.. |image6| image:: /_static/images/en-us_image_0000001860319173.jpg
.. |image7| image:: /_static/images/en-us_image_0000001813599252.jpg
.. |image8| image:: /_static/images/en-us_image_0000001860199313.jpg
.. |image9| image:: /_static/images/en-us_image_0000001860319137.jpg
.. |image10| image:: /_static/images/en-us_image_0000001813439444.jpg
.. |image11| image:: /_static/images/en-us_image_0000001813599260.jpg
.. |image12| image:: /_static/images/en-us_image_0000001813439472.jpg
.. |image13| image:: /_static/images/en-us_image_0000001813599256.jpg
.. |image14| image:: /_static/images/en-us_image_0000001860319169.jpg
.. |image15| image:: /_static/images/en-us_image_0000001813599244.jpg
.. |image16| image:: /_static/images/en-us_image_0000001860319161.jpg
.. |image17| image:: /_static/images/en-us_image_0000001813599236.jpg
.. |image18| image:: /_static/images/en-us_image_0000001860319141.jpg
.. |image19| image:: /_static/images/en-us_image_0000001813599228.jpg
.. |image20| image:: /_static/images/en-us_image_0000001860319177.png
.. |image21| image:: /_static/images/en-us_image_0000001860319165.jpg
.. |image22| image:: /_static/images/en-us_image_0000001813599248.jpg
.. |image23| image:: /_static/images/en-us_image_0000001860319145.jpg
.. |image24| image:: /_static/images/en-us_image_0000001860319177.png
.. |image25| image:: /_static/images/en-us_image_0000001860319165.jpg
.. |image26| image:: /_static/images/en-us_image_0000001813599248.jpg
.. |image27| image:: /_static/images/en-us_image_0000001860319145.jpg
