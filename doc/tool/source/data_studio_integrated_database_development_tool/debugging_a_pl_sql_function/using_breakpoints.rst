:original_name: DWS_DS_622.html

.. _DWS_DS_622:

Using Breakpoints
=================

Topics in this section include:

-  :ref:`Using the Breakpoints Pane <en-us_topic_0000001145513153__en-us_topic_0185264940_section7797521165554>`
-  :ref:`Setting or Adding a Breakpoint on a Line <en-us_topic_0000001145513153__en-us_topic_0185264940_section102263371586>`
-  :ref:`Enabling or Disabling a Breakpoint on a Line <en-us_topic_0000001145513153__en-us_topic_0185264940_section2403313617042>`
-  :ref:`Removing a Breakpoint from a Line <en-us_topic_0000001145513153__en-us_topic_0185264940_section6545757217250>`
-  :ref:`Changing Source Code <en-us_topic_0000001145513153__en-us_topic_0185264940_section3380880717344>`
-  :ref:`Debugging a PL/SQL Program Using a Breakpoint <en-us_topic_0000001145513153__en-us_topic_0185264940_section18232123785817>`

A breakpoint is used to stop a PL/SQL program on the line where the breakpoint is set. You can use breakpoints to control the execution and debug the procedure.

-  When a line with a breakpoint set is reached, the PL/SQL program on this line will be stopped, and you can perform other debugging operations. Data Studio supports the following breakpoint-related operations:

   -  Setting or creating a breakpoint on a line
   -  Enabling or disabling the breakpoint on a line
   -  Removing the breakpoint from a line

-  A disabled breakpoint will not stop a PL/SQL program.

When you run a PL/SQL program, the execution stops on each line with a breakpoint set. In this case, Data Studio retrieves information about the current program state, such as the values of the program variables.

Perform the following steps to debug a PL/SQL program:

#. Set a breakpoint on the line where PL/SQL program needs to be stopped.

#. Start the debugging session.

   When a line with a breakpoint set is reached, monitor the program state in the **Debugging** pane, and continue to execute the program.

#. Close the debugging session.

Data Studio provides debugging options in the toolbar that help you step through the debugging objects.

.. _en-us_topic_0000001145513153__en-us_topic_0185264940_section7797521165554:

Using the Breakpoints Pane
--------------------------

You can view and manage all breakpoints in the **Breakpoints** pane. Click the breakpoint option at the minimized pane to open the **Breakpoints** pane.

The **Breakpoints** pane lists all lines with a breakpoint set and the debugging object names.

You can enable or disable all the breakpoints by clicking |image1| in the **Breakpoints** pane. In the **Breakpoints** pane, you can select the breakpoint check box and click |image2|, |image3|, or |image4| to enable, disable or remove a specific breakpoint.

In the **PL/SQL Viewer** pane, double-click the required breakpoint in the **Breakpoint Info** column to locate the breakpoint.

|image5|

.. note::

   -  When a breakpoint is disabled, the program will not be stopped at the breakpoint. The breakpoint will be retained and can be enabled later.
   -  A removed breakpoint cannot be restored.
   -  Press **Alt+Y** to copy the content of the **Breakpoints** pane.

.. _en-us_topic_0000001145513153__en-us_topic_0185264940_section102263371586:

Setting or Adding a Breakpoint on a Line
----------------------------------------

Perform the following steps to set or add a breakpoint on a line:

#. Open the PL/SQL function on the line where you want to add a breakpoint.
#. In the **PL/SQL Viewer** pane, double-click the breakpoint ruler on the left of the line number. If |image6| is displayed, the breakpoint has been added.

   .. note::

      If the function is not interrupted or stopped during debugging, the breakpoint set for the function will not be validated.

.. _en-us_topic_0000001145513153__en-us_topic_0185264940_section2403313617042:

Enabling or Disabling a Breakpoint on a Line
--------------------------------------------

Once a breakpoint is set, you can temporarily disable it by selecting the corresponding check box in the **Breakpoints** pane and clicking |image7| at the top of **Breakpoints**. A disabled breakpoint will be grayed out as |image8| in **PL/SQL Viewer** and **Breakpoints** panes. To enable a disabled breakpoint, select the corresponding check box and click |image9|.

