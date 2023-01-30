:original_name: DWS_DS_138.html

.. _DWS_DS_138:

Editor
======

This section describes how to customize syntax highlighting, SQL history information, templates, and formatters.

.. _en-us_topic_0000001145913107__en-us_topic_0185264581_section6791101652013:

Syntax Highlighting
-------------------

Perform the following steps to customize SQL highlighting:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor** > **Syntax Coloring**.

   The **Syntax Coloring** pane is displayed.

#. Click the color button to customize the color for a syntax type.

   For example, click |image1| to customize the color for **Strings**. A dialog box is displayed prompting you to select a color.

   Select a color for a specific syntax type. You can select one of the basic colors or customize a color.

   .. note::

      Click **Restore Defaults** in the **Syntax Coloring** pane to restore the default color.

#. Click **OK**. The **Restart Data Studio** dialog box is displayed.

#. Click **Yes** to restart Data Studio. If any export, import or execution operations are in progress, the **Process is running** dialog box is displayed.

#. Click **OK** to wait till the operations are complete or click **Force Restart** to cancel the operations.

   .. note::

      The **Preferences.prefs** file contains the custom color settings. If the file is damaged, Data Studio will display the default settings.

   The customized color will be used after you restart Data Studio.

.. _en-us_topic_0000001145913107__en-us_topic_0185264581_section1382623812471:

SQL History
-----------

You can set the value of **SQL History Count** and also the number of characters saved for each query in **SQL History**.

Perform the following steps to set the value of **SQL History Count** and also the number of characters saved for each query in **SQL History**:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor** > **SQL History**.

   The **SQL History** pane is displayed.

#. Set the number of queries to be saved in the **SQL History Count** field.

   .. note::

      The value ranges from 1 to 1000. The current value of this field will be displayed.

#. Set the value of **SQL Query Characters** to the maximum number of characters allowed in each query that is saved in **SQL History**.

   .. note::

      The value ranges from 1 to 1000. You can enter **0** to remove the character limit. The current value of this field will be displayed.

#. Click **Apply**.

#. Click **OK**.

   .. note::

      -  Click **Restore Defaults** in the **Syntax Coloring** pane to restore the default value.
      -  **SQL History Count** defaults to 50 and **SQL Query Characters** defaults to 1000.
      -  If the input value is less than the original one, data may be lost. In this case, a message is displayed to notify you of the data loss risk and ask you whether to proceed.
      -  If you move away from this pane without saving the changes, a message is displayed to notify you of the unsaved changes.
      -  The number of pinned queries is not affected by the changed value of the **SQL History Count** field. For example, if the number of pinned queries is 50 and **SQL History Count** is set to 25, 50 pinned queries will be displayed in **SQL History**.
      -  If the value of **SQL Query Characters** is changed, the new value applies only to queries added after the change.

.. _en-us_topic_0000001145913107__en-us_topic_0185264581_section116501350276:

Adding a Template
-----------------

Data Studio allows you to create, edit, and remove a template. For details about templates, see :ref:`Using Templates <en-us_topic_0000001145833051__en-us_topic_0185264856_section2303144411114>`.

.. note::

   If the default settings are restored, all user-defined templates will be removed from the list.

Perform the following steps to create a template:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor** > **Templates**.

   The **Templates** pane is displayed.

#. Click **New**.

#. Enter a template name in the **Name** field.

#. Enter description in the **Description** field.

#. Enter a SQL statement pattern in the **Pattern** field.

   .. note::

      The syntax of the text entered in **Pattern** will be highlighted.

#. Click **OK**.

Editing a Template
------------------

Perform the following steps to edit a template:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor** > **Templates**.

   The **Templates** pane is displayed.

#. Click **Edit**.

#. Edit the name in the **Name** field as required.

#. Edit the description in the **Description** field as required.

#. Edit the SQL statement pattern in the **Pattern** field as required.

   .. note::

      The syntax of the text entered in **Pattern** will be highlighted.

