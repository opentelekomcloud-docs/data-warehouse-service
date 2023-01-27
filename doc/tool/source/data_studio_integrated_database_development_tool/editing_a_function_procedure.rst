:original_name: DWS_DS_58.html

.. _DWS_DS_58:

Editing a Function/Procedure
============================

Perform the following steps to edit a function/procedure or SQL function:

#. In the **Object Browser** pane, double-click or right-click the required function/procedure or SQL function and select **View Source**. You must refresh **Object Browser** to view the latest DDL.

   The selected function/procedure or SQL function is displayed in the **PL/SQL Viewer** tab page.

   |image1|

   If multiple functions/procedures or SQL functions have the same schema, name, and input parameters, only one of them can be opened at a time.

#. After editing or updating, compile and execute the PL/SQL program or SQL function. For details, see :ref:`Executing a Function/Procedure <dws_ds_67>`.

   If you execute the function/procedure or SQL function before compilation, the **Source Code Change** dialog box is displayed.

#. Click **Yes** to compile and execute the PL/SQL function/procedure.

   The status of the completed operation is displayed in the **Message** tab page.

   Refer to :ref:`Executing SQL Queries <en-us_topic_0000001145833051__en-us_topic_0185264856_section16147111413113>` for information on reconnect option in case connection is lost during execution.

#. After compiling the function/procedure or SQL function, press **F5** to refresh **Object Browser** to view the updated code.

.. |image1| image:: /_static/images/en-us_image_0000001098993232.jpg
