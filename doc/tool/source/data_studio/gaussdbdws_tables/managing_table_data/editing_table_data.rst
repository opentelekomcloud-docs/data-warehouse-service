:original_name: DWS_DS_102.html

.. _DWS_DS_102:

Editing Table Data
==================

Follow the steps below to edit table data:

#. Right-click the selected table and select **Edit Table Data**.

   The **Edit Table** data tab is displayed.

   Refer to :ref:`Viewing Table Data <dws_ds_101>` for description on copy and search toolbar options.

Data Studio validates only the following data types entered into cells:

Bigint, bit, boolean, char, date, decimal, double, float, integer, numeric, real, smallint, time, time with time zone, time stamp, time stamp with time zone, tinyint, and varchar. Editing of array type data type is not supported.

Any related errors during this operation reported by database will be displayed in Data Studio. Time with time zone and timestamp with time zone columns are non-editable columns.

You can perform the following operations in the **Edit Table Data** tab:

-  :ref:`Insert <en-us_topic_0000001233800701__en-us_topic_0185264734_section5333590620118>`
-  :ref:`Delete <en-us_topic_0000001233800701__en-us_topic_0185264734_section2976164920345>`
-  :ref:`Update <en-us_topic_0000001233800701__en-us_topic_0185264734_updateacell>`
-  :ref:`Copy <en-us_topic_0000001233800701__en-us_topic_0185264734_section2471220104119>`
-  :ref:`Paste <en-us_topic_0000001233800701__en-us_topic_0185264734_section3927320420>`

.. _en-us_topic_0000001233800701__en-us_topic_0185264734_section5333590620118:

Insert
------

Follow the steps to insert a row:

#. Click |image1| to insert a row.

#. Double-click the cell to modify and enter the required details in the row.

#. Click |image2| to save changes.

   The **Edit Table Data** tab status bar shows the **Query Submit Time**, **Number of Rows Fetched**, **Execution Time** and **Status** of the operation.

   .. important::

      Data Studio updates rows identified by the unique key. If a unique key is not identified for a table and there are identical rows, then an update operation made on one of the rows will affect all identical rows. Refresh the **Edit Table Data** tab to view the updated rows.

   .. note::

      -  Changes to cells in a row that are not saved are highlighted in green. Once saved the color resets to default color.
      -  Unsaved records are highlighted in red. The number of successful and failed records are displayed in the status bar of the :ref:`Edit Table Data <dws_ds_102>` tab.
      -  Clicking **Save** either saves all the valid changes or does not save anything if there are invalid changes. Refer to :ref:`Editing Table Data <dws_ds_102>` to set the behavior of save operation.

#. Click |image3| to roll back the changes that are not saved.

#. Set the preference to define:

   -  Number of records to be fetched

   -  Column width

   -  Copy option from result set

      For details, see :ref:`Query Results <en-us_topic_0000001188202558__en-us_topic_0185264923_section28611419201210>`.

Data Studio allows you to edit the distribution key column only for a new row.

.. _en-us_topic_0000001233800701__en-us_topic_0185264734_section2976164920345:

Delete
------

Follow the steps to delete a row:

#. Click the row header of the row to be deleted.
#. Click |image4| to delete a row.
#. Click |image5| to save changes. The **Define Unique Key** dialog box is displayed.
#. Click the required option:

   -  **Use All Columns**

      Click **Use All Columns** to define all columns as unique key.

   -  **Custom Unique Key**

      a. Click **Custom Unique Key** to define selected columns as unique key.
      b. The **Define Unique Key** dialogue box is displayed.
      c. Select the required columns and click **OK**.

   -  Cancel

      Click **Cancel** to modify the information in **Edit Table Data** tab.

      The **Edit Table Data** tab status bar shows the **Query Submit Time**, **Number of Rows Fetched**, **Execution Time** and **Status** of the operation.

      Select **Remember the selection for this window** option to hide the unique definition window from displaying while continuing with the edit table data operation. Click |image6| from Edit Table Data toolbar to clear previously selected unique key definition and display unique definition wind

   .. note::

      -  Changes to cells in a row that are not saved are highlighted in green. Once saved the color resets to default color.
      -  Unsaved records are highlighted in red. The number of successful and failed records are displayed in the status bar of the :ref:`Edit Table Data <dws_ds_102>` tab.
      -  Clicking **Save** either saves all the valid changes or does not save anything if there are invalid changes. For details, see :ref:`Editing Table Data <dws_ds_102>`.

#. Click |image7| to roll back the changes that are not saved.
#. Refresh the table data to view deleted duplicate rows.

.. _en-us_topic_0000001233800701__en-us_topic_0185264734_updateacell:

Update
------

Follow the steps to update cell data:

#. Double-click the cell to update the contents of the cell.

#. Click |image8| to save changes.

   The **Define Unique Key** dialog box is displayed.

