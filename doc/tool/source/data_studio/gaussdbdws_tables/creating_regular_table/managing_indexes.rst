:original_name: DWS_DS_75.html

.. _DWS_DS_75:

Managing Indexes
================

You can create indexes in a table to search for data efficiently.

After a table is created, you can add indexes to it. You can perform the following operations only in a common table:

-  :ref:`Creating an Index <en-us_topic_0000001188362546__en-us_topic_0185264835_section1124173174619>`
-  :ref:`Renaming an Index <en-us_topic_0000001188362546__en-us_topic_0185264835_section024218316464>`
-  :ref:`Changing a Fill Factor <en-us_topic_0000001188362546__en-us_topic_0185264835_section2024363194611>`
-  :ref:`Deleting an Index <en-us_topic_0000001188362546__en-us_topic_0185264835_section192431131204620>`

.. _en-us_topic_0000001188362546__en-us_topic_0185264835_section1124173174619:

Creating an Index
-----------------

Perform the following steps to add an index to a table:

#. Right-click **Indexes** and choose **Create Index** from the shortcut menu.

   The **Create Index** dialog box is displayed.

#. Enter the details and click **Create**. You can also view the SQL statement by clicking the **Preview Query** button. Items in **Available Columns** are not sorted. Items moved back from **Index Columns** to **Available Columns** are unsorted, and is not related to the column order in the table. You can set the order of the **Index Columns** using the arrow buttons. Data Studio displays the operation status in the status bar.

.. _en-us_topic_0000001188362546__en-us_topic_0185264835_section024218316464:

Renaming an Index
-----------------

Follow the steps below to rename an index:

#. Right-click the selected index and select **Rename Index**.

   The **Rename Index** dialog box is displayed.

#. Enter a new name and click **OK**. Data Studio displays the operation status in the status bar.

.. _en-us_topic_0000001188362546__en-us_topic_0185264835_section2024363194611:

Changing a Fill Factor
----------------------

To change a fill factor, perform the following steps:

#. Right-click an index and choose **Change Fill Factor** from the shortcut menu.

   The **Change Fill Factor** dialog box is displayed.

#. Select a fill factor and click **OK**. Data Studio displays the operation status in the status bar.

.. _en-us_topic_0000001188362546__en-us_topic_0185264835_section192431131204620:

Deleting an Index
-----------------

Perform the following steps to delete an index:

#. Right-click an index and choose **Drop Index** from the shortcut menu.

   The **Drop Index** dialog box is displayed.

#. In the confirmation dialog box, click **OK**. Data Studio displays the operation status in the status bar. The index will be deleted from the table.

   .. note::

      When the last index of a table is deleted, the value of the **Has Index** parameter may still be **TRUE**. After a vacuum operation is performed on the table, this parameter will change to **FALSE**.
