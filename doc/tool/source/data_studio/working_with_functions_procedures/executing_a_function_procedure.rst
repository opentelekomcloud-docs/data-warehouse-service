:original_name: DWS_DS_67.html

.. _DWS_DS_67:

Executing a Function/Procedure
==============================

After you connect to the database, all the stored functions/procedures and tables will be automatically populated in the **Object Browser** pane. You can use Data Studio to execute PL/SQL programs or SQL functions.

.. note::

   -  Blank lines occurring above or below in a function/procedure will be trimmed by Data Studio before being sent to the server. Blank lines will also be trimmed when displaying the source received from the server.

   -  To execute a function/procedure, enter the same values in Data Studio and the gsql client. If you do not enter any value in Data Studio, then NULL is considered as the input.

      For example:

      - To execute the function/procedure with string, enter the value as data.

      - To execute the function/procedure with date, enter the value as **to_date('2012-10-10', 'YYYY-MM-DD')**.

   -  A function/procedure with OUT and INOUT parameter types cannot be executed directly.

   -  Data Studio will not execute any function/procedure with unknown data type parameters.

You can right-click the function/procedure in the **Object Browser** to perform the following operations:

-  Refresh the program to get the latest program from the server.
-  Execute the function/procedure or SQL function.
-  Debug a PL/SQL function.
-  Drop the debug object.

Executing a PL/SQL Program or SQL Function
------------------------------------------

Follow the steps below to execute a PL/SQL program or SQL function:

#. Double-click and open the PL/SQL program or SQL function. Each debug object will be opened in a new tab. You can open a maximum of 100 tabs in Data Studio.

#. Click |image1| on the toolbar or choose **Run** > **Execute** from the main menu,

   or right-click the program name in the **Object Browser** and select **Execute**.

#. The **Execute Function/Procedure** dialog box is displayed prompting for your input.

   .. note::

      If there is no input parameter, then the **Execute Function/Procedure** dialog box will not appear. Instead, the PL/SQL program will execute and the result (if any) will be displayed in the **Result** tab.

#. Provide your input for the function/procedure in the **Execute PL/pgSQL** dialog box and click **OK**.

   To set NULL as the parameter value, enter **NULL** or **null**.

   -  If you do not provide a value that starts with a single quote, then a single quote (') will be added by Data Studio before and after the value and typecasting is done.

   -  If you provide a value that starts with a single quote, an additional single quote will not be added by Data Studio, but data type typecasting is done. For example:

      For supported data types, the execution queries are as follows:

      ::

         select func('1'::INTEGER);
         select func('1'::FLOAT);
         select func('xyz'::VARCHAR);

   -  If quotes are already provided, you need to take care of escaping the quotes.

      Example: If the input value is **ab'c**, then you need to enter **ab''c**.

      The PL/SQL program is executed in the **SQL Terminal** tab and the result is displayed in the **Result** tab. You can copy the contents of the Result tab by clicking |image2|. Refer to :ref:`Working with SQL Terminals <dws_ds_128>` for more information on toolbar options..

      Refer to the :ref:`Execute SQL Queries <en-us_topic_0000001234200573__en-us_topic_0185264856_section16147111413113>` section for information on reconnect option in case connection is lost during execution.

.. |image1| image:: /_static/images/en-us_image_0000001290072796.png
.. |image2| image:: /_static/images/en-us_image_0000001188681104.png