#. Click **OK**.

Removing a Template
-------------------

Perform the following steps to remove a template:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor** > **Templates**.

   The **Templates** pane is displayed.

#. Select the template to be removed, and click **Remove**.

   The template is removed from the **Templates** pane.

   .. note::

      Default templates that are removed can be added back using the **Restore Removed** option. It will restore the template to the last updated version. However, the **Restore Removed** option is not applicable to user-defined templates.

Restoring the Default Template Settings
---------------------------------------

Perform the following steps to restore the default template settings:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor** > **Templates**.

   The **Templates** pane is displayed.

#. Select at least one default template that has been modified and restore the default template settings.

#. Click **Revert to Default**.

.. _en-us_topic_0000001145913107__en-us_topic_0185264581_section64411944205615:

Formatter
---------

Data Studio allows you to set the tab width and convert tabs to spaces during indent and unindent operations. For details, see :ref:`Indenting or Un-indenting Lines <en-us_topic_0000001145833051__en-us_topic_0185264856_section2611012152811>`.

Perform the following steps to customize the indent size and convert tabs to spaces:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor** > **Formatter**.

   The **Formatter** pane is displayed.

#. Select **Insert Space** to convert tabs to spaces, or **Insert Tab** to add or remove tabs when indenting or unindenting lines.

#. Enter the indent size in **Indent Size** to define the indent/unindent/space length.

.. _en-us_topic_0000001145913107__en-us_topic_0185264581_section210212814444:

Transaction
-----------

Perform the following steps to edit settings in **Transaction**:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor** > **Transaction**.

   The **Transaction** pane is displayed.

#. In the **Auto Commit** window, you can perform the following operations:

   -  Select **Enable** to enable the Auto Commit feature. In this case, transactions are automatically committed and cannot be manually committed or rolled back.

   |image2|

   -  Select **Disable** to disable the Auto Commit feature. In this case, transactions can be manually committed or rolled back.

   |image3|

   .. note::

      **Auto Commit** defaults to **Enable**.

Folding a SQL Statement
-----------------------

Perform the following steps to fold a SQL statement:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor** > **Folding**.

   The **Folding** pane is displayed.

#. Select **Enable** or **Disable**. By default, **Enable** is selected.

   -  **Enable**: indicates that the SQL folding feature is enabled Supported SQL statements can be folded or unfolded.
   -  **Disable**: indicates that the SQL folding feature is disabled

      .. note::

         Any change in the **Folding** parameter takes effect only in new editors, and will not take effect in opened editors until they are restarted.

Font
----

Perform the following steps to configure **Font**:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor** > **Font**.

   The **Font** pane is displayed.

#. Configure the font size, which ranges from 1 to 50 and defaults to 10.

Auto Suggest
------------

Perform the following steps to configure **Auto Suggest**:

#. Choose **Settings** > **Preferences** from the main menu.

   The **Preferences** dialog box is displayed.

#. Choose **Editor** > **Auto Suggest**.

   The **Auto Suggest** pane is displayed.

#. In the **Auto Suggest** pane, configure **Auto Suggest Min Character**. The value ranges from 2 to 10 and defaults to 2.

   To enable the Auto Suggest feature, sort the following groups:

   a. Keywords
   b. Data types
   c. Loaded database objects

      .. note::

         -  Each group must be sorted.
         -  Databases are classified by keyword and data type.
         -  If database is not connected, default keywords, that is, database type, must be displayed.
         -  When you enter a period (.), only related database objects are displayed. Keywords and data types are not displayed.
         -  The Auto Suggest feature can be enabled using shortcut keys.

.. |image1| image:: /_static/images/en-us_image_0000001098993230.jpg
.. |image2| image:: /_static/images/en-us_image_0000001145513229.png
.. |image3| image:: /_static/images/en-us_image_0000001145833083.png