.. _en-us_topic_0000001145513153__en-us_topic_0185264940_section6545757217250:

Removing a Breakpoint from a Line
---------------------------------

You can remove an unused breakpoint using the same method as that for creating a breakpoint.

In the **PL/SQL Viewer** pane, open the function in which you want to remove the breakpoint. Double-click |image10| in **PL/SQL Viewer** to remove the breakpoint.

You can also enable or disable breakpoints in **PL/SQL Viewer** using the preceding method.

.. _en-us_topic_0000001145513153__en-us_topic_0185264940_section3380880717344:

Changing Source Code
--------------------

If you debug an object after changing the source code obtained from the server, Data Studio displays an error.

You are advised to refresh the object and debug it again.

.. note::

   If you change the source code obtained from the server and execute or debug the source code without setting a breakpoint, the result of the source code obtained from the server will be displayed on Data Studio. You are advised to refresh the source code before executing or debugging it.

.. _en-us_topic_0000001145513153__en-us_topic_0185264940_section18232123785817:

Debugging a PL/SQL Program Using a Breakpoint
---------------------------------------------

Perform the following steps to debug a PL/SQL program using a breakpoint:

#. Open the PL/SQL program and create a breakpoint on the line to be debugged.

   An example is as follows:

   Lines 11, 12, 13

   |image11|

#. To start debugging, click |image12| or press **Ctrl+D**, or right-click the selected PL/SQL program in **Object Browser** and click **Debug**. In the **Debug Function/Procedure** dialog box, enter the parameter information.

   .. note::

      If no parameter is entered, the **Debug Function/Procedure** dialog box will not be displayed.

#. Enter the information and click **OK**. Single quotation marks (') are mandatory for the parameters of **varchar** and **date** data types, but not mandatory for the parameters of **numeric** data type.

   To set the parameter to **NULL**, enter **NULL** or **null**.

   After clicking **Debug**, you will see |image13| pointing to the line where the breakpoint is set. This line is the first line where the execution resumes.

   |image14|

   You can terminate debugging by clicking |image15| in the toolbar, or pressing **F10**, or select **Terminate Debugging** in the **Debug** menu. After the debugging is complete, the function execution proceeds and will not be stopped at any breakpoint.

   Relevant information will be displayed in **Callstack** and **Variables** panes.

   |image16|

   The **Variables** pane shows the current values of variables. If you hover over the variable of a function/procedure, the current value is also displayed.

   |image17|

   You can step through the code using **Step Into**, **Step Out** or **Step Over**. For details, see :ref:`Controlling Execution <dws_ds_623>`.

#. Click |image18| to continue the execution till the next breakpoint (if any). The result of the executed PL/SQL program is displayed in the **Result** tab and the **Callstack** and **Variables** panes are cleared. You can click |image19| in the **Result** tab to copy the content of the tab.

   Perform the following operations to remove a breakpoint:

   -  Double-click a breakpoint to remove it from **PL/SQL Viewer**.
   -  Select the breakpoint in the breakpoint check box and click |image20| in the **Breakpoints** pane.

Rearranging the Variables Pane
------------------------------

You can arrange the **Variables** pane and its columns to the following positions:

-  Next to the **SQL Assistant** tab
-  Next to the **SQL Terminal** tab
-  Next to the **Object Browser** tab
-  Next to the **Result** tab
-  Next to the **Breakpoints** tab
-  Next to the **Callstack** tab
-  Next to the **Object Browser** tab

.. note::

   When debugging is complete, the **Variables** pane will be minimized regardless of its position. If the **Variables** pane is moved next to the **SQL Terminal** or **Result** tab, you need to minimize the pane after debugging is complete. The position of the **Variables** pane remains unchanged after it is rearranged.

Enabling/Disabling System Variables
-----------------------------------

System variables are displayed by default in the **Variables** pane. You can disable the display of system variables if necessary.

#. Click the button under **Variables** to disable the display of system variables.

   |image21|

   The button is toggled on by default.

Displaying Cached Parameters
----------------------------

When a PL/SQL function or procedure is debugged or executed, the same parameter values are used for the next debugging or execution.

