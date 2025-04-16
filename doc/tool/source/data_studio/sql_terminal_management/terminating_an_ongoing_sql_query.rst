:original_name: DWS_DS_030.html

.. _DWS_DS_030:

Terminating an Ongoing SQL Query
================================

You can terminate an ongoing SQL query on the **SQL Terminal** tab page of Data Studio.

To terminate an ongoing SQL query, perform the following steps:

#. Execute a SQL query on the **SQL Terminal** tab page.

#. Click |image1| or press **Shift**\ +\ **Esc**.

   You can also choose **Run** > **Cancel**, or right-click in the code area and choose **Cancel**, or click **Cancel** on the **Progress View** tab page.

After you terminate an ongoing query, the SQL statement being executed is stopped.

Database changes made by the canceled query are rolled back and the following queries will not be executed.

A query cannot be canceled and the **Result** tab displays the result when:

#. The query has been executed and the result is being generated.
#. The result of the executed query is being transferred from the server to the Data Studio client.

A query cannot be canceled when you are viewing the execution plan of a query. For details, see :ref:`Viewing the Execution Plan and Costs <dws_ds_033>`.

The **Messages** tab displays a message indicating that the query has been cancelled.

.. note::

   The **Cancel** option is enabled only when a query is being executed.

.. |image1| image:: /_static/images/en-us_image_0000001860199265.jpg
