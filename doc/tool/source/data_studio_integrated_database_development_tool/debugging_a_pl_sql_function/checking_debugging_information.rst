:original_name: DWS_DS_624.html

.. _DWS_DS_624:

Checking Debugging Information
==============================

When you use Data Studio, you can examine debugging information through several debugging panes. This section describes how to check the debugging information:

-  :ref:`Operating on Variables <en-us_topic_0000001145913161__en-us_topic_0185264399_section37449496>`
-  :ref:`Viewing Results <en-us_topic_0000001145913161__en-us_topic_0185264399_section1351626183918>`

.. _en-us_topic_0000001145913161__en-us_topic_0185264399_section37449496:

Operating on Variables
----------------------

The **Variables** pane is used to monitor information or evaluate values. The **Variables** pane can be opened from the minimized window to evaluate or modify variables or parameters in a PL/SQL procedure. As you step through the code, the values of some local variables may be changed.

|image1|

.. note::

   Press **Alt+K** to copy the content of the **Variables** pane.

You can double-click the corresponding line of the variable and manually change its value during runtime.

Click the **Variable**, **Datatype**, or **Value** column in the **Variables** pane to sort the values. For example, to change the value of the percentage variable from 5 to 15, double-click the corresponding line in the **Variable** pane. The **Set Variable Value** dialog box will be displayed, which prompts you to enter the variable value. Input the variable value and click **OK**.

To set **NULL** as a variable value, enter **NULL** or **null** in the **Value** column.

If a variable is read-only, |image2| will be displayed next to it.

Read-only variables cannot be updated. A variable declared as a constant will not be shown as read-only in the **Variables** pane. However, an error will be reported when this variable is updated.

.. note::

   -  If **'NULL'** or **NULL** is entered in the **Variables** pane, the parameter value will be displayed as **NULL**. This rule also applies to character datatypes.
   -  If the value is set to a variable using Data Studio, the value of the variable is the same as the value returned by the statement **SELECT EXPRESSION** executed using gsql.

+-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Setting/Displaying Variables      | Description                                                                                                                                                                 |
+===================================+=============================================================================================================================================================================+
| Setting a variable to **NULL**    | #. Double-click a variable value in the **Variables** pane.                                                                                                                 |
|                                   |                                                                                                                                                                             |
|                                   |    A dialog box is displayed.                                                                                                                                               |
|                                   |                                                                                                                                                                             |
|                                   | #. Left the parameter blank.                                                                                                                                                |
+-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Setting a string value            | Some examples are as follows:                                                                                                                                               |
|                                   |                                                                                                                                                                             |
|                                   | -  To set the value to **abc**, enter **abc**.                                                                                                                              |
|                                   | -  To set the value to **Master's Degree**, enter **Master''s Degree**.                                                                                                     |
|                                   | -  To set the value as text (**NULL**), set it to **NULL** in the **Variables** pane.                                                                                       |
+-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Setting a **BOOLEAN** value       | Enclose the **BOOLEAN** value **t** or **f** within single quotation marks ('). For example, to set **t** to a **BOOLEAN** variable, enter **'t'** in the **Value** column. |
+-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Displaying a variable value       | If the variable value is **NULL**, it will be displayed as **NULL**.                                                                                                        |
|                                   |                                                                                                                                                                             |
|                                   | If the variable value is **NULL**, it will be displayed as empty.                                                                                                           |
|                                   |                                                                                                                                                                             |
|                                   | If the variable value is a string, for example, *abc*, it will be displayed as *abc*.                                                                                       |
+-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001145913161__en-us_topic_0185264399_section1351626183918:

Viewing Results
---------------

The **Result** tab displays the result of the PL/SQL debugging session, with the corresponding procedure name at the top of the tab. The **Result** tab is automatically displayed only when the result of executing a PL/SQL program exists.

You can click |image3| in the **Result** tab to copy the content of the tab. For details, see :ref:`Using SQL Terminals <dws_ds_128>`.

.. note::

   -  The tool tip in the **Result** tab displays a maximum of 10 lines, each of which contains up to 80 characters.
   -  If the result of an executed query is **NULL**, it will be displayed as **<NULL>**.
   -  Tab characters (ASCII 009) in table data will not be displayed in the **Result**, **View Table Data**, or **Properties** pane. Tab characters will be included correctly in the copied or exported data, and displayed correctly in the tool tip.

.. |image1| image:: /_static/images/en-us_image_0000001145713195.png
.. |image2| image:: /_static/images/en-us_image_0000001145913387.jpg
.. |image3| image:: /_static/images/en-us_image_0000001098993280.jpg
