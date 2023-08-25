:original_name: DWS_DS_121.html

.. _DWS_DS_121:

Opening and Saving SQL Scripts
==============================

Opening an SQL Script
---------------------

Follow the steps to open an SQL script:

#. Choose **File > Open** from the main menu. Alternatively, click **Open** on the toolbar or right-click the **SQL Terminal** and select **Open**.

   If the SQL Terminal has existing content, then there will be an option to overwrite the existing content or append content to it.

#. The **Open** dialog box is displayed.

#. In **Open** dialog box, select the SQL file to import and click **Open**.

   The selected SQL script is opened as a **File Terminal.**

   The icons on the file terminal tab is different from those in the SQL terminal. When you move the mouse cursor over the source file, corresponding database connection will be displayed on File Terminal.

.. note::

   -  The encoding type of the SQL file must match the encoding type specified in :ref:`preferences <en-us_topic_0000001188202558__en-us_topic_0185264923_li1083312422094>`.
   -  Label of the file terminal will start with asterisk(*) if any of its content is edited. Dirty flag is removed once the file terminal is saved.
   -  File Terminals cannot be renamed. One terminal is always mapped to one Source Script File, but one script can be opened in multiple terminals.
   -  You can open SQL scripts only on SQL Terminals.

Data Studio allows you to save and open SQL scripts in the **SQL Terminal**. After saving the changes, SQL Terminal will be changed to a File Terminal.

Saving an SQL Script
--------------------

The **Save** option saves the File Terminal content to the associated file. ,

Follow the steps to save an SQL script:

#. Perform any of the following operations:

   -  Choose **File > Save** from the main menu.
   -  Press "Ctrl + S" to save the SQL terminal content.
   -  Click **Save** on the toolbar or right-click the **SQL Terminal** and select **Save**.

   The **Data Studio Security Disclaimer** dialog box is displayed.

#. Click **OK**.

   Data Studio displays the status of the operation in the status bar.

   .. note::

      -  The script is saved as an SQL file. Data Studio sets the read/write permission for the saved SQL file. To ensure security, you must set the read/write permissions for folders.
      -  When a change is made in a file and if that associated file is unavailable, it will trigger the **Save As** option.

         -  In any case, if saving of the source file is failed due to some reasons, then user is prompted with the **Save As** option to save the content as a new source file. If you choose not to save (that is cancelling **Save As**), then File Terminal gets converted into an SQL Terminal.
         -  The changes made to File Terminals are not Auto Saved.

Saving an SQL Script in New File
--------------------------------

The **Save As** option saves the terminal content to a new file.

Follow the steps to save an SQL script:

#. Perform any of the following operations:

   -  Choose **File > Save As** from the main menu.
   -  Alternatively click "Ctrl + Alt + S key to save SQL terminal or File terminal content in new file.

   The **Data Studio Security Disclaimer** dialog box is displayed.

#. Click **OK**.

   The **Save As** dialog box is displayed.

#. Select the location to save the script and click **Save**.

.. note::

   When there are unsaved changes in File Terminals, user will be given an option to save or cancel on graceful exit of data studio.
