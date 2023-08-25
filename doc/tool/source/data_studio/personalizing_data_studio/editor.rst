:original_name: DWS_DS_138.html

.. _DWS_DS_138:

Editor
======

This section provides details on how to personalize syntax coloring, SQL history information, templates, and formatter.

.. _en-us_topic_0000001188521052__en-us_topic_0185264581_section6791101652013:

Syntax Coloring
---------------

Follow the steps to customize the SQL highlight color:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor > Syntax Coloring**.

   The **Syntax Coloring** pane is displayed.

#. Click the color button to customize the color for the type of syntax.

   For example, click |image1| to customize the color for **Strings**. The color picker dialog box is displayed.

   Use the color picker to set the required color for a specific syntax category. You can choose basic colors or define custom colors in the color picker.

   .. note::

      Click **Restore Defaults** from **Syntax Coloring** pane to reset to default color scheme.

#. Click **OK**. The **Restart Data Studio** dialog box is displayed.

#. Click **Yes** to restart Data Studio. If any export, import or execution operations are in progress, then Data Studio displays the **Process is running** dialog box\ **.**

#. Click **Force Restart** to discard operations and restart Data Studio. Click **OK** to continue performing operations.

   .. note::

      The *Preferences.prefs* file contains the custom color settings. If the file is corrupted, Data Studio will display the default values.

   The custom color(s) will be set after you restart Data Studio.

.. _en-us_topic_0000001188521052__en-us_topic_0185264581_section1382623812471:

SQL History
-----------

You can customize Data Studio to set the number of SQL history count that can be made available and also the number of characters for the query for each of the query saved in SQL history.

Follow the steps to customize the number of executed queries and number of characters in the query to be saved in SQL History:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor > SQL History**.

   The **SQL History** pane is displayed.

#. Set the number of queries to be saved in **SQL History Count** field.

   .. note::

      Minimum value is 1 and maximum is 1000. The current value set for this preference will be displayed.

#. Set the number of characters to be allowed in each query that is saved in the SQL History in the **SQL Query Characters** field.

   .. note::

      Minimum value is 1 and maximum is 1000. Enter "0" in this field to set no character limit. The current value set for this preference will be displayed.

#. Click **Apply**.

#. Click **OK**.

   .. note::

      -  Click **Restore Defaults** from **SQL History** pane to reset to default value.
      -  The default value for **SQL History Count** is 50 and **SQL Query Characters** it is 1000.
      -  If the input value is less than the original one, data may be lost. In this case, a message is displayed to notify you of the data loss risk and ask you whether to proceed with the operation.
      -  If there are unsaved changes and you navigate away from this pane, then a message displays to state that there are unsaved changes.
      -  Pinned queries are not affected by the changes made to the **SQL History Count** field. **Example:** If the number of pinned queries is 50 and the **SQL History Count** is set to 25, then SQL History will show 50 pinned queries.
      -  The **SQL Query Characters** changes affects only queries added post the configuration change.

.. _en-us_topic_0000001188521052__en-us_topic_0185264581_section116501350276:

Adding Templates
----------------

You can customize Data Studio to create new, edit existing, and remove templates. Refer to the :ref:`Using Templates <en-us_topic_0000001234200573__en-us_topic_0185264856_section2303144411114>` section for detailed information on templates.

.. note::

   Restoring the settings to default removes all user defined templates from the list.

Follow the steps below to create templates:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor > Templates**.

   The **Templates** pane is displayed.

#. Click **New**.

#. Enter a name for the template in the **Name** field.

#. Enter description in the **Description** field.

#. Enter the SQL statement pattern in the **Pattern** field.

   .. note::

      The text entered in **Pattern** field will be syntax highlighted.

#. Click **OK**.

Modifying Templates
-------------------

Follow the steps below to edit templates:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor > Templates**.

   The **Templates** pane is displayed.

#. Click **Edit**.

#. Edit the name in the **Name** field, if required.

#. Edit the description in the **Description** field, if required.

#. Edit the SQL statement pattern in the **Pattern** field, if required.

   .. note::

      The text entered in **Pattern** field will be syntax highlighted.

#. Click **OK**.

Removing Templates
------------------

Follow the steps below to remove templates:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor > Templates**.

   The **Templates** pane is displayed.

#. Select the template to be removed, and click **Remove**.

   The template is removed from the **Templates** list.

   .. note::

      Default templates that are removed can be added back using **Restore Removed** option. It will restore the template to the last updated change. **Restore Removed** option is not applicable to user defined templates.

Reverting to Default Templates
------------------------------

Follow the steps below to revert to default templates:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor > Templates**.

   The **Templates** pane is displayed.

#. Select at least one default template that is modified to revert to default template settings.

#. Click **Revert to Default**.

.. _en-us_topic_0000001188521052__en-us_topic_0185264581_section64411944205615:

Formatter
---------

You can customize Data Studio to set the tab width and convert tab to spaces while performing indent and unindent operation. Refer to :ref:`Indent/Un-indent Lines <en-us_topic_0000001234200573__en-us_topic_0185264856_section2611012152811>` section to perform indent/unindent operation and replace tab with spaces.

Follow the steps to customize the indent size and convert tab to spaces:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor > Formatter**.

   The **Formatter** pane is displayed.

#. Select the **Insert Space** option to replace tab with spaces or **Insert Tab** to add/remove tabs while indenting/unindenting lines.

#. Enter the indent size in **Indent Size**. Based on the number specified in this field, the indent/unindent/space length is defined.

.. _en-us_topic_0000001188521052__en-us_topic_0185264581_section210212814444:

Transaction
-----------

Follow the steps to edit Transaction settings:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor > Transaction**

   The **Transaction** pane is displayed.

#. In the **Auto Commit** window, you can perform the following operations:

   -  Select **Enable** to switch on the auto commit feature. In this case commit and rollback button will be disabled. Transaction will be committed automatically.

   |image2|

   -  Select **Disable** to switch off the auto commit feature. Commit and Rollback button can be used manually for commiting or reverting changes.

   |image3|

   .. note::

      Default behavior for Auto-Commit is **ON**.

Folding
-------

Follow the steps for Folding:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor > Folding**.

   The Folding pane is displayed.

#. Select **Enable** or **Disable**. By default, **Enable** is selected.

   -  **Enable:** This indicates enable SQL folding feature. Supported SQL statements can be folded or unfolded.
   -  **Disable:** This indicates disable SQL folding feature.

      .. note::

         Modification in settings reflects in newly opened editor. The editor which is already opened will remain with previous settings until restart.

Font
----

Follow the steps to set Font:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor > Font**.

   The Font pane is displayed.

#. Provide required font size within range from 1 to 50. By default, font size is 10.

Auto Suggest
------------

Follow the steps for Auto Suggest:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor > Auto Suggest**.

   The **Auto Suggest** pane is displayed.

#. In **Auto Suggest** pane, provide required number of character in **Auto Suggest Min Character**. Default value is 2. Range of number of Auto Suggest minimum characters are within 2 to 10.

   For auto suggest, sorting can be as follows:

   a. Keywords
   b. Data types
   c. Loaded Database Objects

      .. note::

         -  Each group should be in sorted order.
         -  Databases are classified by keyword and data type.
         -  If database is not connected, then default keywords must be displayed.
         -  When you press dot (.) then only respective database objects should be displayed. Keywords/Data types should not be displayed.
         -  Auto suggest should be triggered by shortcuts.

.. |image1| image:: /_static/images/en-us_image_0000001234200697.jpg
.. |image2| image:: /_static/images/en-us_image_0000001188362622.png
.. |image3| image:: /_static/images/en-us_image_0000001188521172.png
