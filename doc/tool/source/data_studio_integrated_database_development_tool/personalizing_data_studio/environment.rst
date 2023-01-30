:original_name: DWS_DS_139.html

.. _DWS_DS_139:

Environment
===========

.. _en-us_topic_0000001099153186__en-us_topic_0185264709_section212012207470:

Session Setting
---------------

Perform the following steps to configure Data Studio encoding and file encoding:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment** > **Session Setting**.

   The **Session Setting** pane is displayed.

#. Select the Data Studio encoding from the **Data Studio Encoding** drop-down list.

#. .. _en-us_topic_0000001099153186__en-us_topic_0185264709_li1839502511211:

   Select the file encoding from the **File Encoding** drop-down list.

   .. note::

      Data Studio supports only UTF-8 and GBK file encoding types.

#. Click **OK**. The **Restart Data Studio** dialog box is displayed.

#. Click **Yes** to restart Data Studio. If any export, import or execution operations are in progress, the **Process is running** dialog box is displayed.

#. Click **OK** to proceed or click **Force Restart** to cancel the operations and restart Data Studio.

   .. note::

      Click **Restore Defaults** in **Session Setting** to restore the default value. The default value for **Data Studio Encoding** and **File Encoding** is **UTF-8**.

SQL Assistant
-------------

Perform the following steps to enable or disable **SQL Assistant**:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment** > **Session Setting**.

   The **Session Setting** pane is displayed.

#. Select **Enable/Disable** in **SQL Assistant**.

#. Click **OK**.

   .. note::

      Click **Restore Defaults** in **Session Setting** to restore the default value. The default value for **SQL Assistant** is **Enable**.

.. _en-us_topic_0000001099153186__en-us_topic_0185264709_section1980415371926:

Query/Function/Procedure Backup
-------------------------------

For details about the backup features of Data Studio, see :ref:`Backing up Unsaved Queries/Functions/Procedures <en-us_topic_0000001145833051__en-us_topic_0185264856_section7637071471>`.

Perform the following steps to enable or disable the backup of unsaved data in **SQL Terminal** and **PL/SQL Viewer**:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment** > **Session Setting**.

   The **Session Setting** pane is displayed.

#. Select or deselect **Auto Save** in the **Auto Save** area.

#. Set the time interval for data backup in the **Interval** field.

#. Click **OK**.

   .. note::

      Click **Restore Defaults** in **Session Setting** to restore the default value. By default, data backup is enabled and **Interval** defaults to 5 minutes.

Perform the following steps to enable or disable the encryption of saved data:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment** > **Session Setting**.

   The **Session Setting** pane is displayed.

#. Select or deselect **Encryption** in the **Auto Save** area.

#. Click **OK**.

   .. note::

      Click **Restore Defaults** in **Session Setting** to restore the default value. Encryption is enabled by default.

Perform the following steps to configure the **Import Table Data Limit** and **Import File Data Limit** parameters:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment** > **Session Setting**.

   The **Session Setting** pane is displayed.

   In the **File Limit** area, configure the **Import Table Data Limit** and **Import File Data Limit** parameters.

   |image1|

   **Import Table Data Limit**: specifies the maximum size of the table data to import

   **Import File Data Limit**: specifies the maximum size of the file to import

#. Click **OK**.

   .. note::

      Values in the preceding figure are default values.

Perform the following steps for rendering:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Environment** > **Session Setting**.

   The **Session Setting** pane is displayed.

   In the **Lazy Rendering** area, the **Number of objects in a batch** parameter is displayed.

   |image2|

#. In the **Lazy Rendering** area, configure the **Number of objects in a batch** parameter, which ranges from 100 to 1000 and defaults to 200.

   If the value input is less than 100 or more than 1000, the **Invalid Range, (100 -1000)** error message is displayed.

#. Click **OK**.

.. |image1| image:: /_static/images/en-us_image_0000001099153262.png
.. |image2| image:: /_static/images/en-us_image_0000001145713199.png
