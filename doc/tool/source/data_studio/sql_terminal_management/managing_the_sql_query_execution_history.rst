:original_name: DWS_DS_027.html

.. _DWS_DS_027:

Managing the SQL Query Execution History
========================================

Data Studio allows viewing and managing frequently used SQL queries. The history of executed SQL queries is maintained only for the **SQL Terminal**.

#. On the **SQL Terminal** tab page, click **SQL History.** The **SQL History** dialog box is displayed.

   |image1|

.. note::

   SQL history scripts are not encrypted.

The number of queries saved in the **SQL History** dialog box is based on the value defined in **Preferences > Editor > SQL** **History** pane. Refer to the :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>` section to modify the SQL History count. Data Studio overwrites the older queries into the SQL history after the list is full. The executed query is automatically stored in the list.

The **SQL History** dialog box has the following columns:

-  **Pin Status** - Displays the pinned status of the queries. Pinned queries will always show on the top and it will not be deleted from the history even when the list is full.
-  **SQL Statement** - Displays the SQL query. The number of characters for an SQL query displayed in the **SQL Statement** column is based on the number defined in **Preferences > Editor > SQL** **History** pane. Refer to the :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>` section to modify the number of characters for a query.
-  **Number of Records** - Displays the number of records fetched by the SQL query.
-  **Start Time** - Displays the time the query execution was started.
-  **Execution Time** - Displays the time taken to execute the query.
-  **Database Name** - Displays the name of the database.
-  **Execution Status** - Displays the execution status of the query as **Success** or **Failure**.

Deleting the connection profile deletes the history. If the **SQL History** dialog box is closed, the query is not removed from the list.

You can perform the following operations in the **SQL History** dialog box:

-  :ref:`Loading an SQL Query Into the SQL Terminal <en-us_topic_0000001813598768__en-us_topic_0185264358_ref471314809>`
-  :ref:`Loading Multiple SQL Queries Into the SQL Terminal <en-us_topic_0000001813598768__en-us_topic_0185264358_section29561677225829>`
-  :ref:`Deleting an SQL Query <en-us_topic_0000001813598768__en-us_topic_0185264358_section41363205225852>`
-  :ref:`Deleting all SQL Queries <en-us_topic_0000001813598768__en-us_topic_0185264358_section3787436422590>`
-  :ref:`Pinning an SQL Query <en-us_topic_0000001813598768__en-us_topic_0185264358_ref471314785>`
-  :ref:`Unpinning an SQL Query <en-us_topic_0000001813598768__en-us_topic_0185264358_section62690964225923>`

.. _en-us_topic_0000001813598768__en-us_topic_0185264358_ref471314809:

Loading an SQL Query Into the SQL Terminal
------------------------------------------

Follow the steps to load the SQL query into the SQL terminal:

#. Select the required query and click |image2|.

   The query is appended to the cursor position in the **SQL Terminal**.

.. _en-us_topic_0000001813598768__en-us_topic_0185264358_section29561677225829:

Loading Multiple SQL Queries Into the SQL Terminal
--------------------------------------------------

The **Load in SQL** **Terminal and close History** button loads selected queries into the **SQL Terminal** and closes the **SQL History** dialog box.

Follow the steps to load selected SQL queries into the SQL terminal:

#. Select the required queries.

#. Click |image3|.

   The queries are appended to the cursor position in the **SQL Terminal**.

.. note::

   If you continue the execution on error, then each statement in the terminal will be running as a scheduled job and runs one after the other. The execution status is updated on the console and job progress is displayed. When the time difference between Job Execution, Progress Bar Update and Console Update is very minimal, you will not be able to open the progress bar and stop the execution. In such scenarios you have to close the SQL terminal to the terminate execution.

Loading More Records
--------------------

Regarding to load more data of result tab, you have to scroll down to bottom in order to load more data, which is inconvenient in some use cases. Currently, DS supports a loading more record button which makes it easier to trigger the loading more data action.

Follow the steps to load more records

#. Select the required queries and click |image4|.

   List all the required records.

.. _en-us_topic_0000001813598768__en-us_topic_0185264358_section41363205225852:

Deleting an SQL Query
---------------------

Follow the steps to delete a SQL query from the SQL history list:

#. Select the required query and click |image5|.

   A confirmation pop up window is displayed.

#. Click **OK**.

.. _en-us_topic_0000001813598768__en-us_topic_0185264358_section3787436422590:

Deleting all SQL Queries
------------------------

Follow the steps to delete all SQL queries from the SQL History list:

#. Click |image6|.

   A confirmation pop up window is displayed.

#. Click **OK**.

.. _en-us_topic_0000001813598768__en-us_topic_0185264358_ref471314785:

Pinning an SQL Query
--------------------

You can pin queries that you do not want Data Studio to delete automatically from the **SQL History**. You can pin a maximum of 50 queries. Pinned queries are displayed at the top of the list. The value set in SQL history count does not affect the pinned queries. Refer to :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>` for additional information on SQL history count.

.. note::

   The pinned queries appear on top once the **SQL History** window is closed and re-opened.

Follow the steps to pin a SQL query:

#. Select the required SQL query and click |image7|.

   The **Pin Status** column displays the pinned status of the query.

.. _en-us_topic_0000001813598768__en-us_topic_0185264358_section62690964225923:

Unpinning an SQL Query
----------------------

Follow the steps to unpin a SQL query:

#. Select the required SQL query and click |image8|.

   The **Pin Status** column displays the unpinned status of the query.

.. |image1| image:: /_static/images/en-us_image_0000001813599184.png
.. |image2| image:: /_static/images/en-us_image_0000001860319669.jpg
.. |image3| image:: /_static/images/en-us_image_0000001813599736.jpg
.. |image4| image:: /_static/images/en-us_image_0000001813439396.png
.. |image5| image:: /_static/images/en-us_image_0000001813599732.jpg
.. |image6| image:: /_static/images/en-us_image_0000001860199813.jpg
.. |image7| image:: /_static/images/en-us_image_0000001813439956.jpg
.. |image8| image:: /_static/images/en-us_image_0000001813439952.jpg
