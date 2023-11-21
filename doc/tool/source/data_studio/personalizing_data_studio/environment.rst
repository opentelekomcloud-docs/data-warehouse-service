:original_name: DWS_DS_139.html

.. _DWS_DS_139:

Environment
===========

.. _en-us_topic_0000001234200631__en-us_topic_0185264709_section212012207470:

Session Setting
---------------

Follow the steps to set Data Studio and file encoding:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment > Session Setting**.

   The **Session Setting** pane is displayed.

#. Select the required Data Studio encoding from **Data Studio Encoding** drop-down.

#. .. _en-us_topic_0000001234200631__en-us_topic_0185264709_li1839502511211:

   Select the file encoding from **File Encoding** field.

   .. note::

      Data Studio supports only UTF-8 and GBK file encoding types.

#. Click **OK**. The **Restart Data Studio** dialog box is displayed.

#. Click **Yes** to restart Data Studio. If any export, import or execution operations are in progress, then Data Studio displays the **Process is running** dialog box.

#. Click **Force Restart** to discard operations and restart Data Studio. Click **OK** to continue performing operations.

   .. note::

      Click **Restore Defaults** from **Session Setting** pane to reset to default values. The default value for Data Studio Encoding and File Encoding is **UTF-8**.

SQL Assistant
-------------

Follow the steps to enable/disable SQL Assistant tool:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment > Session Setting**.

   The **Session Setting** pane is displayed.

#. Select **Enable/Disable** from SQL Assistant section.

#. Click **OK**.

   .. note::

      Click **Restore Defaults** from **Session Setting** pane to reset to default value. The default value for SQL Assistant is **Enable**.

.. _en-us_topic_0000001234200631__en-us_topic_0185264709_section1980415371926:

Query/Function/Procedure Backup
-------------------------------

Refer to the :ref:`Backuping Unsaved Queries/Functions/Procedures <en-us_topic_0000001234200573__en-us_topic_0185264856_section7637071471>` section for information on backup feature provided by Data Studio.

Follow the steps to enable/disable backup of unsaved data in SQL Terminal/PL/SQL Viewer:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment > Session Setting**.

   The **Session Setting** pane is displayed.

#. Select/unselect **Auto Save** from **Auto Save** section.

#. Set the time interval to backup the data in **Interval** field.

#. Click **OK**.

   .. note::

      Click **Restore Defaults** from **Session Setting** pane to reset to default value. Backup of data will be enabled by default with 5 minutes as the default time interval.

Follow the steps to enable/disable data encryption of saved data:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment > Session Setting**.

   The **Session Setting** pane is displayed.

#. Select/unselect **Encryption** from Auto Save section.

#. Click **OK**.

   .. note::

      Click **Restore Defaults** from **Session Setting** pane to reset to default value. Encryption will be enabled by default.

Follow the steps to set the size of **Import Table Data Limit**/**Import File Data Limit**:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment > Session Setting**.

   The **Session Setting** pane is displayed.

   In File Limit section **Import Table Data Limit** and **Import File Data Limit** parameters are displayed.

   |image1|

   **Import Table Data Limit** value defines the maximum size of the table data to be imported.

   **Import File Data Limit** value defines the maximum size of the file to be imported.

#. Click **OK**.

   .. note::

      Mentioned values in the above screenshot are the default values.

Follow the steps to perform rendering:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment > Session Setting**.

   The **Session Setting** pane is displayed.

   In Lazy Rendering section, **Number of objects in a batch** parameter is displayed.

   |image2|

#. Provide required number of objects in a batch, want to be rendered. Range is from 100 to 1000. Default value is **200**.

   If you provide any value which is less than 100 or more than 1000, then **Invalid Range, (100 -1000)** error message is displayed.

#. Click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000001234200761.png
.. |image2| image:: /_static/images/en-us_image_0000001233800833.png
