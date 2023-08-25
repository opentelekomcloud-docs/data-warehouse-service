:original_name: DWS_DS_123.html

.. _DWS_DS_123:

Canceling the Execution of SQL Queries
======================================

Data Studio allows you to cancel the execution of an SQL query being executed in the **SQL Terminal**.

Follow the steps to cancel execution of an SQL query:

#. Execute the SQL query in the **SQL Terminal**.

#. Click |image1| in the **SQL Terminal** or press **Shift**\ +\ **Esc**.

   Alternatively, you can choose **Run > Cancel** from the main menu or right-click **SQL Terminal** and select **Cancel**, or select **Cancel** from **Progress View** tab.

When you cancel the query, the execution stops at the currently executing SQL statement.

Database changes made by the canceled query are rolled back and the queries following the canceled query are not executed.

A query cannot be canceled and the **Result** tab shows the result when:

#. The server has finished execution of the query and is preparing the result.
#. The result of the executed query is being transferred from the server to the Data Studio client.

A query cannot be canceled while viewing the query **Execution Plan**. For more details, refer to :ref:`Viewing the Query Execution Plan and Cost <dws_ds_126>`.

The **Messages** tab shows the query cancelation message.

.. note::

   The **Cancel** button is enabled only during query execution.

.. |image1| image:: /_static/images/en-us_image_0000001188202730.jpg
