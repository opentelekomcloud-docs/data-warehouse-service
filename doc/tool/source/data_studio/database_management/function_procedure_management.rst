:original_name: DWS_DS_015.html

.. _DWS_DS_015:

Function/Procedure Management
=============================

Creating a Function/Procedure
-----------------------------

#. In the **Object Browser** pane, right-click **Functions/Procedures** under the schema where you want to create the function/procedure. Then select **Create Function**, **Create SQL Function**, **Create Procedure**, or **Create C Function** as required.

   Right-click **Functions/Procedures** and a menu is displayed.

   |image1|

#. The selected template is displayed in the new tab of Data Studio.

   |image2|

#. After adding a function or procedure, you can choose **Run** > **Compile/Execute Statement** from the main menu or right-click the tab and select **Compile** to compile the function or procedure.

   |image3|

#. The new function/procedure is displayed under **Object Browser**.

   .. note::

      -  The asterisk (*) next to the procedure name indicates that the procedure is not compiled or added to **Object Browser**.
      -  Refresh **Object Browser** by pressing **F5** to view the newly added debugging object.
      -  C functions do not support debugging operations.
      -  A pop-up message displays the status of the completed operation, which is not displayed in the status bar.

Editing a Function/Procedure
----------------------------

#. In the **Object Browser** pane, double-click or right-click the required function/procedure or SQL function and select **View Source**.

   The selected function/procedure or SQL function is displayed in the **PL/SQL Viewer** tab page.

   |image4|

   .. note::

      -  You must refresh **Object Browser** to view the latest DDL.
      -  If multiple functions/procedures or SQL functions have the same schema, name, and input parameters, only one of them can be opened at a time.

#. After editing or updating, compile and execute the PL/SQL program or SQL function.

   If you execute the function/procedure or SQL function before compilation, the **Source Code Change** dialog box is displayed.

#. Click **Yes** to compile and execute the PL/SQL function/procedure. The status of the completed operation is displayed in the **Message** tab page.

#. After compiling the function/procedure or SQL function, press **F5** to refresh **Object Browser** to view the updated code.

Granting or Revoking a Permission
---------------------------------

#. Right-click **Function/Procedure Group** and select **Grant/Revoke**. The **Grant/Revoke** dialog box is displayed.
#. Open the **Object Selection** tab to select the desired objects, and click **Next**.
#. The **Privilege Selection** tab is displayed. Select the role from the **Role** drop-down list.
#. On the **SQL Preview** tab, you can check the automatically generated SQL query. If the result does not meet the expectation, return to the previous step until the result meets the expectation.
#. Click **Finish**.

Debugging a Function/Procedure
------------------------------

A breakpoint is used to stop a PL/SQL program on the line where the breakpoint is set. You can use breakpoints to control the execution and debug the procedure. When a line with a breakpoint set is reached, the PL/SQL program on this line will be stopped, and you can perform other debugging operations.

-  Setting or Adding a Breakpoint on a Line

   Data Studio allows you to set or add breakpoints on a line.

   Open the function or procedure for which you want to add a breakpoint, double-click the breakpoint ruler on the left of the line number field, and set a breakpoint. If the breakpoint is enabled, the operation is successful. If the function is not interrupted or stopped during debugging, the breakpoint set for the function is not validated.

-  Using the Breakpoints Pane

   You can view and manage all breakpoints in the **Breakpoints** pane. Click the breakpoint option at the minimized pane to open the **Breakpoints** pane.

   You can enable, disable or remove a specific breakpoint by selecting the breakpoint check box and clicking the enabling, disabling, and removing icon in the **Breakpoints** pane. You can also double-click the breakpoint ruler on the left of the line number field to set a breakpoint or double-click the breakpoint icon to delete the breakpoint.

   In the **PL/SQL Viewer** pane, double-click the required breakpoint in the **Breakpoint Info** column to locate the breakpoint.

   |image5|

   .. note::

      -  The **Breakpoints** pane lists each breakpoint with the row number and the debug object name.
      -  When a breakpoint is disabled, the program will not be stopped at the breakpoint. The breakpoint will be retained and can be enabled later.
      -  A removed breakpoint cannot be restored.
      -  Press **Alt+Y** to copy the content of the **Breakpoints** pane.

-  Changing Source Code

   During debugging, if the source code is changed after it is fetched from the server and the debugging is continued, Data Studio displays an error. You are advised to refresh the object and perform the debug operation again.

   If you change the source code obtained from the server and execute or debug the source code without setting a breakpoint, the result of the source code obtained from the server will be displayed on Data Studio. You are advised to refresh the source code before executing or debugging it.