#. Click the required option:

   -  **Use All Columns**

      Click **Use All Columns** to define all columns as unique key.

   -  **Custom Unique Key**

      a. Click **Custom Unique Key** to define selected columns as unique key.
      b. The **Define Unique Key** dialogue box is displayed.
      c. Select the required columns and click **OK**.

   -  **Cancel**

      Click **Cancel** to modify the information in **Edit Table Data** tab.

      The status bar shows the **Execution Time** and **Status** of the operation.

      Select **Remember the selection for this window** option to hide the unique definition window from displaying while continuing with the edit table data operation. Click |image9| from Edit Table Data toolbar to clear previously selected unique key definition and display unique definition wind

      .. note::

         -  Changes to cells in a row that are not saved are highlighted in green. Once saved the color resets to default color.
         -  Unsaved records are highlighted in red. The number of successful and failed records are displayed in the status bar of the **Edit Table Data** tab.
         -  Clicking **Save** either saves all the valid changes or does not save anything if there are invalid changes. For details, see :ref:`Editing Table Data <dws_ds_102>`.

#. Click |image10| to roll back the changes that are not saved.

#. Refresh the table data to view deleted duplicate rows.

During the edit operation, Data Studio does not allow you to edit the distribution key column as it is used by the DB to locate data in the database cluster.

.. _en-us_topic_0000001233800701__en-us_topic_0185264734_section2471220104119:

Copy
----

You can copy data from the **Edit Table Data** tab.

Follow the steps to copy data:

#. Select the cell(s) and click |image11| (Copy) or |image12| (Advanced Copy).

   For more information about the differences between **Copy** and **Advanced Copy**, see :ref:`Execute SQL Queries <en-us_topic_0000001234200573__en-us_topic_0185264856_section16147111413113>`.

   .. note::

      -  Data can be copied to include the row number and/or column header. Refer to :ref:`Query Results <en-us_topic_0000001188202558__en-us_topic_0185264923_section28611419201210>` to set this preference.
      -  Select part of cell content and press **Ctrl+C** or click |image13| to copy selected text from a cell.

.. _en-us_topic_0000001233800701__en-us_topic_0185264734_section3927320420:

Paste
-----

You can copy data from a CSV file and paste it into cells in the **Edit Table Data** tab to insert and update records. If you paste onto existing cell data, the data is overwritten with the new data from the CSV file. Follow the steps to paste data into a cell:

#. Copy data from the CSV file.
#. Select the cell(s) and click |image14|.
#. Click |image15| to save changes. The **Define Unique Key** dialog box is displayed.
#. Click the required option:

   -  **Use All Columns**

      Click **Use All Columns** to define all columns as unique key.

   -  **Custom Unique Key**

      a. Click **Custom Unique Key** to define selected columns as unique key.
      b. The **Define Unique Key** dialogue box is displayed.
      c. Select the required columns and click **OK**.

   -  **Cancel**

      Click **Cancel** to modify the information in **Edit Table Data** tab.

      The status bar shows the **Execution Time** and **Status** of the operation.

      Select **Remember the selection for this window** option to hide the unique definition window from displaying while continuing with the edit table data operation. Click |image16| from Edit Table Data toolbar to clear previously selected unique key definition and display unique definition wind

      .. note::

         -  The number of copied cells from CSV must match the number of cells selected in the **Edit Table Data** tab to paste the data.

         -  Click |image17| to roll back the changes that are not saved.
         -  Changes to cells in a row that are not saved are highlighted in green. Once saved the color resets to default color.
         -  Unsaved records are highlighted in red. The number of successful and failed records are displayed in the status bar of the **Edit Table Data** tab.
         -  Clicking **Save** either saves all the valid changes or does not save anything if there are invalid changes. For details, see :ref:`Editing Table Data <dws_ds_102>`.

During the pasting operation, Data Studio does not allow you to edit the distribution key column as it is used by the DB to locate data in the database cluster.

.. note::

   Empty cells are shown as [NULL]. Empty cell in **Edit Table Data** tab can be searched using the **Null Values** search drop-down.

Refer to :ref:`Execute SQL Queries <en-us_topic_0000001234200573__en-us_topic_0185264856_section16147111413113>` for information on show/hide search bar, sort, column reorder, and encoding options..

.. |image1| image:: /_static/images/en-us_image_0000001234042271.png
.. |image2| image:: /_static/images/en-us_image_0000001234042273.png
.. |image3| image:: /_static/images/en-us_image_0000001234042269.png
.. |image4| image:: /_static/images/en-us_image_0000001188362688.png
.. |image5| image:: /_static/images/en-us_image_0000001234042277.png
.. |image6| image:: /_static/images/en-us_image_0000001188521240.png
.. |image7| image:: /_static/images/en-us_image_0000001233922323.png
.. |image8| image:: /_static/images/en-us_image_0000001234200763.png
.. |image9| image:: /_static/images/en-us_image_0000001233922321.png
.. |image10| image:: /_static/images/en-us_image_0000001188202726.png
.. |image11| image:: /_static/images/en-us_image_0000001234200767.png
.. |image12| image:: /_static/images/en-us_image_0000001188521242.jpg
.. |image13| image:: /_static/images/en-us_image_0000001234042275.jpg
.. |image14| image:: /_static/images/en-us_image_0000001233800831.png
.. |image15| image:: /_static/images/en-us_image_0000001188681160.png
.. |image16| image:: /_static/images/en-us_image_0000001290392476.png
.. |image17| image:: /_static/images/en-us_image_0000001234200765.png
