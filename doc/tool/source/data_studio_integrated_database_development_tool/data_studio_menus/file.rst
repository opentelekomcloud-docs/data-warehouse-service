:original_name: DWS_DS_22.html

.. _DWS_DS_22:

File
====

The **File** menu contains database connection options. Click **File** in the main menu or press **Alt+F** to open the **File** menu.

+----------------------------------+-----------------+-----------------+------------------------------------------------------------------------------------+
| Function                         | Button          | Shortcut Key    | Description                                                                        |
+==================================+=================+=================+====================================================================================+
| Creating a connection            | |image1|        | Ctrl+N          | Creates a database connection in the **Object Browser** and **SQL Terminal** tabs. |
+----------------------------------+-----------------+-----------------+------------------------------------------------------------------------------------+
| Deleting a connection            | |image2|        | ``-``           | Deletes the selected database connection from **Object Browser**.                  |
+----------------------------------+-----------------+-----------------+------------------------------------------------------------------------------------+
| Opening a connection             | |image3|        | ``-``           | Connects to the database.                                                          |
+----------------------------------+-----------------+-----------------+------------------------------------------------------------------------------------+
| Disconnecting from the database  | |image4|        | Ctrl+Shift+D    | Disconnects from the specified database.                                           |
+----------------------------------+-----------------+-----------------+------------------------------------------------------------------------------------+
| Disconnecting all connections    | |image5|        | ``-``           | Disconnects all the databases of a specified connection.                           |
+----------------------------------+-----------------+-----------------+------------------------------------------------------------------------------------+
| Opening                          | |image6|        | Ctrl+O          | Loads SQL queries in **SQL Terminal**.                                             |
+----------------------------------+-----------------+-----------------+------------------------------------------------------------------------------------+
| Saving SQL scripts               | |image7|        | Ctrl+S          | Saves the SQL scripts of the **SQL Terminal** to a SQL file.                       |
+----------------------------------+-----------------+-----------------+------------------------------------------------------------------------------------+
| Saving SQL scripts to a new file | |image8|        | CTRL+ALT+S      | Saves the SQL scripts in **SQL Terminal** to a new SQL file.                       |
+----------------------------------+-----------------+-----------------+------------------------------------------------------------------------------------+
| Exiting                          | ``-``           | Alt+F4          | Exits from Data Studio and disconnects from the database.                          |
|                                  |                 |                 |                                                                                    |
|                                  |                 |                 | .. note::                                                                          |
|                                  |                 |                 |                                                                                    |
|                                  |                 |                 |    Any unsaved changes will be lost.                                               |
+----------------------------------+-----------------+-----------------+------------------------------------------------------------------------------------+

Stopping Data Studio
--------------------

Perform the following steps to stop Data Studio:

#. Click |image9|.

   Alternatively, choose **File** > **Exit**.

   The **Exit Application** dialog box is displayed prompting you to take the required actions.

#. Click the corresponding button as required.

   -  **Force Exit**: Exits the application without saving the SQL execution history.

   .. note::

      If you click **Force Exit**, the SQL execution history that is not saved may be lost.

   -  **Graceful Exit**: Saves the SQL execution history that has not been saved to the disk before exiting.
   -  **Cancel**: Cancels the exit from the application.

.. |image1| image:: /_static/images/en-us_image_0000001098673390.png
.. |image2| image:: /_static/images/en-us_image_0000001145833075.png
.. |image3| image:: /_static/images/en-us_image_0000001098993226.png
.. |image4| image:: /_static/images/en-us_image_0000001145913183.png
.. |image5| image:: /_static/images/en-us_image_0000001145513223.png
.. |image6| image:: /_static/images/en-us_image_0000001098673394.png
.. |image7| image:: /_static/images/en-us_image_0000001145913185.png
.. |image8| image:: /_static/images/en-us_image_0000001099153198.png
.. |image9| image:: /_static/images/en-us_image_0000001145713143.jpg
