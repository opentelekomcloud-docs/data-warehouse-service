:original_name: DWS_DS_101.html

.. _DWS_DS_101:

Viewing Table Data
==================

Follow the steps to view table data:

#. Right-click the selected table and select **View Table Data**.

   The **View Table Data** tab is displayed where you can view the table data information.

   Toolbar menu in the **View Table Data** window:

   +----------------------+--------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Toolbar Name         | Toolbar Icon | Description                                                                                                                                                                                                                                                                                                   |
   +======================+==============+===============================================================================================================================================================================================================================================================================================================+
   | Copy                 | |image4|     | Click the icon to copy selected content from **View Table Data** window to clipboard. Shortcut key - **Ctrl+C**.                                                                                                                                                                                              |
   +----------------------+--------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Advanced Copy        | |image5|     | Click the icon to copy content from result window to the clipboard. Results can be copied to include the row number and/or column header. Refer to :ref:`Query Results <en-us_topic_0000001145713115__en-us_topic_0185264923_section28611419201210>` to set this preference. Shortcut key - **Ctrl+Shift+C**. |
   +----------------------+--------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Show/Hide Search bar | |image6|     | Click the icon to display/hide the search text field. This is a toggle button.                                                                                                                                                                                                                                |
   +----------------------+--------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Encoding             | ``-``        | Refer to :ref:`Executing SQL Queries <en-us_topic_0000001145833051__en-us_topic_0185264856_section16147111413113>` for information on encoding selection.                                                                                                                                                     |
   +----------------------+--------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   Icons in Search field:

   +-------------------+-----------+--------------------------------------------------------------------------------------------------------------------+
   | Icon Name         | Icon      | Description                                                                                                        |
   +===================+===========+====================================================================================================================+
   | Search            | |image9|  | Click the icon to search the table data displayed based on the criteria defined. Search text are case insensitive. |
   +-------------------+-----------+--------------------------------------------------------------------------------------------------------------------+
   | Clear Search Text | |image10| | Click the icon to clear the search texts entered in the search field.                                              |
   +-------------------+-----------+--------------------------------------------------------------------------------------------------------------------+

   Refer to :ref:`Executing SQL Queries <en-us_topic_0000001145833051__en-us_topic_0185264856_section16147111413113>` for column reordering and sort option.

   -  **Query Submit Time** - Provides the query submitted time.
   -  Number of rows fetched with execution time is displayed. The default number of rows is displayed. If there are additional rows to be fetched, then it will be denoted with the word "more". You can scroll to the bottom of the table to fetch and display all rows.

      .. important::

         -  When viewing table data, Data Studio automatically adjusts the column widths for an optimal table view. Users can resize the columns as needed. If the text content of a cell exceeds the total available display area, then resizing the cell column may cause DS to become unresponsive.
         -  When the data in a table cell is more than 1000 characters, it will appear trimmed up to 1000 characters with "..." at the end.

            -  If the user copies the data from a cell in a table or Result tab and pastes it on any editor (such as SQL terminal/PLSQL source editor, notepad or any other external editor application), the entire data is pasted.
            -  If the user copies the data from a cell in a table or Result tab and pastes it on an editable cell (same or different), the cell shows only the first 1000 characters with "..." in the end.
            -  When the table/Result tab data is exported, the exported file contains the whole data.

   .. note::

      -  Individual table data window is displayed for each table.
      -  If the data of the table that is already opened is modified, then refresh and open the table data again to view the updated information on the same opened window.
      -  While the data is loading a message displays at the bottom stating "fetching".
      -  If the content of the column has spaces between the words, then word wrap is applied to fit the column within the display area. Word wrap is not applied if the content does not have any spaces between the words.
      -  Select part of cell content and press **Ctrl+C** or click |image11| to copy selected text from a cell.
      -  The size of the column is determined by the maximum content length column.
      -  You can save preference to define:

         -  Number of records to be fetched.

         -  Column width

         -  Copy option from result set.

            Refer to :ref:`Query Results <en-us_topic_0000001145713115__en-us_topic_0185264923_section28611419201210>` for more information.

.. |image1| image:: /_static/images/en-us_image_0000001099153416.jpg
.. |image2| image:: /_static/images/en-us_image_0000001145513439.jpg
.. |image3| image:: /_static/images/en-us_image_0000001098993432.png
.. |image4| image:: /_static/images/en-us_image_0000001099153416.jpg
.. |image5| image:: /_static/images/en-us_image_0000001145513439.jpg
.. |image6| image:: /_static/images/en-us_image_0000001098993432.png
.. |image7| image:: /_static/images/en-us_image_0000001145513437.png
.. |image8| image:: /_static/images/en-us_image_0000001145833289.png
.. |image9| image:: /_static/images/en-us_image_0000001145513437.png
.. |image10| image:: /_static/images/en-us_image_0000001145833289.png
.. |image11| image:: /_static/images/en-us_image_0000001098833436.jpg
