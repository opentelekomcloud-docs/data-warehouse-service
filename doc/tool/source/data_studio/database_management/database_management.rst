:original_name: DWS_DS_013.html

.. _DWS_DS_013:

Database Management
===================

A relational database contains a set of tables that can be operated based on the relational model of data. A relational database contains a group of data objects for storing, managing, and accessing data objects, including tables, views, indexes, and functions.

Creating a Database
-------------------

#. In the **Object Browser** pane, right-click the **Databases** group and select **Create Database**.

   |image1|

#. A **Create Database** dialog box is displayed, prompting you to provide the information required for creating a database.

   -  Enter a database name.
   -  Select the required type of encoding character set from the **Database Encoding** drop-down list. The database supports **UTF-8**, **GBK**, **SQL_ASCII**, and **LATIN1** encoding character sets. Creating the database with other encoding character sets may result in erroneous operations.

      .. note::

         This operation can be performed only when there is at least one active database.

#. Select **Connect to the DB** and click **OK**.

   The status bar displays the status of the completed operation.

   You can view the created database in the **Object Browser**. The system-related schema in the server is automatically added to the new database.

Connecting to the Database
--------------------------

In the **Object Browser** pane, right-click the database name and choose **Connect to DB**. The status bar displays the status of the completed operation.

.. note::

   This operation can be performed on a disconnected database.

Renames a Database
------------------

#. In the **Object Browser** pane, right-click the selected database and select **Rename Database**.

   .. note::

      This operation can be performed on a disconnected database.

   The **Rename Database** dialog box is displayed, prompting you to provide the information required for renaming the database.

#. Enter a new database name. Select the **Connect to the DB** check box and click **OK**.

   A confirmation dialog box is displayed, promoting you to confirm the renaming operation.

Disconnecting from a Database
-----------------------------

You can disconnect all databases or a specified database under a connection by using the disconnection function.

#. In the **Object Browser** pane, right-click the selected the **Databases** group and select **Disconnect All**. This will disconnect all the databases under the connection.

   |image2|

   .. note::

      This operation can be performed only when there is at least one active database.

   A confirmation dialog box is displayed, prompting you to confirm the disconnection operation.

#. In the **Object Browser** pane, right-click the selected database name and select **Disconnect from DB**.

   |image3|

   .. note::

      This operation can be performed only on the primary database.

Dropping a Database
-------------------

In the **Object Browser** pane, right-click the selected database and select **Drop Database**. A confirmation dialog box is displayed, asking you whether to delete the database.

|image4|

.. note::

   This operation can be performed on a disconnected database.

Viewing Data Properties
-----------------------

Right-click the selected database and select **Properties**. The status bar displays the status of the completed operation.

|image5|

The properties of the selected database are displayed.

|image6|

.. note::

   -  This operation can be performed only when there is at least one active database.
   -  If the property of an opened database is modified, then refresh and open the properties of the database again to view the updated information on the same opened window.

.. |image1| image:: /_static/images/en-us_image_0000001860319089.png
.. |image2| image:: /_static/images/en-us_image_0000001860199253.png
.. |image3| image:: /_static/images/en-us_image_0000001813439392.png
.. |image4| image:: /_static/images/en-us_image_0000001813599180.png
.. |image5| image:: /_static/images/en-us_image_0000001813599176.png
.. |image6| image:: /_static/images/en-us_image_0000001860199237.png
