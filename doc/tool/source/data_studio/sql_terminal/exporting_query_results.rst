:original_name: DWS_DS_129.html

.. _DWS_DS_129:

Exporting Query Results
=======================

You can export the results of an SQL query into a CSV, Text or Binary file.

This section contains the following topics:

-  :ref:`Exporting all data <en-us_topic_0000001188680978__en-us_topic_0185264341_section449445904115>`
-  :ref:`Exporting Data On the Current Page <en-us_topic_0000001188680978__en-us_topic_0185264341_section1056892694218>`

.. _en-us_topic_0000001188680978__en-us_topic_0185264341_section449445904115:

Exporting all data
------------------

The following functions are disabled while the export operation is in progress:

-  Executing SQL queries in the **SQL Terminal**

-  Executing PL/SQL statements
-  Debugging PL/SQL statements

Follow the steps below to export all results:

#. Select the **Result** tab.

#. Click |image1|.

   The **Export ResultSet Data** window is displayed.

   Refer to :ref:`Exporting Table Data <dws_ds_98>` to complete the export operation.

   .. note::

      You can check the status bar to view the status of the result being exported.

   The **Data Exported Successfully** dialog box is displayed.

#. Click **OK**. Data Studio displays the status of the operation in the **Messages** tab.

   .. note::

      If the disk is full while exporting the results, then Data Studio displays an error in the Messages tab. In this case, clear the disk, re-establish the connection and export the result data.

The Messages tab shows the **Execution Time**, **Total Result Records Fetched**, and the path where the file is saved.

.. _en-us_topic_0000001188680978__en-us_topic_0185264341_section1056892694218:

Exporting Data On the Current Page
----------------------------------

It is recommended to export all results instead of exporting the current page. The **Export Current Page to CSV** function has been deleted.

Follow the steps below to export the current page:

#. Select the **Result** tab.

#. Click the |image2| icon to export the current page.

   The **Data Studio Security Disclaimer** dialog box is displayed.

#. Click **OK**.

#. Select the location to save the current page.

   .. note::

      You can check the status bar to view the status of the page being exported.

#. Click **Save**. The **Data Exported Successfully** dialog box is displayed.

#. Click **OK**. Data Studio displays the status of the operation in the **Messages** tab.

   .. note::

      If the disk is full while exporting the results, then Data Studio displays an error in the Messages tab. In this case, clear the disk, re-establish the connection and export the result data.

.. |image1| image:: /_static/images/en-us_image_0000001188202682.jpg
.. |image2| image:: /_static/images/en-us_image_0000001233922285.jpg
