:original_name: DWS_DS_120.html

.. _DWS_DS_120:

Managing the History of Executed SQL Queries
============================================

Data Studio allows viewing and managing frequently executed SQL queries. The history of executed SQL queries is saved only in **SQL Terminal**.

Perform the following steps to view the history of executed SQL queries:

#. Click |image1| in **SQL Terminal**.

   The **SQL History** dialog box is displayed.

.. note::

   The scripts of historical SQL queries are not encrypted.

The number of queries displayed in the **SQL History** dialog box depends on the value set in **Preferences** > **Editor** > **SQL History**. For details about setting the value, see :ref:`SQL History <en-us_topic_0000001145913107__en-us_topic_0185264581_section1382623812471>`. Data Studio overwrites the older queries into the SQL history after the list is full. The executed queries are automatically stored in the list.

The **SQL History** dialog box contains the following columns:

-  **Pin Status**: displays whether a query remains on the top. Pinned queries remain on the top and will not be deleted from the history even when the list is full.
-  **SQL Statement**: displays the SQL query. The maximum number of characters for a SQL query displayed in the **SQL Statement** column depends on the value set in **Preferences** > **Editor** > **SQL History**. For details about modifying the value, see :ref:`SQL History <en-us_topic_0000001145913107__en-us_topic_0185264581_section1382623812471>`.
-  **Number of Records**: displays the number of records obtained by the SQL query
-  **Start Time**: displays the time when the query was executed
-  **Execution Time**: displays the duration of the query execution
-  **Database Name**: displays the name of the database
-  **Execution Status**: displays the status of the executed query, which can be **Success** or **Failure**

The connection information is deleted together with the query history. If the **SQL History** dialog box is closed, the query is not removed from the list.

You can perform the following operations in the **SQL History** dialog box:

-  :ref:`Loading a SQL Query into the SQL Terminal Pane <en-us_topic_0000001145833043__en-us_topic_0185264358_ref471314809>`
-  :ref:`Loading Multiple SQL Queries into the SQL Terminal Pane <en-us_topic_0000001145833043__en-us_topic_0185264358_section29561677225829>`
-  :ref:`Deleting a SQL Query <en-us_topic_0000001145833043__en-us_topic_0185264358_section41363205225852>`
-  :ref:`Deleting All SQL Queries <en-us_topic_0000001145833043__en-us_topic_0185264358_section3787436422590>`
-  :ref:`Pinning a SQL Query <en-us_topic_0000001145833043__en-us_topic_0185264358_ref471314785>`
-  :ref:`Unpinning a SQL Query <en-us_topic_0000001145833043__en-us_topic_0185264358_section62690964225923>`

.. _en-us_topic_0000001145833043__en-us_topic_0185264358_ref471314809:

Loading a SQL Query into the SQL Terminal Pane
----------------------------------------------

Perform the following steps to load a SQL query into the **SQL Terminal** pane:

#. Select the required query and click |image2|.

   The query is added to the cursor position in **SQL Terminal**.

.. _en-us_topic_0000001145833043__en-us_topic_0185264358_section29561677225829:

Loading Multiple SQL Queries into the SQL Terminal Pane
-------------------------------------------------------

You can click the **Load in SQL Terminal and close History** button to load selected queries into **SQL Terminal** and close the **SQL History** dialog box.

Perform the following steps to load multiple selected SQL queries into the **SQL Terminal** pane:

#. Select the required queries.

#. Click |image3|.

   The queries are added to the cursor position in **SQL Terminal**.

.. note::

   If you continue the execution upon an error, each statement in **SQL Terminal** will be executed in sequence as a scheduled job. The execution status is updated in the console and each job is listed in the progress bar. When the time difference between **Job Execution**, **Progress Bar Update** and **Console Update** is small, you will not be able to stop the execution by opening the progress bar. In this case, you need to close **SQL Terminal** to stop the execution.

Loading More Records
--------------------

To load more data in the **Result** tab, you need to scroll down to bottom, which is inconvenient in some scenarios. Data Studio provides a button that simplifies the loading operation.

Perform the following steps to load more records:

#. Select the required queries and click |image4|.

   All the required records are listed.

.. _en-us_topic_0000001145833043__en-us_topic_0185264358_section41363205225852:

Deleting a SQL Query
--------------------

Perform the following steps to delete a SQL query from the **SQL History** list:

#. Select the required query and click |image5|.

   A confirmation dialog box is displayed.

#. Click **OK**.

.. _en-us_topic_0000001145833043__en-us_topic_0185264358_section3787436422590:

Deleting All SQL Queries
------------------------

Perform the following steps to delete all SQL queries from the **SQL History** list:

#. Click |image6|.

   A confirmation dialog box is displayed.

#. Click **OK**.

.. _en-us_topic_0000001145833043__en-us_topic_0185264358_ref471314785:

Pinning a SQL Query
-------------------

You can pin queries that you do not want Data Studio to delete automatically from **SQL History**. You can pin a maximum of 50 queries. Pinned queries are displayed at the top of the list. The value set in **SQL History** does not affect the pinned queries. For details, see :ref:`SQL History <en-us_topic_0000001145913107__en-us_topic_0185264581_section1382623812471>`.

.. note::

   The pinned queries are displayed at the top of the list once the **SQL History** pane is closed and opened again.

Perform the following steps to pin a SQL query:

#. Select the required SQL query and click |image7|.

   The **Pin Status** column displays the pinned status of the query.

.. _en-us_topic_0000001145833043__en-us_topic_0185264358_section62690964225923:

Unpinning a SQL Query
---------------------

Perform the following steps to unpin a SQL query:

#. Select the required SQL query and click |image8|.

   The **Pin Status** column displays the unpinned status of the query.

.. |image1| image:: /_static/images/en-us_image_0000001099153360.jpg
.. |image2| image:: /_static/images/en-us_image_0000001099153248.jpg
.. |image3| image:: /_static/images/en-us_image_0000001145713307.jpg
.. |image4| image:: /_static/images/en-us_image_0000001099153250.png
.. |image5| image:: /_static/images/en-us_image_0000001145513273.jpg
.. |image6| image:: /_static/images/en-us_image_0000001145713301.jpg
.. |image7| image:: /_static/images/en-us_image_0000001145833125.jpg
.. |image8| image:: /_static/images/en-us_image_0000001145713187.jpg
