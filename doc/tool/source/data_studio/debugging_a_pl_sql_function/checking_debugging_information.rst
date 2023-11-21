:original_name: DWS_DS_624.html

.. _DWS_DS_624:

Checking Debugging Information
==============================

When you use Data Studio, you can examine debugging information through several debugging tabs. This section describes how to check the debugging information:

-  :ref:`Operating on Variables <en-us_topic_0000001233800681__en-us_topic_0185264399_section37449496>`
-  :ref:`Viewing Results <en-us_topic_0000001233800681__en-us_topic_0185264399_section1351626183918>`

.. _en-us_topic_0000001233800681__en-us_topic_0185264399_section37449496:

Operating on Variables
----------------------

The **Variables** pane is used to monitor information or evaluate values. The **Variables** pane can be opened from the minimized window panel. Using this pane, you can evaluate or modify variables or arguments in a PL/SQL function. As you step through the code, the values of some local variables may change.

|image1|

.. note::

   The content of the **Variables** pane can be copied to the clipboard using **Alt**\ +\ **K**.

You can double-click the corresponding row of the variable and manually change variable values during run-time.

Click the **Variable**, **Datatype**, or **Value** column in the **Variables** pane to sort the values. For example, to change the value of the percentage variable from 5 to 15, double-click the corresponding row in the **Variable** pane. The **Set Variable Value** dialog box will be displayed, which prompts you to input the variable value. Input the variable value and click **OK**.

To set **NULL** as a variable value, enter **NULL** or **null** in the **Value** column.

If the variable is read-only, it will be indicated by |image2| beside the corresponding variable.

Users cannot update these variables. A variable declared as a constant will not be shown as read-only in the **Variables** pane. However, while updating it, an error will occur.

.. note::

   -  In the **Variables** pane, the parameter value will be displayed as **NULL**, if the input to the parameter value is string literal 'NULL'.
   -  When the value is set to a variable using Data Studio, then the value of the variable is the same as the value returned by the statement "select expression" executed from **gsql**.

+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| Setting/Displaying Variables      | Description                                                                                                                          |
+===================================+======================================================================================================================================+
| Setting NULL Values               | #. Double-click a variable value in the **Variables** pane.                                                                          |
|                                   |                                                                                                                                      |
|                                   |    A dialog box is displayed.                                                                                                        |
|                                   |                                                                                                                                      |
|                                   | #. Set the variable to an empty value.                                                                                               |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| Configuring String Values         | Configure the string values as follows:                                                                                              |
|                                   |                                                                                                                                      |
|                                   | -  To configure **abc**, enter **abc**.                                                                                              |
|                                   | -  To configure **Master's Degree**, enter **Master's Degree**.                                                                      |
|                                   | -  To set variable as text (**NULL**), configure **NULL** in the **Variables** pane.                                                 |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| Setting Boolean Values            | Enclose the boolean values *t* or *f* within single quotes. To set *t* to a boolean variable, enter *'t'* in the **Variables** pane. |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+
| Displaying Variable Value         | If the variable value is **NULL** text, it will be displayed as **NULL**.                                                            |
|                                   |                                                                                                                                      |
|                                   | If the variable value is **NULL**, it will be displayed as empty.                                                                    |
|                                   |                                                                                                                                      |
|                                   | If the variable value is a string, for example, *abc*, it will be displayed as *abc*.                                                |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001233800681__en-us_topic_0185264399_section1351626183918:

Viewing Results
---------------

The **Result** tab displays the output for the PL/SQL debugging session, with the corresponding function/procedure name at the top of the tab. The **Result** tab will appear automatically, only if there is a result for the executed PL/SQL program.

You can copy the content of the **Result** tab, by clicking |image3|. For details, see :ref:`Working with SQL Terminals <dws_ds_128>`.

.. note::

   -  The tool tip in the **Result** tab displays a maximum of 10 lines, where each line contains maximum of 80 characters.
   -  If the result of the executed query is NULL, it will be displayed as **<NULL>**.
   -  Tab characters (ASCII 009) in table data will not be displayed in the **Results/View Table Data/Properties** window. Tab characters will be included correctly when copying/exporting the data. Tool tip will also display the tab characters correctly.

.. |image1| image:: /_static/images/en-us_image_0000001188202700.png
.. |image2| image:: /_static/images/en-us_image_0000001234042435.jpg
.. |image3| image:: /_static/images/en-us_image_0000001233801001.jpg