When a PL/SQL object is executed, the following window is displayed.

|image22|

The **Value** column is empty upon the first execution. Enter the values as required.

|image23|

Click **OK**. The parameter values will be cached. The cached parameter values will be displayed in the next execution or debugging.

.. note::

   Once a specific connection is removed, all the cached parameter values are cleared.

Displaying Variable in the Monitor Pane
---------------------------------------

Data Studio displays the variables which are being monitored in the **Monitor** pane during debugging.

In the **Monitor** pane, add a variable in the following ways:

-  Select a variable from the **Variables** pane and right-click the variable.

-  Select a variable from the **Variable** column and add it by clicking the button in the **Variables** window toolbar.

   |image24|

   If the variable is monitored, its value in the **Monitor** pane will always be the same as that in the **Variables** pane.

-  During function/procedure debugging, right-click the variable to be added in the editor and choose **Add Variable To Monitor** in the menu displayed.

|image25|

The **Monitor** pane can be dragged to anywhere in the **Data Studio** window.

Hovering Over a Variable to View Its Information During Debugging
-----------------------------------------------------------------

When debugging a PL/SQL function in Data Studio, you can hover over a variable to view its information.

|image26|

Supporting Rollback/Commit During Debugging
-------------------------------------------

Data Studio allows committing or rolling back the PL/SQL query result after debugging is complete.

-  If **Debug With Rollback** is enabled, the PL/SQL execution result after debugging is not saved to the database.
-  If **Debug With Rollback** is disabled, the PL/SQL execution result after debugging is submitted to the database.

Perform the following steps to enable the rollback function:

#. Check the **Debug With Rollback** box and enable the rollback function during PL/SQL debugging.

   Or

   Right-click the **SQL Terminal** pane where the PL/SQL function is executed.

   |image27|

   Select **Debug With Rollback** to enable the rollback function after the debugging is complete.

   Or

   Right-click any PL/SQL function under **Functions/Procedures** in **Object Browser**.

   |image28|

.. |image1| image:: /_static/images/en-us_image_0000001145513237.png
.. |image2| image:: /_static/images/en-us_image_0000001098833238.jpg
.. |image3| image:: /_static/images/en-us_image_0000001145713167.jpg
.. |image4| image:: /_static/images/en-us_image_0000001098833234.jpg
.. |image5| image:: /_static/images/en-us_image_0000001145833095.jpg
.. |image6| image:: /_static/images/en-us_image_0000001098993252.png
.. |image7| image:: /_static/images/en-us_image_0000001098833250.jpg
.. |image8| image:: /_static/images/en-us_image_0000001099153346.jpg
.. |image9| image:: /_static/images/en-us_image_0000001145833093.png
.. |image10| image:: /_static/images/en-us_image_0000001145833093.png
.. |image11| image:: /_static/images/en-us_image_0000001145833089.jpg
.. |image12| image:: /_static/images/en-us_image_0000001098993240.jpg
.. |image13| image:: /_static/images/en-us_image_0000001098673418.jpg
.. |image14| image:: /_static/images/en-us_image_0000001145513243.jpg
.. |image15| image:: /_static/images/en-us_image_0000001099153218.jpg
.. |image16| image:: /_static/images/en-us_image_0000001098673416.jpg
.. |image17| image:: /_static/images/en-us_image_0000001098833236.png
.. |image18| image:: /_static/images/en-us_image_0000001145713161.jpg
.. |image19| image:: /_static/images/en-us_image_0000001098993236.jpg
.. |image20| image:: /_static/images/en-us_image_0000001099153348.jpg
.. |image21| image:: /_static/images/en-us_image_0000001098673408.png
.. |image22| image:: /_static/images/en-us_image_0000001145513235.png
.. |image23| image:: /_static/images/en-us_image_0000001145833097.png
.. |image24| image:: /_static/images/en-us_image_0000001145913209.png
.. |image25| image:: /_static/images/en-us_image_0000001099153210.png
.. |image26| image:: /_static/images/en-us_image_0000001437136861.png
.. |image27| image:: /_static/images/en-us_image_0000001145913211.png
.. |image28| image:: /_static/images/en-us_image_0000001145513253.png
