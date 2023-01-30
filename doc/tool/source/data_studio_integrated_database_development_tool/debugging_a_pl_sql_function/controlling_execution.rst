:original_name: DWS_DS_623.html

.. _DWS_DS_623:

Controlling Execution
=====================

Topics in this section include:

-  :ref:`Starting Debugging <en-us_topic_0000001098993176__en-us_topic_0185264594_section1360793411367>`
-  :ref:`Single Stepping a PL/SQL Function <en-us_topic_0000001098993176__en-us_topic_0185264594_section49567323716>`
-  :ref:`Continuing the Debugging <en-us_topic_0000001098993176__en-us_topic_0185264594_ref471314683>`
-  :ref:`Viewing Callstack <en-us_topic_0000001098993176__en-us_topic_0185264594_viewcallstack>`

.. _en-us_topic_0000001098993176__en-us_topic_0185264594_section1360793411367:

Starting Debugging
------------------

Select the function that you want to debug in the **Object Browser** pane. Start debugging by clicking |image1| on the toolbar or using any other method mentioned in previous sections. If no breakpoint is set or the breakpoint set is invalid, the debugging operation is performed without stopping any statement and Data Studio will simply execute the object and display the results (if any).

.. _en-us_topic_0000001098993176__en-us_topic_0185264594_section49567323716:

Single Stepping a PL/SQL Function
---------------------------------

You can run the command for single stepping in the toolbar to debug a function. This allows you to debug the program line by line. When a breakpoint occurs during the operation of single stepping, the operation will be suspended and the program will be stopped.

Single stepping means executing one statement at a time. Once a statement is executed, you can see the execution result in other debugging tabs.

.. note::

   A maximum of 100 **PL/SQL Viewer** tabs can be displayed at a time. If more than 100 tabs are opened, the tabs of function calls will be closed. For example, if 100 tabs are opened and a new debugging object is called, Data Studio will close the tabs of function calls and open the tab of the new debugging object.

Step Into
---------

To execute the code by statement, select **Step Into** from the **Debug** menu, or click |image2|, or press **F7**.

When stepping into a function, Data Studio executes the current statement and then enters the debugging mode. The debugged line will be indicated by an arrow |image3| on the left. If the executed statement calls another function, Data Studio will step into that function. Once you have stepped through all the statements in that function, Data Studio jumps to the next statement of the function it was called from.

Press **F7** to go to the next statement. If you click **Continue**, PL/SQL code execution will continue.

An example is as follows:

When entering line 8, enter **m := F3_TEST();**. That is, go to line 9 in **f3_test()**. You can step through all the statements in **f3_test()** by pressing **F7** repeatedly to step into each line. Once you have stepped through all the statements in that function, Data Studio jumps to line 10 in **f2_test()**.

The currently debugged object is marked with the |image4| symbol in the tab title, which indicates the function name.

|image5|

Stepping Over
-------------

Stepping over is the same as Stepping into, except that when it reaches a call for another function, it will not step into the function. The function will run, and you will be brought to the next statement in the current function. **F8** is the shortcut key for **Step Over**. However, if there is a breakpoint set inside the called function, **Step Over** will enter the function, and hit the set breakpoint.

In the following example, when you click **Step Over** in line 10, Data Studio executes the **f3_test()** function.

|image6|

The cursor will be moved to the next statement in **f2_test()**, that is, line 11 in **f2_test()**.

|image7|

You can step over a function when you are familiar with the way the function works and ensure that its execution will not affect the debugging.

.. note::

   Stepping over a line of code that does not contain a function call executes the line just like stepping into the line.

Stepping Out
------------

Stepping out of a sub-program continues execution of the function and then suspends execution after the function returns to its calling function. You can step out of a long function when you have determined that the rest of the function is not significant to debug. However, if a breakpoint is set in the remaining part of the function, then that breakpoint will be hit before returning to the calling function.

A function will be executed when you step over or step out of it. **Shift+F7** is the shortcut key for **Step Out**.

|image8|

In the preceding example:

-  Choose **Debug** > **Step Into** to step into **f3_test()**.
-  Choose **Debug** > **Step Out** to step out of **f3_test()**.

.. _en-us_topic_0000001098993176__en-us_topic_0185264594_ref471314683:

Continuing the Debugging
------------------------

When the debugged process stops at a specific location, you can select **Continue** (**F9**) from the **Debug** menu, or click |image9| in the toolbar to continue the PL/SQL function execution.

.. _en-us_topic_0000001098993176__en-us_topic_0185264594_viewcallstack:

Viewing Callstack
-----------------

The **Callstack** pane displays the chain of functions as they are called. The **Callstack** pane can be opened from the minimized window. The most recent functions are listed at the top, and the least recent at the bottom. At the end of each function name is the current line number in that function.

You can double-click the function names in the **Callstack** pane to open panes of different functions. For example, when **f2_test()** calls line 10 of **f3_test()**, the debugging pointer will point to the first executable line (that is, line 9 in the preceding example) in the function call.

In this case, the **Callstack** pane will be displayed as follows.

|image10|

.. note::

   Press **Alt+J** to copy the content in the **Callstack** pane.

.. |image1| image:: /_static/images/en-us_image_0000001098833258.jpg
.. |image2| image:: /_static/images/en-us_image_0000001098993394.jpg
.. |image3| image:: /_static/images/en-us_image_0000001145513399.jpg
.. |image4| image:: /_static/images/en-us_image_0000001145833249.jpg
.. |image5| image:: /_static/images/en-us_image_0000001098993258.jpg
.. |image6| image:: /_static/images/en-us_image_0000001098673432.jpg
.. |image7| image:: /_static/images/en-us_image_0000001145713179.jpg
.. |image8| image:: /_static/images/en-us_image_0000001145713175.jpg
.. |image9| image:: /_static/images/en-us_image_0000001098833260.jpg
.. |image10| image:: /_static/images/en-us_image_0000001145513261.jpg