-  Using Breakpoints to Debug Functions/Procedures

   After adding a breakpoint in the row you want to debug, click the debug icon or right-click the selected function in **Object Browser** and select **Debug**. In the **Debug Function/Procedure** dialog box, enter the parameter information.

   |image6|

   .. note::

      -  If no parameter is entered, the **Debug Function/Procedure** dialog box will not be displayed.
      -  Single quotation marks (') are mandatory for the parameters of **varchar** and **date** data types, but not mandatory for the parameters of **numeric** data type. To set the parameter to **NULL**, enter **NULL** or **null**.
      -  When a function or procedure is debugged or executed, the same parameter values are used for the next debugging or execution. The **Value** column is empty upon the first execution. Enter the values as required. Click **OK**. The parameter values will be cached. The cached parameter values will be displayed in the next execution or debugging. Once a specific connection is removed, all the cached parameter values are cleared.

   -  The **Callstack** pane is populated.

      |image7|

   -  The **Variables** pane shows the current value of variables. If you hover over the variable of a function/procedure, the current value is also displayed. System variables are displayed by default in the **Variables** pane. You can disable the display of system variables if necessary. The button is toggled on by default.

      |image8|

      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Setting/Displaying Variables      | Description                                                                                                                                                                 |
      +===================================+=============================================================================================================================================================================+
      | Setting a variable to **NULL**    | #. Double-click a variable value in the **Variables** pane.                                                                                                                 |
      |                                   |                                                                                                                                                                             |
      |                                   |    A dialog box is displayed.                                                                                                                                               |
      |                                   |                                                                                                                                                                             |
      |                                   | #. Set the variable to an empty value.                                                                                                                                      |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Setting a string value            | Some examples are as follows:                                                                                                                                               |
      |                                   |                                                                                                                                                                             |
      |                                   | -  To set the value to **abc**, enter **abc**.                                                                                                                              |
      |                                   | -  To set the value to **Master's Degree**, enter **Master's Degree**.                                                                                                      |
      |                                   | -  To set the value as text (**NULL**), set it to **NULL** in the **Variables** pane.                                                                                       |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Setting a **BOOLEAN** value       | Enclose the **BOOLEAN** value **t** or **f** within single quotation marks ('). For example, to set **t** to a **BOOLEAN** variable, enter **'t'** in the **Value** column. |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Displaying a variable value       | If the variable value is **NULL**, it will be displayed as **NULL**.                                                                                                        |
      |                                   |                                                                                                                                                                             |
      |                                   | If the variable value is **NULL**, it will be displayed as empty.                                                                                                           |
      |                                   |                                                                                                                                                                             |
      |                                   | If the variable value is set to a string, for example, **abc**, it will be displayed as **abc**.                                                                            |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   -  During function/procedure debugging, right-click the variable to be added in the editor and choose **Add Variable To Monitor** in the menu displayed. If the variable is monitored, its value in the **Monitor** pane will always be the same as that in the **Variables** pane.

   -  In Data Studio, variable information is displayed if the cursor is hovered over that variable during the debugging of PL/SQL functions.

-  Terminating Debugging

   Click the terminating debugging button on the toolbar or choose **Debug** > **Terminate Debugging**. After the debugging is complete, the function execution proceeds and will not be stopped at any breakpoint.

   After the debugging is complete, the result tab page displays the function execution result, and the **Callstack** and **Variables** panes are cleared.

-  Data Studio provides the option to commit/rollback the query execution result after debugging is finished. Right-click the **SQL Terminal** window where the function is executed. Select **Debug With Rollback** to enable the rollback function after the debugging is complete.

   -  If **Debug With Rollback** option is enabled, the function execution result after debugging is not saved to the database.
   -  If **Debug With Rollback** option is disabled, the function execution result after debugging is submitted to the database.

Controlling Execution
---------------------

-  Single Stepping a PL/SQL Function

   You can run the command for single stepping in the toolbar to debug a function. This allows you to debug the program line by line. When a breakpoint occurs during the operation of single stepping, the operation will be suspended and the program will be stopped.

   Single stepping means executing one statement at a time. Once a statement is executed, you can see the execution result in other debugging tabs.

   .. note::

      A maximum of 100 **PL/SQL Viewer** tabs can be displayed at a time. If more than 100 tabs are opened, the tabs of function calls will be closed. For example, if 100 tabs are opened and a new debugging object is called, Data Studio will close the tabs of function calls and open the tab of the new debugging object.

-  Step into

   To step through code one statement at a time, select **Step Into** from the **Debug** menu.

   When stepping into a function, Data Studio executes the current statement and then enters the break mode. The debug position will be indicated by an arrow |image9| on the left ruler pane. If the execution statement calls another function, Data Studio will step into that function. Once you have stepped through all the statements in that function, Data Studio jumps to the next statement of the function it was called from.

   Press **F7** to go to the next statement. If you click **Continue**, PL/SQL code execution will continue.

   |image10|

-  Step Over

   **Step Over** is the same as **Step Into**. Unless another function is called, it will not step into the function. The function will run, and the next statement in the current function is executed. **F8** is the shortcut key for **Step Over**. However, if there is a breakpoint set inside the called function, **Step Over** will enter the function, and hit the set breakpoint.

-  Step Out

   Stepping out of a sub-program continues the execution of the function and then suspends the execution after the function returns to its function call. You can step out this function when the rest of the function does not need to debug. However, if a breakpoint is set in the remaining part of the function, then that breakpoint will be hit before returning to the function call.

   A function will be executed when you step over or step out of it. **Shift+F7** is the shortcut key for **Step Out**.

   |image11|

-  Continuing the Debugging

   When the debugging process stops at a specific location, you can select **Continue** (or press **F9**) from the **Debug** menu to continue the PL/SQL function execution.

-  Viewing Callstack

   The **Callstack** pane displays the chain of functions as they are called. The **Callstack** pane can be opened from the minimized window. The most recent functions are listed at the top, and the least recent at the bottom. At the end of each function name is the current line number in that function.

   You can double-click the function names in the **Callstack** pane to open panes of different functions.

   |image12|

Exporting a Function or Procedure DDL
-------------------------------------

Follow the steps below to export the function/procedure DDL:

#. In the **Object Browser** pane, right-click the selected function or procedure and select **Export DDL**.

   You need to customize the export path. To compress data, select **.zip**.

   |image13|

   You must select **I agree** under **Security Disclaimer**, then click **OK**. You can disable the security disclaimer. After the disclaimer is disabled, it will not be displayed when you export the DDL. For details, see :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>`.

#. Click **OK**. The operation progress is displayed on the status bar in the lower right corner.

   .. note::

      -  If the file name contains characters that are not supported by Windows, the file name will be different from the schema name.
      -  MS Visual C runtime file (msvcrt100.dll) is required to complete this operation. For details, see :ref:`Troubleshooting <dws_ds_039>`.

   The **Data Exported Successfully** dialog box and status bar display the status of the completed operation.

   .. table:: **Table 1** Supported DDL encoding formats

      ================= ============= ===========================
      Database Encoding File Encoding Support for Exporting a DDL
      ================= ============= ===========================
      UTF-8             UTF-8         Yes
      \                 GBK           Yes
      \                 LATIN1        Yes
      GBK               GBK           Yes
      \                 UTF-8         Yes
      \                 LATIN1        No
      LATIN1            LATIN1        Yes
      \                 GBK           No
      \                 UTF-8         Yes
      ================= ============= ===========================

Deleting a Function/Procedure
-----------------------------

You can delete functions or programs one by one or in batches.

#. In the **Object Browser** pane, right-click the selected function/procedure object and select **Drop Object**.

#. Select one or more function or procedure objects and choose **Delete Object**.

#. In the dialog box that is displayed, click **Yes**.

   The status bar displays the status of the completed operation.

.. |image1| image:: /_static/images/en-us_image_0000001813439448.png
.. |image2| image:: /_static/images/en-us_image_0000001813599232.png
.. |image3| image:: /_static/images/en-us_image_0000001813439468.png
.. |image4| image:: /_static/images/en-us_image_0000001813439436.jpg
.. |image5| image:: /_static/images/en-us_image_0000001813439456.jpg
.. |image6| image:: /_static/images/en-us_image_0000001813599220.png
.. |image7| image:: /_static/images/en-us_image_0000001860199297.jpg
.. |image8| image:: /_static/images/en-us_image_0000001860199321.png
.. |image9| image:: /_static/images/en-us_image_0000001813439464.jpg
.. |image10| image:: /_static/images/en-us_image_0000001860199325.jpg
.. |image11| image:: /_static/images/en-us_image_0000001860199317.jpg
.. |image12| image:: /_static/images/en-us_image_0000001813439440.jpg
.. |image13| image:: /_static/images/en-us_image_0000001860199329.png
