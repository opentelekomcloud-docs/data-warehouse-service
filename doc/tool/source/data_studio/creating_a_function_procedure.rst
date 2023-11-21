:original_name: DWS_DS_57.html

.. _DWS_DS_57:

Creating a Function/Procedure
=============================

Perform the following steps to create a function/procedure and SQL function:

#. In the **Object Browser** pane, right-click **Functions/Procedures** under the schema where you want to create the function/procedure. Then select **Create PL/SQL Function**, **Create SQL Function**, **Create PL/SQL Procedure**, or **Create C Function** as required.

   The selected template is displayed in the new tab of Data Studio.

#. Add the function/procedure by right-clicking the tab and selecting **Compile**, or choosing **Run > Compile/Execute Statement** from the main menu, or pressing **Ctrl+Enter** to compile the procedure.

   The **Created function/procedure Successfully** dialog box is displayed, and the new function/procedure is displayed under the **Object Browser**. Click **OK** to close the **NewObject()** tab and add the debugging object to **Object Browser**.

   Refer to :ref:`Execute SQL Queries <en-us_topic_0000001234200573__en-us_topic_0185264856_section16147111413113>` for re-obtaining connection options when connections are lost during execution.

#. The asterisk (*) next to the procedure name indicates that the procedure is not compiled or added to **Object Browser**.

   Refresh **Object Browser** by pressing **F5** to view the newly added debugging object.

   .. note::

      -  C functions do not support debugging operations.
      -  A popup message displays the status of the completed operation, which is not displayed in the status bar.

Compiling a Function
--------------------

When a user creates a PL/SQL object from the template or by editing an existing PL/SQL object, the created PL/SQL object will be displayed in a new tab page.

Perform the following steps to compile a created function:

#. Select **Functions/Procedures** from the **Object Browser** tab page.

#. Right-click **Functions/Procedures** and a menu is displayed.

   |image1|

#. Click **Create PL/SQL Function** and a new tab page is opened.

   |image2|

#. Edit the code.

#. Right-click the blank area of the tab page and a menu is displayed.

   |image3|

#. Click **Compile**. A pop-up message is displayed as follows.

   |image4|

   The function is displayed in a new tab page.

.. |image1| image:: /_static/images/en-us_image_0000001188521250.png
.. |image2| image:: /_static/images/en-us_image_0000001234042285.png
.. |image3| image:: /_static/images/en-us_image_0000001234200775.png
.. |image4| image:: /_static/images/en-us_image_0000001188681168.png
