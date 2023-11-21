:original_name: DWS_DS_58.html

.. _DWS_DS_58:

Editing a Function/Procedure
============================

Follow the steps below to open and edit the function/procedure or SQL function:

#. In the **Object Browser** pane, double-click the required function/procedure or SQL function or right-click the function/procedure or SQL function and select **View Source**. You must refresh the **Object Browser** to view the latest DDL.

   The function/procedure or SQL function based on your selection is displayed.

   |image1|

   Only one function/procedure or SQL function with the same schema, name, and input parameters can be opened in Data Studio.

#. After editing or updating, compile and execute the PL/SQL program or SQL function. For more details, refer to :ref:`Executing a Function/Procedure <dws_ds_67>`.

   If you execute the function/procedure or SQL function before compiling, the **Source Code Change** dialog box is displayed.

#. Click **Yes** to compile and execute the function/procedure.

   The status of the completed operation is displayed on the **Message** tab page.

   Refer to the :ref:`Execute SQL Queries <en-us_topic_0000001234200573__en-us_topic_0185264856_section16147111413113>` for information on reconnect option in case connection is lost during execution.

#. After compiling the function/procedure or SQL function, refresh the **Object Browser** (using **F5**) to view the updated code.

.. |image1| image:: /_static/images/en-us_image_0000001188202654.jpg
